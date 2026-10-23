```
; Selected TOC Entries:
;
6; 2615 16653 SCHEMA - forum forum
215; 1259 16654 TABLE forum categories forum
216; 1259 16659 SEQUENCE forum categories_pk_seq forum
217; 1259 16660 TABLE forum delete_posts forum
218; 1259 16665 TABLE forum j_posts_tags forum
219; 1259 16668 TABLE forum new_categories forum
220; 1259 16673 TABLE forum posts forum
221; 1259 16682 SEQUENCE forum posts_pk_seq forum
222; 1259 16683 TABLE forum tags forum
223; 1259 16688 SEQUENCE forum tags_pk_seq forum
224; 1259 16689 TABLE forum users forum
225; 1259 16694 SEQUENCE forum users_pk_seq forum
3368; 0 16654 TABLE DATA forum categories forum
3370; 0 16660 TABLE DATA forum delete_posts forum
3371; 0 16665 TABLE DATA forum j_posts_tags forum
3372; 0 16668 TABLE DATA forum new_categories forum
3373; 0 16673 TABLE DATA forum posts forum
3375; 0 16683 TABLE DATA forum tags forum
3377; 0 16689 TABLE DATA forum users forum
3385; 0 0 SEQUENCE SET forum categories_pk_seq forum
...
```

Lines beginning with a semicolon are comments, and as you can see, the first few lines that are printed out are a banner that describes the content of the backup, the date the backup was taken, the format (in this example, “directory”), and how many entries (objects) are in the backup.

Every line that is not a comment represents a database object or a single action that the restore process will perform. As an example, take a look at the following line:

```
222; 1259 16683 TABLE forum tags forum
```

This indicates that the tags table will be restored by the user forum within the forum schema.

The following line means that the same table will be filled with the data:

```
3373; 0 16673 TABLE DATA forum posts forum
```