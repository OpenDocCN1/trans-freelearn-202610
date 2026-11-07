First, we add a **BLOB** field and **cover_photo** to our **book** table. If we now browse the table, we can see the field length information, **[BLOB - 0 B]**, for each **BLOB** field.

| isbn | title | page_count | author_id | language | description | cover_photo |
|---|---|---|---|---|---|---|
| 1-234567-89-0 | A hundred years of cinema (volume 1) | 600 | 1 | en |  | [BLOB - 0 B] |
| 1-234567-22-0 | Future souvenirs | 200 | 2 | en |  | [BLOB - 0 B] |
| 1-234567-90-0 | A hundred years of cinema (volume 2) | 602 | 1 | en |  | [BLOB - 0 B] |

This is because the **Show BLOB** display option (do you remember the **Options** slider?) has no check mark by default. So, it blocks the display of **BLOB** contents in Browse mode. This behavior is intentional. Usually, we cannot do anything with binary data represented in plain text.

# Binary content uploads

If we edit one row, we see the **Binary – do not edit** warning and a **Browse...** button. The exact caption on this button depends on the browser. Even though editing is not allowed, we can easily upload a text or binary file's contents into this **BLOB** column.

Let's choose an image file using the **Browse** button—for example, the **logo_left.png** file in a test copy of the **phpMyAdmin/themes/original/img** directory located on our client workstation. We now click **Go**.

![](bbox=[0.18, 0.59, 0.82, 0.64])

We need to keep in mind some limits for the upload size. Firstly, the **BLOB** field size is limited to 64K. Hence, phpMyAdmin reminds us of this limit with the **Max: 65,536 Bytes** warning. Also, there could be limits inherent to PHP itself (see Chapter 8 for more details). We have now uploaded an image inside this field for a specific row.

| isbn | title | page_count | author_id | language | description | cover_photo |
|---|---|---|---|---|---|---|
| 1-234567-89-0 | A hundred years of cinema (volume 1) | 600 | 1 | en |  | [BLOB - 6.7 KiB] |