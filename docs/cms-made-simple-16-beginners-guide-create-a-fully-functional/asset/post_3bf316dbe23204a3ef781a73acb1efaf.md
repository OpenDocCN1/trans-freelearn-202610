
## What just happened?

You have created your own user-defined tag (UDT) in CMS Made Simple. This tag automatically shows the number of days until the date given in the first line of the PHP script (January 1, 2011).

Then, you have used the tag in a sentence on the page. Instead of the tag {countdown}, the number of days is shown at the place where you have added the tag.

The name of the tag is technical information and can only contain letters, numbers, and underscores, just like the names of PHP variables. The name is what you will have to type in curly brackets to add it into a page. So be descriptive, but do not make it too long.

You do not need any <? or <?php tags when you paste the code into UDT, just skip them. If you make a syntax mistake in your PHP code, then you will not be able to save the code. However, the following error will be displayed, so that you know you have to correct your PHP code.

> Invalid code entered.
> Parse error: syntax error, unexpected ']' in /var/www/ html/businessworld /admin/edituserplugin.php(103) : eval()'d code on line 5

## Parameters for tags

User defined tags get really useful when you start to add parameters to them. You can pass parameters to the PHP code this way:

```
{countdown date='05-01-2011'}
```

In this way, you can specify the countdown date directly in the UDT instead of writing it in the code, so that that there is no need to customize PHP code again and again.

In PHP code, you can get the parameter with the help of the special array $params, which is always defined and contains all parameters passed. In the next example, the passed parameter date is used in the countdown script:

```
$date = explode('-', $params['date']);
$target = mktime(0, 0, 0, $date[0], $date[1] , $date[2]);
$diff = $target - time();
$days = floor($diff/86400);
echo $days;
```

Test this tag the same way as before, but instead of using {countdown}, use {countdown date='05-01-2011'} or any other date in the format MM-DD-YYYY.

These are basic principles of writing user-defined tags. You can do some useful things with a little imagination, so just think about what you can do if you know a little PHP.

## How to get the page information in UDT

You can pass any variable to the UDT including objects created by CMS Made Simple. You can show variables and objects that exist in the page with the following code:

```
{get_template_vars}
```

If you add the above tag to the template, then you will find out that the page has an object called $content_obj. This object contains all the information about the actual page and can be passed to UDT as well.

Let's assume we need some additional navigation that looks like the tracing path of the visitor on the website. It means that we would like to display the last three pages that the visitor has seen. This navigation has to be built automatically and saved in the visitor session.

Create a new user-defined tag and call it trace. Add the PHP code shown in the following code snippet:

```
$pagetitle = $params['page']->Name();
$pageurl = $params['page']->GetURL();

$_SESSION['trace'][$pagetitle] = $pageurl;

if (sizeof($_SESSION['trace']) > 3) {
    array_shift($_SESSION['trace']);
}

$trace = array_reverse($_SESSION['trace']);

foreach ($trace as $title => $url) {
    echo '<a href="'. $url .'">'. $title .'</a><br />';
}
```

This code assumes that the parameter page has been passed to it ($params['page']) and this parameter contains the page object. We can call the user-defined tag as shown in the following code snippet:

```
<h4>Last 3 pages you have visited</h4>
{trace page=$content_obj}
```

I suggest that you add the tag to the page template below the site navigation. Every time the user opens a new page, the information about it is passed to the UDT. The page parameter contains the page object, and can be used with page content methods (Name() and GetURL()). Please see CMS Made Simple API for more information.

The last three pages that the visitor has seen are saved in the session variable and are now temporarily displayed below the sidebar menu.

## Understanding events

In the last section, you learned to create user-defined tags. To execute the PHP code in the tags, you had to place it in the page or the template. With Event Manager, you can execute the code when certain events occur. An event can be the creation of a new page or even deleting a user in the admin console. There are over 50 events already defined in CMS Made Simple, and every module can have additional events.

Examples for executing user-defined tags on events are as follows:

- Sending an e-mail to the administrator of the website if any page is updated
- Replacing certain parts of the page before displaying it on the website
- Deleting associated stylesheets if the template is deleted

CMS Made Simple triggers events such as ContentEditPost or DeleteTemplatePre, when the specified action occurs. You create one or more user-defined tags with custom functionality and assign them to the event. This means that the assigned functionality will be executed when the event is triggered. The event sends the actual data to the user-defined tag (if there is any), and this data can now be used in any way in your PHP code.

See the list of events already added to CMS Made Simple in the admin console (Extensions | Event Manager). For every event, there is a brief description of when the user-defined tags appended to it will be executed.

This functionality allows anybody with a bit of PHP knowledge to extend core or module functionality. For example, it should now be easy to send an e-mail to the administrator if the page is modified by the editor. For the next example, the CMSMailer module should be configured properly, as explained in Chapter 2, Getting Started.

## Time for action – sending mails after page update

To enable CMS Made simple to send e-mails to the administrator when a content page is updated, perform the following steps:

- 1. In the admin console, click on Extensions | User Defined Tags.
- 2. Click on the Add User Defined Tag link.
- 3. Name the new tag buzzer and the code as shown:
```
global $gCms;
$cmsmailer = $gCms->modules['CMSMailer']['object'];
$cmsmailer->AddAddress($cmsmailer->GetFrom(),$cmsmailer->GetFromName());
$cmsmailer->SetSubject('Admin buzzer');
$cmsmailer->SetBody('The page <i> '.$params['content']->mName.' </i> has been changed.');
$cmsmailer->IsHTML(true);
$cmsmailer->Send();
```
- 4. Click on Submit.
- 5. In the admin console, click on Extensions | Event Manager.
- 6. Find the event called ContentEditPost, and click the edit button on the same line.
- 7. Select the buzzer tag from the drop-down field, and click Add.
- 8. Now, change any page to test the admin buzzer function. You should receive an e-mail notification with the page name changed.

## What just happened?

We have used the CMSMailer module of CMS Made Simple to send an e-mail notification. In this way, we consider all individual settings made in the module and ensure that e-mails are sent properly.

The PHP code added to the tag is a slight modification of the sample code added to the Help section of the CMSMailer module. It sends mails to the e-mail address configured in the module in the field From address. We have also used the name of the changed page in the e-mail notification. This was possible because an event provides us with the object $params['content'] that contains a reference to the affected page and all information about the page.

Then, you have assigned the user-defined tag to the event ContentEditPost. This event is sent after the page is edited and submitted (it is not enough to just Apply changes, you must Submit them).

## Integrating jQuery in navigation

CMS Made Simple uses plain HTML, CSS, and Smarty in all templates. This means that you can use any jQuery effect in your templates as well. jQuery is a JavaScript framework and has a lot of plugins.
