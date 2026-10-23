The very last pg_dump format is the .tar one, which can be obtained by means of the -Ft command-line flag. The result is the creation of a tar(1) uncompressed archive that contains the same directory structure that we created in the previous example, but where every file is not compressed:

```
$ pg_dump -Ft -f backup_forumdb.tar forumdb
$ tar -tf backup_forumdb.tar
toc.dat
3368.dat
3370.dat
3371.dat
3372.dat
3373.dat
3375.dat
3377.dat
```

Next, we will look at running a selective restore, which will help you choose which elements of a database you want to restore.

# Performing a selective restore

When performing a plain SQL database dump, you are allowed to manually edit the result, since it is plain text, and selectively remove parts you don’t want to restore. With custom formats and pg_restore, you can do the very same thing, but you need to perform a few steps to do so.

First of all, you can always inspect the content of a binary dump by means of pg_restore and its --list option, which prints the index (Table of Contents or ToC for short) to the screen. You need to specify, after the --list option, either the single file or directory that contains the backup to get the TOC printed:

```
$ pg_restore --list backup.d
;
; Archive created at 2023-09-29 15:35:12 UTC
;
; dbname: forumdb
;
; TOC Entries: 39
;
; Compression: -1
;
; Dump Version: 1.14-0
;
; Format: DIRECTORY
;
;
```