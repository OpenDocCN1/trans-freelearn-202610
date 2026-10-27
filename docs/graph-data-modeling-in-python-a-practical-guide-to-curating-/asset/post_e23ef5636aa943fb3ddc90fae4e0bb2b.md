
For this section, we will be using MySQL version 5.7.39. Newer versions of MySQL are available, but loading data into MySQL tables in newer versions requires additional configuration. If you already have MySQL installed, you can skip the installation steps and proceed to the Setting up a MySQL database section.

Navigate to https://downloads.mysql.com/archives/installer/ and select the appropriate MySQL 5.7.39 (https://downloads.mysql.com/archives/get/p/25/file/mysql-installer-community-5.7.39.0.msi) distribution for your operating system. Install MySQL with all default options (you can uncheck Open Windows Firewall port for network access if you wish – we will only be connecting to the database locally). When prompted to enter a password for the server, choose a dummy password for this chapter, since we will be connecting to the database using Python, and the password will be visible.

Should any issues arise during installation, consult the setup guide at https://dev.mysql.com/doc/mysql-getting-started/en/ for operating system-specific tips.

## Setting up a MySQL database

The following steps will aid you in setting up your MySQL database:

- 1. To begin setting up a MySQL database, first, navigate to the installation folder binaries. For a Windows machine, they are likely located at `C:/Program Files/MySQL/MySQL Server 5.7/bin`. Open a Command Prompt terminal here. Alternatively, open Command Prompt from the Start menu, and navigate to this folder using the cd command:

```
cd "Program Files/MySQL/MySQL Server 5.7/bin"
```

- 2. Once in a Command Prompt in the bin folder, access the MySQL shell by running the following command:

```
mysql -u root -p
```

When prompted for a password, enter the password that you set up during your MySQL installation earlier.

- 3. Now that we are in the MySQL shell, we can create a database, like so:

```
CREATE DATABASE steam_data;
```

This database now needs to have tables and data added to it. First, we need to move the `steam_play.csv` and `steam_purchase.csv` files over to an area where MySQL can access them. Copy and paste these CSV files into `C:\ProgramData\MySQL\MySQL Server 5.7\Uploads`.

- 4. Now that MySQL can access our data, we can create our two tables. In this MySQL shell, we can prepare our tables with the following commands, each run separately:

```
USE steam_data;
CREATE TABLE steam_play (id VARCHAR(100), game_name VARCHAR(255), type VARCHAR(10), hours FLOAT(10, 1), misc_column INT);
CREATE TABLE steam_purchase (id VARCHAR(100), game_name VARCHAR(255), type VARCHAR(10), game_purchased_flag FLOAT(2, 1), misc_column INT);
```

- 5. We will come back to the structure of our data and its fields later in the next section, once it has been loaded into MySQL. For now, to populate the blank tables we have created, run the following lines:

```
LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 5.7/Uploads/steam_play.csv' INTO TABLE steam_play FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '\"' LINES TERMINATED BY '\r\n';
LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 5.7/Uploads/steam_purchase.csv' INTO TABLE steam_purchase FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '\"' LINES TERMINATED BY '\r\n';
```

On a successful import, the MySQL shell window should print out how many records have been added. For the `steam_play` table, there should be 70,489 rows, while for the `steam_purchase` table, there should be 129,511 rows.

With our relational database set up, we are now ready to begin evaluating and analyzing the data.

## Querying MySQL in Python

To transform our relational database into a graph, our first step must be examining the data and designing a sensible graph schema to represent it. As we did in Chapter 2, Working with Graph Data Models, let’s examine the first few lines of the datasets.

From this point, we will be accessing data in MySQL from Python by using the `mysql.connector` driver. We can use the `mysql.connector.connect()` method to open an authenticated connection with our locally hosted database and query the data contained in its tables. The steps to get to your MySQL database instance, from Python, are captured hereunder:

- 1. To start, we’ll need to import the driver and make a connection to our database. Connecting to `localhost` means that we are accessing a database on the local machine, while `root` means that we will be accessing the database as an administrator. Our database name is equal to the one we set up in the MySQL shell earlier. Replace `password` with the password you chose during the MySQL setup in the previous section:

```
import mysql.connector
PASSWORD = '<Your MySQL password upon creating MySQL instance>'
connection = mysql.connector.connect(
    host="localhost",
    user="root",
    passwd=PASSWORD,
    database="steam_data"
)
```

- 2. Now, we can specify some example SQL to query a table:

```
sql = 'SELECT count(*) FROM steam_purchase;'
```

- 3. We can run the SQL string in our variable using `connection.cursor()` and `cursor.execute()`. Then, we can retrieve and print the result using `cursor.fetchall()` and a `print` statement, before finally closing the connection with `connection.close()`:

```
cursor = connection.cursor()
cursor.execute(sql)
result = cursor.fetchall()
print(result)
connection.close()
```

- 4. Now, because we’ll be accessing these tables many times, let’s set up a reusable method for this process. We will just be changing the SQL queries to the same database so that we can abstract away the query into a new method parameter:

```
import mysql.connector
def query_mysql(query, password, host='localhost', user='root', database='steam_data'):
    connection = mysql.connector.connect(
        host=host,
        user=user,
        passwd=password,
        database=database
    )
    cursor = connection.cursor()
    cursor.execute(query)
    result = cursor.fetchall()
    connection.close()
    return result
```

- 5. Now, we can call our new `query_mysql()` method to get the length of the `steam_purchase` table again, to check that it is functioning as expected:

```
result = query_mysql('SELECT count(*) FROM steam_purchase;', password=PASSWORD)
print(result)
```

Our method returns the `result` variable, and printing this shows that the `steam_purchase` table contains 129,511 rows.

## Examining the data in Python

Now that we can query data from our MySQL database through Python, let's begin to examine the data contained in our tables.

Follow these steps to examine your data in MySQL:

- 1. First, let's look at the `steam_play` table, which contains play time data for Steam users. We can take a look at the first few lines with another simple SQL query, using the method we defined in the previous section:

```
result = query_mysql('SELECT * FROM steam_play LIMIT 4;', password=PASSWORD)
for row in result:
    print(row)
```

- 2. Looking at the first few printed rows of `steam_play`, we can see that there are five columns:

```
('151603712', 'The Elder Scrolls V: Skyrim', 'play', 273.0, 0)
('151603712', 'Fallout 4', 'play', 87.0, 0)
('151603712', 'Spore', 'play', 14.9, 0)
('151603712', 'Fallout: New Vegas', 'play', 12.1, 0)
```

According to the data's source, the first column contains a unique user ID. The second column contains the name of the game the user has played. The third column contains the 'play' string in each row, which gives us little information. The fourth column shows the number of hours the row's user has been playing a game. Finally, the last column seems to contain only 0s; we can safely ignore this column going forward.

- 3. The other table, `steam_purchase`, contains information on user purchases. Again, we should check the first few lines of this dataset:

```
('151603712', 'The Elder Scrolls V: Skyrim', 'purchase', 1.0, 0)
('151603712', 'Fallout 4', 'purchase', 1.0, 0)
('151603712', 'Spore', 'purchase', 1.0, 0)
('151603712', 'Fallout: New Vegas', 'purchase', 1.0, 0)
```

The `steam_purchase` table also seems to contain five columns. In the `steam_purchase` table, the first column again contains a unique user ID. As with `steam_play`, the second column contains a game's name. The third column always contains the 'purchase' string. The fourth column in this table always contains float 1.0, indicating the purchase of that row's game, by the user. The last column, as in the previous file, contains all 0s.

At a glance, we can see that there is data for user `151603712` in both the `steam_play` and `steam_purchase` tables. So, users in our datasets have data about both the purchase of games and play time. From a brief look into the data, we can see that there are plenty of columns that we won't need going forward, so we can start to exclude them from our analysis by querying specific rows.
