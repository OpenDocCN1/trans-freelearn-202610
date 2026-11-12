Remember this: you can only improve something that already exists. Code that works can be improved. Code that works also usually needs to be changed to allow new features or fix bugs. After making the required changes, code usually needs to be reviewed to avoid maintenance troubles in the future. Software improvement is an endless process. While it is useful, it is a product in a continuous improvement process.

# Eliminating duplicated code

A computer program is a set of instructions. The wonderful thing about computers is that we don’t need to repeat an instruction for them if this instruction is already stored. We only need to say where the instruction is, like pointing somebody who is reading a handbook to a specific page. In fact, if you give the handbook to somebody, they don’t need to ask you what is written in it anymore.

When we create software, we create several handbooks. These handbooks can be procedures, functions, classes, or traits. All of these code structures are reusable units of code. We have several options to avoid duplicated lines of code, by centralizing them in one of these structures.

Duplicated lines of code can generate more work because you will have to modify more than one line, possibly in more than one file, to implement a new feature or fix a bug. For this reason, we have to avoid duplicated lines of code.

There is a useful tool to detect duplicated lines in PHP programs – **PHP Copy/Paste Detector (PHPCPD)**. You can download the latest version of PHPCPD from `https://phar.phpunit.de/phpcpd.phar`. The downloaded file is `phpcpd.phar`. Rename this file `phpcpd` and permit it to execute. Move `phpcpd` to a directory of the system path (which is visible everywhere).

The `phpcpd` program compares PHP code from a folder and shows the pairs of duplicated code blocks. Let us run `phpcpd`. However, before we do this, we need to delete the `vendor` subdirectory inside the `whatstore` directory. Why? It is because we have to analyze only our code and not the dependencies code.

So, let's run the following command:

```
$ phpcpd whatstore/
```

The output will look like this:

```
phpcpd 6.0.3 by Sebastian Bergmann.

Found 1 clones with 21 duplicated lines in 2 files:

 - /opt/lamp/htdocs/whatstore/module/Inventory/src/Model/
Discount.php:31-52 (21 lines)
```