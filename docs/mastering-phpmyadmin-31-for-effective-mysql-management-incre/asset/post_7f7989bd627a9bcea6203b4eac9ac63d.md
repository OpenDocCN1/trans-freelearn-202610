
For an introduction to collations, see http://dev.mysql.com/doc/mysql/en/Charset-general.htm, and for a more technical explanation of the algorithms involved, refer to http://www.unicode.org/reports/tr10/.

## Unicode and UTF-8

> Unicode is an industry standard designed to allow text and symbols [...] to be consistently represented and manipulated by computers.

For more information, visit http://en.wikipedia.org/wiki/Unicode and http://www.unicode.org/.

Unicode currently supports more than 600 languages, which is its main advantage over other character sets available with ISO or Windows. This is especially important with a multi-language product like phpMyAdmin.

To represent or encode these Unicode characters, many Unicode Transformation Formats (UTF) exist. A popular transformation format is UTF-8, which uses one to four octets per character. For more details, visit http://en.wikipedia.org/wiki/UTF-8.

Note that the browser must support UTF-8 (as most current browsers do). The phpMyAdmin distribution kit includes a UTF-8 version of every language file in the lang subdirectory.

## Language selection

A Language selector appears on the login panel (if any) and on the homepage. The default behavior of phpMyAdmin is to use the language defined in our browser's preferences, if there is a corresponding language file for this version.

The default language used—in case the program cannot detect one—is defined in config.inc.php, in the $cfg['DefaultLang'] parameter with 'en-utf-8'. This value can be changed. The possible values for language names are defined in the libraries/select_lang.lib.php script as an array.

Even if the default language is defined, each user (especially on a multi-user installation) can choose his/her preferred language from the selector. The user's choice will be remembered in a cookie whenever possible.

We can also force a single language by setting the $cfg['Lang'] parameter with a value, such as 'fr-utf-8'. Starting with version 2.7.0, another parameter, $cfg['FilterLanguages'], is available. Suppose we want to shorten the list of available languages to English and Français – French, as those are the ones used exclusively by our users. This is accomplished building a regular expression indicating which languages we want to display based on the ISO 639 codes of these languages. To continue with our example, we would use:

```
$cfg['FilterLanguages'] = '^(fr|en)';
```

In this expression, the caret (^) means starting with and the (|) means or. The expression indicates that we are restricting the list to languages whose corresponding ISO codes start with fr or en.

By default, this parameter is empty, meaning that no filter is applied to the list of available languages.

The small information icon beside Language gives access to phpMyAdmin's translator page, which lists, by language, the official translator and the contact information. This way, we can reach the translator for corrections, or to offer help on untranslated messages.

## Effective character sets and collations

On the homepage, we can see the MySQL charset information and a MySQL connection collation selector. Here is the MySQL charset information:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_74_0.png)

The character set information (as seen here after MySQL charset) comes directly from the $charset variable located in the language file that corresponds to the currently selected language. It is used to generate HTML information, which tells the browser what is the page's character set.

We can also choose which character set and collation will be used for our connection to the MySQL server using the MySQL connection collation dialog. This is passed to the MySQL server. MySQL then transforms the characters that will be sent to our browser into this character set. MySQL also interprets what it receives from the browser according to the character set information. Remember that all tables and fields have a character set information describing how their data is encoded.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_75_0.png)

Normally, the default value should work. However, if we are entering some characters using a different character set, we can choose the proper character set in this dialog.

The following parameter defines both the default connection collation and character set:

```
$cfg['DefaultConnectionCollation'] = 'utf8_unicode_ci';
```

## Left panel (navigation)

The left panel contains the following elements:

- The logo (see the next section)
- The server list (if $cfg['LeftDisplayServers'] is set to TRUE)
- The Home link or icon (takes you back to the phpMyAdmin homepage)
- A Log out link or icon
- A link or icon leading to the Query window
- Icons to display phpMyAdmin and MySQL documentation
- The databases and tables with statistics about the number of tables per database

If $cfg['MainPageIconic'] is set to TRUE (the default), we see the icons. However, if it is set to FALSE, we see the Home, Log out, and Query window links.

The left panel can be resized by clicking and moving the vertical separation line in the preferred direction to reveal more data, in case the database or table names are too long for the default left panel size.

We can customize the appearance of this panel. Generally, all parameters are located in `themes/themename/layout.inc.php` unless stated otherwise. `$cfg['LeftWidth']` contains the default width of the left frame in pixels. The background color is defined in `$cfg['LeftBgColor']` with a default value of `'#D0DCE0'`. The `$cfg['LeftPointerColor']` parameter, with a default value of `'#CCFFCC'`, defines the pointer color. (The pointer appears when we are using the Full mode, discussed shortly.) To activate the left pointer for any theme being used, a master setting, `$cfg['LeftPointerEnable']`, exists in `config.inc.php`. Its default value is TRUE.

## Logo configuration

The logo display behavior is controlled by a number of parameters. First, `$cfg['LeftDisplayLogo']` has to be set to TRUE, to enable any displaying of the logo. It is true by default. A click on this logo brings the interface to the page listed in the `$cfg['LeftLogoLink']` parameter, which is usually the main phpMyAdmin page (default value `main.php`), but can be any URL. Finally, the `$cfg['LeftLogoLinkWindow']` parameter indicates in which window the new page appears after a click on the logo. By default, it's on the main page (value `main`). However, it could be on a brand new window by using the value `new`.

The logo image itself comes from the `logo_left.png` file, which is located in each specific theme directory structure.

## Database and table list

The following examples show that no database has been chosen yet:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_76_0.png)

It is also possible to see the following screen:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_77_0.png)

This means that our current MySQL rights do not allow us to see any existing databases.

> A MySQL server always has at least one database (named **mysql**), but in this case, we do not have the right to see it. Moreover, as MySQL 5.0.2, a special database called **information_schema** appears at all times in the database list. It contains a set of views describing the metadata visible for the logged-in user.

We may have the right to create one, as explained in Chapter 4.

## Light mode

The left panel can be shown in two ways—the Light mode and the Full mode. The Light mode is used by default, defined by a TRUE value in $cfg['LeftFrameLight']. This mode shows a drop-down list of the available databases, and only tables of the currently chosen database are displayed. It is more efficient than Full Mode; the reason is explained in the Full Mode section appearing later in the chapter. Here we have chosen the **mysql** database:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_77_1.png)

Clicking on a name opens the right panel in the **Database** view, and clicking on a table name opens the right panel in the **Table** view to browse this table. (see the *Right Panel* section for details.)

## Tree display of database names

A user might be allowed to work on a single database—for example **marc**.
Some system administrators offer a more flexible scheme by allowing user **marc** to create many databases, provided all have their names starting with **marc**, such as **marc_airline** and **marc_car**. In this situation, the left panel can be set to display a tree of these database names, like this:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_78_0.png)

This feature is currently offered only in light mode, and is controlled by these parameters:

```
$cfg['LeftFrameDBTree'] = TRUE;
$cfg['LeftFrameDBSeparator'] = '_';
```

The default value of TRUE in $cfg['**LeftFrameDBTree**'] ensures that this feature is activated. A popular value for the separator is '_'.

## Full mode

The previous examples were shown in Light mode, but setting the $cfg['**LeftFrameLight**'] parameter to FALSE produces a complete layout of our databases and tables using collapsible menus (if supported by the browser):
