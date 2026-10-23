

```
where NEW.tag ilike 'b%'
DO INSTEAD insert into new_b_tags(pk,tag,parent)values (NEW.pk,NEW.tag,NEW.parent);
```

- 7. 现在，让我们使用与规则中相同的流程：创建一个新函数，然后由触发器触发它：

```
forumdb=> CREATE OR REPLACE FUNCTION f2_tags() RETURNS trigger as $$
BEGIN
    IF lower(substring(NEW.tag from 1 for 1)) = 'b' THEN
    insert into new_b_tags(pk,tag,parent)values (NEW.pk,NEW.tag,NEW.parent);
    RETURN NULL;
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE 'plpgsql';
CREATE FUNCTION

forumdb=> CREATE TRIGGER t2_tags BEFORE INSERT on new_tags FOR EACH ROW EXECUTE PROCEDURE f2_tags();
CREATE TRIGGER
```

- 8. 这个 lower 语句 `(substring(NEW.tag from 1 for 1)) = 'b'` 与我们最初在规则中看到的几乎相同。不同之处在于 `RETURN NULL`，这意味着如果 `NEW.tag` 的值以 `b` 开头，则会向默认操作返回一个 `NULL` 值，并且对 `new_tags` 表的 `INSERT` 将不会插入任何值。相反，如果 `IF` 条件不满足，则函数返回 `NEW`，记录会被插入到 `new_tags` 表中。

让我们看看它是否有效：

```
forumdb=> truncate new_tags;
TRUNCATE TABLE
forumdb=> truncate new_a_tags;
TRUNCATE TABLE
forumdb=> truncate new_b_tags;
TRUNCATE TABLE

forumdb=> insert into new_tags (pk,tag,parent) values (1,'bsd unix',NULL);
INSERT 0 0
```

正如我们所见，`IF` 条件生效了，结果 `INSERT 0 0` 表示没有记录被插入到 `new_tags` 表中。这是因为触发器作用于 `BEFORE INSERT` 事件，`IF` 条件将记录转移到了 `new_b_tags` 表中。

- 9. 我们现在来看看如何使用单个触发器编写整个流程。首先，让我们回到环境的初始条件。和之前一样，我们删除表中的数据，并使用 `CASCADE` 选项删除选定的触发器及其所有关联的触发器：

```
forumdb=> TRUNCATE new_tags;
TRUNCATE TABLE
forumdb=> TRUNCATE new_a_tags;
TRUNCATE TABLE
forumdb=> TRUNCATE new_b_tags;
TRUNCATE TABLE
forumdb=> DROP TRIGGER t_tags ON new_tags CASCADE;
DROP TRIGGER
forumdb=> DROP TRIGGER t2_tags ON new_tags CASCADE;
DROP TRIGGER
```

- 10. 在最后一步中，我们将把在函数 `f_tags()` 和 `f2_tags()` 中编写的内容合并到一个单独的函数 `f3_tags()` 中，该函数将在 `BEFORE INSERT` 事件中由 `t3_tags` 触发器触发：

```
forumdb=> CREATE OR REPLACE FUNCTION f3_tags() RETURNS trigger as $$
BEGIN
    IF lower(substring(NEW.tag from 1 for 1)) = 'a' THEN
        insert into new_a_tags(pk,tag,parent) values (NEW.pk,NEW.tag,NEW.parent);
        RETURN NEW;
    ELSIF lower(substring(NEW.tag from 1 for 1)) = 'b' THEN
        insert into new_b_tags(pk,tag,parent) values (NEW.pk,NEW.tag,NEW.parent);
    RETURN NULL;
    ELSE
        RETURN NEW;
    END IF;
END;
$$ 
LANGUAGE 'plpgsql';
CREATE FUNCTION

forumdb=> CREATE TRIGGER t3_tags BEFORE INSERT on new_tags FOR EACH ROW EXECUTE PROCEDURE f3_tags();
CREATE TRIGGER
```

该函数包含了前面两个函数的逻辑。这样，我们就可以通过使用单个函数和单个触发器更优雅地解决问题。让我们看看它是否有效：

```
forumdb=> insert into new_tags (pk,tag,parent) values (1,'operating systems',NULL);
INSERT 0 1

forumdb=> insert into new_tags (pk,tag,parent) values (2,'alpine linux',1);
INSERT 0 1
forumdb=> insert into new_tags (pk,tag,parent) values (3,'bsd unix',1);
INSERT 0 0

forumdb=> select * from new_tags ;
pk |      tag      | parent
----+---------------+--------
  1 | operating systems |
  2 | alpine linux  |      1
(2 rows)

forumdb=> select * from new_a_tags ;
pk |   tag       | parent
----+-------------+-------
  2 | alpine linux |     1
(1 row)

forumdb=> select * from new_b_tags ;
pk |   tag   | parent
----+---------+-------
  3 | bsd unix |     1
(1 row)
```

可以看到，该函数运行正常。

## TG_OP 变量

如官方文档 https://www.PostgreSQL.org/docs/current/plpgsql-trigger.html 所示，PostgreSQL 允许使用特殊变量来控制触发器，其中两个我们已经见过（`NEW` 变量和 `OLD` 变量）。还有另一个特殊变量叫做 `TG_OP`，它告诉我们触发器是从哪个事件触发的。`TG_OP` 变量的可能值为 `INSERT`、`DELETE`、`UPDATE` 和 `TRUNCATE`。

## UPDATE / DELETE 触发器

现在，让我们回到图 8.1 中使用的示例。我们想要实现的目标是创建一个能够处理 `INSERT`、`DELETE` 和 `UPDATE` 事件的单一函数。首先，让我们回到环境的初始条件：

```
forumdb=> truncate new_tags;
TRUNCATE TABLE
forumdb=> truncate new_a_tags;
TRUNCATE TABLE
forumdb=> truncate new_b_tags;
TRUNCATE TABLE
forumdb=> drop trigger t3_tags on new_tags cascade;
DROP TRIGGER
```

现在，和之前一样，我们将逐步进行。第一步是编写将在 `INSERT` 事件期间执行的代码部分。然后，我们将看到如何扩展该函数以管理 `DELETE` 和 `UPDATE` 事件。

将处理所有这三个事件的函数将是 `fcopy_tags()` 函数；此函数将由 `tcopy_tags` 触发器调用。`fcopy_tags()` 函数使用 `TG_OP` 变量，将能够区分 `INSERT`、`UPDATE` 和 `DELETE` 事件。

让我们首先编写 `fcopy_tags()` 函数来处理 `INSERT` 事件：

```
forumdb=> CREATE OR REPLACE FUNCTION fcopy_tags() RETURNS trigger as $$
BEGIN
IF TG_OP = 'INSERT' THEN
    IF lower(substring(NEW.tag from 1 for 1)) = 'a' THEN
        insert into new_a_tags(pk,tag,parent)values (NEW.pk,NEW.tag,NEW.parent);
    ELSIF lower(substring(NEW.tag from 1 for 1)) = 'b' THEN
        insert into new_b_tags(pk,tag,parent)values (NEW.pk,NEW.tag,NEW.parent);
    END IF;
    RETURN NEW;
END IF;
END;
$$ LANGUAGE 'plpgsql';
CREATE FUNCTION

forumdb=> CREATE TRIGGER tcopy_tags_ins BEFORE INSERT on new_tags FOR EACH ROW EXECUTE PROCEDURE fcopy_tags();
CREATE TRIGGER
```

现在让我们看看，对于 `INSERT` 事件，这段代码是否有效：

```
forumdb=> insert into new_tags (pk,tag,parent) values (1,'operating systems',NULL);
INSERT 0 1
forumdb=> insert into new_tags (pk,tag,parent) values (2,'alpine linux',1);
INSERT 0 1
forumdb=> insert into new_tags (pk,tag,parent) values (3,'bsd unix',1);
INSERT 0 1

forumdb=> select * from new_a_tags ;
pk |     tag     | parent
----+--------------+-------
2  | alpine linux |     1
(1 row)

forumdb=> select * from new_b_tags ;
pk |     tag     | parent
----+--------------+-------
3  | bsd unix |     1
(1 row)

forumdb=> select * from new_tags ;
pk |          tag          | parent
----+----------------------+--------
1  | operating systems |
2  | alpine linux |     1
3  | bsd unix |     1
(3 rows)
```

很明显，它有效！

接下来，让我们处理 `DELETE` 事件。我们需要做的事情如下：

- 向函数添加一些代码行来管理 `DELETE` 操作。
- 添加一个能够处理 `DELETE` 事件的新触发器。

该函数变为如下形式：

```
forumdb=> CREATE OR REPLACE FUNCTION fcopy_tags() RETURNS trigger as $$
BEGIN
IF TG_OP = 'INSERT' THEN
    IF lower(substring(NEW.tag from 1 for 1)) = 'a' THEN
        insert into new_a_tags(pk,tag,parent)values (NEW.pk,NEW.tag,NEW.parent);
    ELSIF lower(substring(NEW.tag from 1 for 1)) = 'b' THEN
        insert into new_b_tags(pk,tag,parent)values (NEW.pk,NEW.tag,NEW.parent);
    END IF;
    RETURN NEW;
END IF;
IF TG_OP = 'DELETE' THEN
    IF lower(substring(OLD.tag from 1 for 1)) = 'a' THEN
        DELETE FROM new_a_tags WHERE pk = OLD.pk;
        ELSIF lower(substring(OLD.tag from 1 for 1)) = 'b' THEN
        DELETE FROM new_b_tags WHERE pk = OLD.pk;
    END IF;
    RETURN OLD;
END IF;
END;
$$ 
LANGUAGE 'plpgsql';
CREATE FUNCTION
```

添加了这段代码：

```
IF TG_OP = 'DELETE' THEN
    IF lower(substring(OLD.tag from 1 for 1)) = 'a' THEN
        DELETE FROM new_a_tags WHERE pk = OLD.pk;
    ELSIF lower(substring(OLD.tag from 1 for 1)) = 'b' THEN
        DELETE FROM new_b_tags WHERE pk = OLD.pk;
    END IF;
    RETURN OLD;
END IF;
```

如果待删除的记录以字母 `a` 或 `b` 开头，这段代码会删除 `new_a_tags` 和 `new_b_tags` 表中的数据。现在我们必须创建一个能够处理 `DELETE` 事件的新触发器：

```
forumdb=> CREATE TRIGGER tcopy_tags_del
AFTER DELETE on new_tags FOR EACH ROW EXECUTE PROCEDURE fcopy_tags();
CREATE TRIGGER
```

该触发器在 `AFTER DELETE` 时执行；在这种情况下，我们创建的 `TRIGGER` 是 `BEFORE` 还是 `AFTER INSERT` 函数都没有什么区别。让我们看看这个针对 `DELETE` 事件的触发器是否有效：

```
forumdb=> delete from new_tags where pk=2;
DELETE 1
forumdb=> delete from new_tags where pk=3;
DELETE 1
```

