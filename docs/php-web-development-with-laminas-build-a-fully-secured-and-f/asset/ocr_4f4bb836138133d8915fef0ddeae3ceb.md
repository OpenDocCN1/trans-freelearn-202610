```
<td><?=$row->name?></td>
<td><a href="<?=$this->url('schoolclass',
['action' => 'delete', 'code' => $row->code]) ?>">Remove</a></td>
</tr>
<?php
endforeach;
?>
</tbody>
</table>
<p><a href="<?=$this->url('home') ?>">Homepage</a></p>
```

The $this->schoolClasses attribute was defined by the indexAction method of SchoolClassController through the ViewModel class. The true difference for this file is the use of the $this->url method. This method creates a URL from a route declared in some of the module.config.php files.

+   22. Finally, you can run the test successfully!

Wait, won't we need to navigate in the web interface? Well, you can navigate partially with the listing implemented here or by downloading the complete project.

+   23. Start a web server. As we did with our “hello world” project, you can use the embedded web server of PHP. Inside the school3 folder, you run the following:

```
php -S localhost:8000 -t public
```

Now you can access the school3 application at localhost : 8000.

Don't worry if some details here seem like magic; each one will be explained starting in the next chapter. The main aim of Chapter 3, Using Laminas as a Library with Test-Driven Development and Chapter 4, From Object-Relational Mapping to MVC Containers was to avoid a sudden leap from an experience with pure PHP to the Laminas way of development. You can find a quick-start guide in Laminas’ documentation, but it is like an improved “hello world,” without database handling.

So, you have reached the end of this section with a little MVC Laminas application, where the framework calls your code instead of your code calling the framework components.

The school3 project is fully available at https://github.com/PacktPublishing/PHP-Web-Development-with-Laminas/tree/main/chapter04/school3. You can install, navigate, and handle school classes and students. After this preparation, you are ready for our awesome e-commerce project. You will soon deeply understand each layer of the Laminas MVC implementation.