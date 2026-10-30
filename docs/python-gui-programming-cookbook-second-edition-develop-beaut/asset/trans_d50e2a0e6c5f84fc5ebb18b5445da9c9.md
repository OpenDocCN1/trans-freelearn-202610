

我们使用主外键关系来连接这两个表的数据。

我们将在接下来的两个菜谱中详细介绍其工作原理，在那里我们将修改和删除 MySQL 数据库中的数据。

## 准备工作

本菜谱基于上一菜谱 *Designing the Python GUI database* 中创建的 MySQL 数据库，并向你展示如何删除并重新创建 GuiDB。

删除数据库当然会删除数据库表中所有的数据，因此我们也会向你展示如何重新插入这些数据。

## 如何操作...

我们 `GUI_MySQL_class.py` 模块的完整代码位于本章的代码文件夹中，可从 https://github.com/PacktPublishing/Python-GUI-Programming-Cookbook-Second-Edition 下载。它创建数据库，向其中添加表，然后向我们创建的两个表中插入数据。

这里，我们将概述代码而不展示所有实现细节以节省篇幅，因为展示完整代码需要太多页数：

```
import MySQLdb as mysql
import Ch07_Code.GuiDBConfig as guiConf

class MySQL():
    # class variable
    GUIDB = 'GuiDB'
    #------------------------------------------------------------
    def connect(self):
        # connect by unpacking dictionary credentials
        conn = mysql.connector.connect(**guiConf.dbConfig)
        # create cursor
        cursor = conn.cursor()
        return conn, cursor
    #------------------------------------------------------------
```

```
def close(self, cursor, conn):
    # close cursor
#-------------------------------------------------------------------------------
def showDBs(self):
    # connect to MySQL
#-------------------------------------------------------------------------------
def createGuiDB(self):
    # connect to MySQL
#-------------------------------------------------------------------------------
def dropGuiDB(self):
    # connect to MySQL
#-------------------------------------------------------------------------------
def useGuiDB(self, cursor):
    '''Expects open connection.'''
    # select DB
#-------------------------------------------------------------------------------
def createTables(self):
    # connect to MySQL
    # create Table inside DB
#-------------------------------------------------------------------------------
def dropTables(self):
    # connect to MySQL
#-------------------------------------------------------------------------------
def showTables(self):
    # connect to MySQL
#-------------------------------------------------------------------------------
def insertBooks(self, title, page, bookQuote):
    # connect to MySQL
    # insert data
#-------------------------------------------------------------------------------
def insertBooksExample(self):
    # connect to MySQL
    # insert hard-coded data
#-------------------------------------------------------------------------------
def showBooks(self):
    # connect to MySQL
#-------------------------------------------------------------------------------
def showColumns(self):
    # connect to MySQL
#-------------------------------------------------------------------------------
def showData(self):
    # connect to MySQL
#-------------------------------------------------------------------------------
if __name__ == '__main__':
    # Create class instance
    mySQL = MySQL()
```

运行上述代码会在我们创建的数据库中创建以下表和数据：

```
mysql> USE guidb
Database changed
mysql> SELECT * FROM books;
+---------+----------------------------------+----------+
| Book_ID | Book_Title                       | Book_Page |
+---------+----------------------------------+----------+
|       1 | Design Patterns                  |        7 |
|       2 | xUnit Test Patterns              |       31 |
+---------+----------------------------------+----------+
2 rows in set (0.10 sec)

mysql> SELECT * FROM quotations;
+----------+-----------------------------------------------+---------------+
| Quote_ID | Quotation                                     | Books_Book_ID |
+----------+-----------------------------------------------+---------------+
|        1 | Programming to an Interface, not an Implementation |             1 |
|        2 | Philosophy of Test Automation                 |             2 |
+----------+-----------------------------------------------+---------------+
2 rows in set (0.00 sec)

mysql>
```

## 工作原理...

我们创建了一个 MySQL 数据库，连接到它，然后创建了两个表来保存最喜欢的书籍或期刊引用的数据。

我们将数据分布在两个表之间，因为引文往往比较大，而书籍标题和书籍页码则非常短。通过这样做，我们可以提高数据库的效率。

在 SQL 数据库语言中，将数据分离到不同的表中称为规范化。

## 使用 SQL UPDATE 命令

本菜谱将使用上一菜谱 Using the SQL INSERT command 中的代码，详细解释它，然后扩展代码以更新我们的数据。

## 准备工作

本菜谱基于上一菜谱 Using the SQL INSERT command，因此请阅读并研究上一菜谱，以便跟随本菜谱中的编码，在这里我们修改现有数据。

## 如何操作...

首先，我们将通过运行以下 Python 到 MySQL 命令来显示要修改的数据：

```
import MySQLdb as mysql
import Ch07_Code.GuiDBConfig as guiConf

class MySQL():
    # class variable
    GUIDB = 'GuiDB'
    #------------------------------------------------------------
def showData(self):
    # connect to MySQL
    conn, cursor = self.connect()

    self.useGuiDB(cursor)

    # execute command
    cursor.execute("SELECT * FROM books")
    print(cursor.fetchall())

    cursor.execute("SELECT * FROM quotations")
    print(cursor.fetchall())

    # close cursor and connection
    self.close(cursor, conn)
#================================================================
if __name__ == '__main__':
    # Create class instance
    mySQL = MySQL()
    mySQL.showData()
```

运行代码会得到以下结果：

`GUI_MySQL_class.py`

![](img/986b0dbf3f539f4f3baa82bd3003f04e_254_0.png)

我们可能不同意四人组，所以让我们更改他们著名的编程名言。

> 四人组是四位创建了世界著名书籍 *Design Patterns* 的作者，该书极大地影响了整个软件行业，使其认识、思考和编码时使用软件设计模式。

我们将通过更新我们最喜欢的引文数据库来实现这一点。首先，我们通过搜索书籍标题检索主键值，然后将该值传递到我们对引文的搜索中：

```
```python
#====================================================
def updateGOF(self):
    # connect to MySQL
    conn, cursor = self.connect()

    self.useGuiDB(cursor)

    # execute command
    cursor.execute("SELECT Book_ID FROM books WHERE Book_Title = 'Design Patterns'")
    primKey = cursor.fetchall()[0][0]
    print("Primary key=" + str(primKey))

    cursor.execute("SELECT * FROM quotations WHERE Books_Book_ID = (%s)", (primKey,))
    print(cursor.fetchall())

    # close cursor and connection
    self.close(cursor, conn)
#====================================================
if __name__ == '__main__':
    mySQL = MySQL()        # Create class instance
    mySQL.updateGOF()
```
```

这给我们以下结果：

GUI_MySQL_class.py

```
# execute command
cursor.execute("SELECT Book_ID FROM books WHERE Book_Title = 'Design Patterns'")
primKey = cursor.fetchall()[0][0]
print("Primary key=" + str(primKey))

cursor.execute("SELECT * FROM quotations WHERE Books_Book_ID = (%s)", (primKey,))
print(cursor.fetchall())

<terminated> C:\Eclipse_NEON_workspace\2nd Edition Python GUI Programming Cookbook\Ch07_Code\GUI_MySQL_class.py
Primary key=1
((1, 'Programming to an Interface, not an Implementation', 1),)
```

现在我们知道了引文的主键，我们可以通过执行以下命令来更新引文：

```
#----------------------------------------------------------------
def showDataWithReturn(self):
    # connect to MySQL
    conn, cursor = self.connect()

    self.useGuiDB(cursor)

    # execute command
    cursor.execute("SELECT Book_ID FROM books WHERE Book_Title = 'Design Patterns'")
    primKey = cursor.fetchall()[0][0]
    print(primKey)

    cursor.execute("SELECT * FROM quotations WHERE Books_Book_ID = (%s)", (primKey,))
    print(cursor.fetchall())

    cursor.execute("UPDATE quotations SET Quotation = (%s) WHERE Books_Book_ID = (%s)", ("Pythonic Duck Typing: If it walks like a duck and talks like a duck it probably is a duck...", primKey))

    # commit transaction
    conn.commit()
```

