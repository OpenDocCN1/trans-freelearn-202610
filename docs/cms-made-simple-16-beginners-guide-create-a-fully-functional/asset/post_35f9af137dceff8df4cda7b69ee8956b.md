
## E-commerce Workshop

Now that we've learned how to realize a complete website including e-commerce functionality, we're ready to proceed with advanced topics that require basic PHP knowledge, which is the subject of the next chapter. By this time your website is fully functional and can be published, so that you do not need any extended topics from the next chapter to have a complete website.

## Advanced Use of CMS Made Simple

> This chapter includes some tips for advanced webmasters who are creating websites with CMS Made Simple. You do not need to read the entire chapter; you can just pick up the sections that are most important to you.

In this chapter, we will:

- Learn how to develop multilingual websites with CMS Made Simple
- Create additional layout control possibilities for the editors of the website
- Make some search engine optimizations
- Work with user-defined tags and the event manager to implement our own functionality or define custom workflows

You will need basic PHP knowledge to follow instructions in this chapter. So let's get on with it.

### Localization and translation

The admin console of CMS Made Simple is translated in over 20 languages. As you already know from Chapter 2, CMS Made Simple is distributed in two versions. The base version contains just the English language files, while the full version includes all language files.

If you need only one language in addition to English (English cannot be uninstalled), you can download the base package and then the required language files. Language files can be found on the official CMS Made Simple website (http://www.cmsmadesimple.org/), in the **Downloads | CMSMS Release** section at the bottom of the page (you will only need the file corresponding to your release). To include any language in the base installation, download and unpack the base installation file first, then download the required language file corresponding to the base release number and unpack it as well. Merge the contents of the base folder with the contents of the language file folder by copying the language folder over the base files. Then install CMS Made Simple as usual.

Users with access to the admin console of the website can select their preferred language in their personal accounts (**My Preferences | User Preferences**). For the base installation, only **English** is shown in the field. If you have installed any additional language, then it will be shown in the drop-down field **Language** as well. This setting does not affect the website language used for visitors to the website.

To set up the website language, in the admin console, click on **Site Admin | Global Settings**, and change the **Default language for the frontend**. If no language is set there, then CMS Made Simple uses English as the default language (**en_US**). This setting will automatically change the default text for some modules on the website, for example, the label of the search button in the **Search** module or some labels in the **News** module.

![](img/b21e3dc5c94405bc7b3f102758ea770c_279_0.png)

In the preceding screenshot, see the difference between the labels for **Category** (*Kategorie* in German), **Posted by** (*Erstellt von* in German), and so on. This will help to define most labels displayed by the modules that are translated into your language. However, this would only help if your website is not multilingual and uses only one language.

### Configuring dates

From time to time, CMS Made Simple must output dates and times. Especially, for news articles, the proper date format is important.

The general date format can be changed in the admin console (**Site Admin | Global Settings**) in the field **Date Format String**. The date format should be entered according to the PHP format function `strftime` (http://www.php.net/manual/en/function.strftime.php). There are some examples shown in the following table:

| Date format string | Output |
|---|---|
| `%Y-%m-%d` | 2009-05-20 |
| `%a %d %b %Y` | Wed 20 May 2009 |
| `%a %d %b %H:%M:%S %Y` | Wed 20 May 15:53:40 2009 |

The first column is built according to allowed parameters (please see the last link for an explanation). The second column shows the result on the page. Enter parameters from the first column in the **Date Format String** field to get the results shown in the second column.

The month and weekday names depend on the `locale` setting of your server. On some servers, you can change the language used to output dates. Setting the locale will also affect currency, decimal separators, and other outputs dependent on the language. In CMS Made Simple, open the `config.php` file and find the line with `$config['locale']`. It depends on your server, what value you enter here. For German month names on a Linux server, try changing the line as shown:

```php
$config['locale'] = 'de_DE.UTF8';
```

### Making multilingual websites

Multilingual websites are created in more than one language. Every language has its own translated navigation, content, and probably translated news articles. We do not speak about automatic translation of the websites that are made by Google or any other tool. Automatic translations are awful. They are often not readable by human beings and make no sense. In this section, you will learn how to create a real multilingual website where the entire content is translated by human translators.

You can use the **Babel** module to manage multiple languages for the website and navigation. Install the module the same way you install other modules for your website (**Extensions | Module Manager**).

After installation of the module, set up the page structure for the website to suit your desired languages, for example:

- 1. English
  - 1.1. Our company
  - 1.2. Announcements
  - 1.3. Contact Us
- 2. German
  - 2.1. Unternehmen
  - 2.2. Aktuelles
  - 2.3. Kontakt

The root language pages (1. and 2.) should be defined as pages with the content type Section Headers. You can give them any title you want. For the page alias, I suggest using the two-letter ISO 639-1 code system such as en (for English), de (for German), and fr (for French). The page alias will be used in the website URL like http://www.yourdomain/en/our-company or http://www.yourdomain/de/kontakt.

In order to display the menu correctly (that is, not to display language header pages and pages belonging to other languages), you have to add the parameter start_level="2" to your top menu tag in the main template (Layout | Templates). Open the template, and adjust your top navigation, as shown in the following code snippet:

```
{menu number_of_levels="1" start_level="2"}
```

This would ensure that only second level pages belonging to the selected language are displayed in the top navigation. If you use the sidebar navigation, you should change the number of levels here as well by reducing the number of the start level:

```
{menu start_level="3"}
```

When the page structure is created, click on Extensions | Babel: Multilingual Site in the admin console. For each language you have integrated in the page structure, click on the link Add Language, and enter the appropriate information, as shown in the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_281_0.png)

The **Root page associated with the language** should be one of the section headers you have created for each language. The **Default page for redirection to this language** should be the start page for each language (not the section header from the preceding field). For the **Language code**, you can use any five character code, but in order to provide good compatibility with the modules, you should use the same language codes as CMS Made Simple. Use the drop-down list on the top-right of the admin area to prefill the form with default values.

Click **Submit**, and be sure that you have activated the language in the **Active** column. Add all desired languages the same way.

To display a language menu, use the following tag in the main template at the place where the links to different languages should be displayed:

```
{babel action="menu"}
```

### Editing language entries

In the admin area of the **Babel** module, you will find a link for editing the language entries for each language. Language entries are strings that exist for each language but hold different text (or HTML code). For the label of the *search* button in the **Search** module, you would definitely like to have different descriptions such as:

- *Search* for English
- *Suche* for German
- *Recherche* for French, and so on.

Open the **Babel: Multilingual Site** module in the admin console, and click on the **Edit language entries** link. Click on the **Add a new language entry** link at the top-right corner of the module's admin area. This is the place where you can define your personal translation for each language. The entry name is a general key that will be used to get the right translation. In our example, we would like to translate the label of the search button, so the key for the translation can be `search button`. In the fields below, enter the translation for each defined language, and click on **Submit**.

Customize the label for the search button in the template of the module **Search (Extensions | Search, tab Search Template)**, and replace the tag `{$submittext}` with `{babel show="search button"}`, as shown in the following code snippet:

```
{$startform}
<input type="text" class="search-input"
    id="{$search_actionid}searchinput"
    name="{$search_actionid}searchinput" size="18" maxlength="50"
    value="{$searchtext}" {$hogan}/>
<input class="search-button" name="submit" value="{babel show="search button"}" type="submit" />
{if isset($hidden)}{$hidden}{/if}
{$endform}
```
