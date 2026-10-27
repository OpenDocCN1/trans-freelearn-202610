To give a finishing touch, create a template for the dialog form and a template for the dialog summary view. The dialog form does not need any special title as it will get the title of the ticket and gets **Re:** as prefix for the ticket title. It will also be prefilled with the values from the ticket. Create a new comment form template, and add the following code snippet into it:

```
{if isset($message)}
    <p class="info">{$message}</p>
{else}
    {if isset($error)}
    <p class="error">{$error}</p>
    {/if}
    {$formstart}
    <input type="hidden" name="{$actionid}title" value="Re:
    {$onecomment.title}"/>
    </p>
    <p><label>{$mod->Lang('prompt_your_name')}:</label>
        <input type="text" name="{$actionid}author_name"
            value="{$onecomment.author_name}"/>
    </p>
    <p><label>{$mod->Lang('prompt_your_email')}:</label>
        <input type="text" name="{$actionid}author_email"
            value="{$onecomment.author_email}"/>
    </p>
    <p><label>Question:</label>
        <textarea name="{$actionid}comment" cols="40"
                rows="5">{$comment}</textarea></p>
    <p><input type="submit" name="{$actionid}submit" value="{$mod
>Lang('submit')}"/>
    </p>
    {$formend}
{/if}
```

Save the template with the name **TicketDialogue**. In the detail view of the ticket (template **TicketDetail**), adjust the Smarty tag at the bottom to use the new form template as shown in the following code snippet:

```
{CGFeedback key1="Dialogue" key2=$userid key3=$onecomment.id
 action="summary" sortorder="ASC"}
<hr/>
{CGFeedback key1="Dialogue" key2=$userid key3=$onecomment.id
 policy="session" commenttemplate="TicketDialogue"}
```