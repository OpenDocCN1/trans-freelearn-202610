```
<tbody>
<?php
require 'list.php';
?>
</tbody>
</table>
</body>
</html>
```

+   7. As we have shown in the component diagram (see Figure 3.4), the index.php file depends on the list.php file. The first file keeps the static content while the second file generates the dynamic content. Since we have created the first file, we will now create the second file.

+   8. Select the school11 project, access the File | New | PHP File menu, and type list.php into the File name field. Input the following code into the file:

# list.php

```
<?php
use School\\SchoolClass;
foreach(SchoolClass::getAll() as $row) :
?>
<tr>
<td><a href="edit.php?code=<?=$row['code'] ?>"><?=$row['code'] ?></a></td>
<td><?=$row['name'] ?></td>
<td><a href="delete.php?code=<?$row
['code'] ?>">Remove</a></td>
</tr>
<?php
endforeach;
```

+   9. The list.php file contains a repeating loop that iterates over a collection provided by the SchoolClass class for listing the school classes. We are using an alternative syntax of the foreach structure, which uses endforeach instead of braces. This alternative syntax is useful when you have PHP code within HTML code because it is clearer. We have a few lines of code now, but as we have more and more lines, it can become confusing to understand the many different openings and closings of the PHP code blocks.