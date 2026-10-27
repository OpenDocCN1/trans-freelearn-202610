# What just happened?

You have created a new template for the list of tickets in the customer area. To keep the example clean, I have removed the page navigation from the summary template, but you can copy it back from the Sample template if you like.

In the next step, change the detail template for the ticket detail view. In the admin console, click on Content | Calguys Feedback Module, and choose the Detail Templates tab. Click on New Template, and enter TicketDetail in the Template Name field. In the Template Text field, enter the code suggested as follows:

```
<h3>Ticket No. {$onecomment.id}: {$onecomment.title}</h3>
<p>{$onecomment.created|date_format:"%a %d %b %H:%M"} |
<a href="mailto:{$onecomment.author_email}">
{$onecomment.author_name}
</a></p>
<div class="feedback_item_data">
{$onecomment.data}
</div>
{if isset($onecomment.fields)}
{foreach from=$onecomment.fields key="name" item="field"}
{assign var=$name value=$field.value}
{/foreach}
{if !empty($Answer)}
<p><strong>Staff Answer</strong><br/>
<small>{$onecomment.modified|date_format:"%a %d %b %H:%M"} |
{$Staff}</small></p>
<div>
{$Answer}
</div>
{/if}
{/if}
```

Click on Submit. To use the created template for displaying tickets with staff answers, you have to set the template as default. Open the list of detail templates (Content | Calguys Feedback Module, Detail templates tab) and click the icon with a red cross in the Default column to use the template TicketDetail by default.