```
con=wr.mysql.connect("wranglerbook")
cursor=con.cursor()
cursor.execute("show columns from seat_type")
cursor.fetchall()
```

```
([('name', 'varchar(15)', 'NO', 'PRI', None, ''),
('description', 'varchar(120)', 'YES', '', None, ''),
('relative_quality', 'tinyint', 'YES', '', None, '')])
```

```
cursor.execute("select * from seat_type;")
cursor.fetchall()
```

```
([('luxury', 'Excellent seats – box seats, behind the plate, etc. etc.', 1),
('obstructed', 'Pretty crappy, partially obstructed seats', 5),
('premium', 'Really good seats – first level, mid field, etc. etc.', 2),
('standard', 'Standard seats – not super awesome but pretty good', 3),
('standing', "Really? That's – not a seat at all now, is it?", 10),
('sub-standard', 'End zone, nose bleed, etc.', 4)])
```

# Figure 9.38: Exploring the current data available in the seat table

We will modify the data to emulate a dataset that will help us walk through the data-cleaning activities. Rather than deleting or updating the current data from the seat table, we will insert new records that are a mocked-up version of the current data.

The following screenshot shows the new mocked-up records with the same columns in the seat_type table. However, the description column has now additional details such as the level of the stadium in which the seat type is available and how many seats of that particular type are available, along with a description.

```
cursor.execute("insert into seat_type values('Luxury1', 'level1,1500 seats,Comfortable seats,Leather seats!!!',1)")
cursor.execute("insert into seat_type values('Obstructed1', 'level3,100 seats,Comfortable seats though,But wont have any v...',1)")
cursor.execute("insert into seat_type values('Premium1', 'level4-5,500 seats,Comfortable but normal seats, good views for 1...',1)")
cursor.execute("insert into seat_type values('Standard1', 'level1,5500 seats,Comfortable seats,Leather seats!!!',3)")
cursor.execute("insert into seat_type values('standing1', 'level1-5,Comfortable seats,Leather seats!!!',10)")
cursor.execute("insert into seat_type values('sub-standard1', 'level1,200 seats,Front row , risk of injury!!!',4)")
```

# Figure 9.39: Mocked-up records for the seat table

For our current use case, we will extract only the data that we inserted into the table. We will filter records that have a seat type name column with values that begin with a letter (a-z) and end with a numeric value (0-9), which will only get the newly inserted records that have the number 1 appended at the end of the name column.