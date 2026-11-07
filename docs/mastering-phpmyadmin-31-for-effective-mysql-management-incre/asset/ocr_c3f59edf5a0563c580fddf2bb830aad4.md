# TIMESTAMP options

Starting with MySQL 4.1.2, there are more options that can affect a **TIMESTAMP** column. Let's add to our **book** table, a column named **stamp** of type **TIMESTAMP**. In the **Default** drop-down, we could choose **CURRENT_TIMESTAMP**; but we won't for this exercise. However in the **Attributes** column, we choose **on update CURRENT_TIMESTAMP**.

![](bbox=[0.3, 0.26, 0.7, 0.42])

# Bit

MySQL 5.0.3 introduced true bit-field values. These take the same amount of space in the database as the number of bits in their definition. Let's say we have three pieces of information about each book, and each piece can only be true (1) or false (0):

+   - Book is hard cover
+   - Book contains a CD-ROM
+   - Book is available only in electronic format

We'll use a single **BIT** field to store these three pieces of information. Therefore, we add a field having a length of **3** (which means 3 bits) to the book table:

![](bbox=[0.35, 0.65, 0.65, 0.74])

To construct and subsequently interpret the values we store in this field, we have to think in binary, respecting the position of each bit within the field. To indicate that a book is hard cover, does not contain a CD-ROM, and is available only in electronic format, we would use a value of 101.