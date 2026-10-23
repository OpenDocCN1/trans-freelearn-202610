

一个 VOLATILE 函数可以做任何事情，包括修改数据库。它在相同参数的连续调用中可以返回不同的结果。优化器对这类函数的行为不做任何假设。使用易变函数的查询会在每次需要其值时在每一行重新计算该函数。如果一个函数被标记为 VOLATILE，那么如果我们使用相同的输入参数多次调用它，它可能会返回不同的结果。

一个 STABLE 函数不能修改数据库，并且保证在单个语句中所有行内给定相同参数时返回相同的结果。此类别允许优化器将函数的多次调用优化为一次调用。特别是，在索引扫描条件中使用包含此类函数的表达式是安全的。如果一个函数被标记为 STABLE，则在同一事务内给定相同参数时，该函数将返回相同的结果。

一个 IMMUTABLE 函数不能修改数据库，并且保证在给定相同参数时永远返回相同的结果。此类别允许优化器在查询使用常量参数调用函数时预先计算该函数。

在本章的后续页面中，我们将只关注易变函数的示例；然而，在这里我们将简要查看一个稳定函数的示例和一个不可变函数的示例：

- 1. 让我们从一个稳定函数开始——例如，now() 函数就是一个稳定函数。now() 函数返回我们在事务开始时拥有的当前日期和时间，如下所示：

```
forumdb=> begin ;
BEGIN

forumdb=* > select now();
    now
----------------------------
2023-03-17 13:25:25.37224+00
(1 row)

forumdb=* > select now();
        now
-----------------------------
2023-03-17 13:25:25.37224+00
(1 row)

forumdb=* > commit;
COMMIT

forumdb=> begin ;
BEGIN

forumdb=* > select now();
        now
-----------------------------
2023-03-17 13:27:02.012632+00
(1 row)

forumdb=* > commit ;
COMMIT
```

注意：在 PostgreSQL 16 中，当 psql 显示像 *> 这样的提示时，这意味着我们处于一个事务块中。

- 2. 现在，让我们看一个不可变函数——例如，lower(string_expression) 函数。lower 函数接受一个字符串并将其转换为小写格式。如我们所见，如果输入参数相同，则 lower 函数始终返回相同的结果，即使在不同的事务中执行：

```
forumdb=> begin;
BEGIN

forumdb=* > select now();
now
-----------------------------
2023-03-17 13:33:39.586388+00
(1 row)

forumdb=* > select lower('MICKY MOUSE');
    lower
----------------
micky mouse
(1 row)

forumdb=* > commit;
COMMIT

forumdb=> begin;
BEGIN

forumdb=* > select now();
       now
-----------------------------
2023-03-17 13:34:56.491773+00
(1 row)

forumdb=* > select lower('MICKY MOUSE');
    lower
----------------
micky mouse
(1 row)

forumdb=* > commit;
COMMIT
```

## 控制结构

PL/pgSQL 具有管理以下控制结构的能力：

- 条件语句
- 循环语句
- 异常处理语句

## 条件语句

PL/pgSQL 语言可以管理 IF 类型条件语句和 CASE 类型条件语句。

### IF 语句

在 PL/pgSQL 中，IF 语句的语法如下：

```
IF boolean-expression THEN
  statements
[ ELSIF boolean-expression THEN
  statements
[ ELSIF boolean-expression THEN
  statements
  ...
]
]
[ ELSE
  statements ]
END IF;
```

例如，假设我们想编写一个函数，当给定两个输入值 x 和 y 时，返回以下内容：

- *第一个参数大于第二个参数* 如果 x > y
- *第二个参数大于第一个参数* 如果 x < y
- *两个参数相等* 如果 x = y

我们必须编写以下函数：

```
forumdb=> CREATE OR REPLACE FUNCTION my_check(x integer default 0, y integer default 0) RETURNS text AS
$BODY$
BEGIN
  IF x > y THEN
  return 'first parameter is greater than second parameter';
ELSIF x < y THEN
return 'second parameter is greater than first parameter';
ELSE
return 'the 2 parameters are equals';
END IF;
END;
$BODY$
language 'plpgsql';
CREATE FUNCTION
```

在这个例子中，我们看到了 IF 构造的最大形式：IF [...] THEN[...] ELSIF [...] ELSE[...] END IF；

然而，更短的形式也存在，如下所示：

- IF [...] THEN[...] ELSE[...] END IF;
- IF [...] THEN[...] END IF;

之前定义的函数提供的一些结果示例如下：

```
forumdb=> select my_check(1,2);
                my_check
---------------------------------------------
second parameter is higher than first parameter
(1 row)

forumdb=> select my_check(2,1);
                my_check
---------------------------------------------
first parameter is higher than second parameter
(1 row)

forumdb=> select my_check(1,1);
                my_check
---------------------------------------------
the 2 parameters are equals
(1 row)
```

### CASE 语句

在 PL/pgSQL 中，也可以使用 CASE 语句。CASE 语句可以有以下两种语法。

以下是简单 CASE 语句：

```
CASE search-expression
  WHEN expression [, expression [ ... ]] THEN
  statements
  [ WHEN expression [, expression [ ... ]] THEN
  statements
  ... ]
  [ ELSE
  statements ]
END CASE;
```

以下是搜索 CASE 语句：

```
CASE
  WHEN boolean-expression THEN
  statements
  [ WHEN boolean-expression THEN
  statements
  ... ]
  [ ELSE
  statements ]
END CASE;
```

现在，我们将执行以下操作：

- 我们将使用第一个，即简单 CASE 语法，如果我们需要从一组值中做出选择。
- 我们将使用第二个，当我们需要从一系列值中选择时。

让我们从第一个语法开始：

```
forumdb=> CREATE OR REPLACE FUNCTION my_check_value(x integer default 0)
RETURNS text AS
$BODY$
BEGIN
CASE x
WHEN 1 THEN return 'value = 1';
WHEN 2 THEN return 'value = 2';
ELSE return 'value >= 3 ';
END CASE;
END;
$BODY$
language 'plpgsql';
CREATE FUNCTION
```

前面的 my_check_value 函数返回以下内容：

- value = 1 如果 x = 1
- value = 2 如果 x = 2
- value >= 3 如果 x >= 3

我们可以在这里看到这是正确的：

```
forumdb=> select my_check_value(1);
my_check_value
----------------
value = 1
(1 row)

forumdb=> select my_check_value(2);
my_check_value
----------------
value = 2
(1 row)

forumdb=> select my_check_value(3);
my_check_value
----------------
value >= 3
(1 row)
```

现在，让我们看一个搜索 CASE 语法的示例：

```
forumdb=> CREATE OR REPLACE FUNCTION my_check_case(x integer default 0, y integer default 0) RETURNS text AS
$BODY$
BEGIN
    CASE
        WHEN x > y THEN return 'first parameter is higher than second parameter';
        WHEN x < y THEN return 'second parameter is higher than first parameter';
        ELSE return 'the 2 parameters are equals';
    END CASE;
END;
$BODY$
    language 'plpgsql';
CREATE FUNCTION
```

my_check_case 函数返回的数据与我们之前编写的 my_check 函数相同：

```
forumdb=> select my_check_case(2,1);
                my_check_case
--------------------------------------------------
 first parameter is higher than second parameter
(1 row)

forumdb=> select my_check_case(1,2);
                my_check_case
--------------------------------------------------
 second parameter is higher than first parameter
(1 row)

forumdb=> select my_check_case(1,1);
                my_check_case
-----------------------------
 the 2 parameters are equals
(1 row)

forumdb=> select my_check_case();
                my_check_case
-----------------------------
```

```
for rw in select * from posts where pk=p_id loop
    ret.id := rw.pk;
    ret.title := rw.title;
    ret.record_data := hstore(ARRAY['title',rw.title,'Title and Content',format('%s %s',rw.title,rw.content)]);
    return next ret;
    end loop;
    return;
END;
$$
language 'plpgsql';
CREATE FUNCTION
```

正如我们所见，这几行 PL/pgSQL 代码中集中了许多内容：

- 1. `rw posts%ROWTYPE`：通过此语句，rw 变量被定义为 posts 表的一行容器的容器。
- 2. `for rw in select * from posts where pk=p_id loop`：通过此语句，我们在选择结果中循环，每次将 select 命令返回的值分配给 rw 变量。接下来的三个步骤将值分配给 ret 变量。
- 3. `return next ret;`：此语句返回 ret 变量的值并转到 for 循环的下一条记录。
- 4. `end loop;`：此语句告诉 PostgreSQL for 循环在此结束。
- 5. `return;`：这是函数的返回指令。

需要记住的重要一点是，PL/pgSQL 语言位于 PostgreSQL 事务系统内部。这意味着函数是原子执行的，并且函数不是在执行 RETURN NEXT 命令时返回结果，而是在执行放置在函数末尾的 RETURN 命令时返回结果。这可能意味着对于非常大的数据集，PL/pgSQL 函数在返回结果之前可能需要很长时间。

## 记录类型

在我们之前使用过的一个示例中，我们引入了 %ROWTYPE 数据类型。在 PL/pgSQL 语言中，可以将此概念推广。有一种名为 record 的数据类型，它推广了 %ROWTYPE 的概念。

