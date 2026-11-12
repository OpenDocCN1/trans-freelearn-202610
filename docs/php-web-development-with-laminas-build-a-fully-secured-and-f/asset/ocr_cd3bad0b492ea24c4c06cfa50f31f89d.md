+   18. Create an index.phtml file in the view/school/school-class folder inside the School module.

The view folder has an application subfolder, a copy of the folder from the Application module.

+   19. Remove this subfolder because it is not useful for the School module.

+   20. Create a school folder inside the view folder and a school-class folder in the school folder.

The first level of folders under view is related to controller classes. The second level is related to controller actions. The name of folders and files inside the view folder must be in small letters. When the name of a class or method is formed of several names, the related folders and filenames must be separated with a hyphen.

+   21. Create the index.phtml file inside the school-class folder, as follows:

# index.phtml

```
<h1>Classes Registration</h1>
<p>
 <a href="<?=$this->url ('schoolclass',
 ['action' => 'edit']) ?>">Add a new class</a>
</p>
<table>
 <thead>
 <tr>
 <th>code</th>
 <th>name</th>
 </tr>
 </thead>
 <tbody>
 <?php
 foreach($this->schoolClasses as $row):
 ?>
 <tr>
 <td><a href="<?=$this->url
 ('schoolclass',['action' => 'edit', 'code'
 => $row->id]) ?>"><?=$row->code?></a></td>
```