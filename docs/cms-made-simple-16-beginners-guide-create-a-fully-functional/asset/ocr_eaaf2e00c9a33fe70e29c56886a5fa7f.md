For more help, you may take a look at the FAQ. The link to the FAQ can be found in the module help above the Copyright and License section.

# Creating the product list template

For modules created with CTLModuleMaker, some sample templates are created automatically. However, for each level, you can create an unlimited number of templates by yourself.

# Time for action – creating a new list template

Let's customize the appearance of the product list and add products' images as thumbnails to this list.

+   1. In the admin console, click on Content | Products Catalogue, and choose the Templates tab.
+   2. Below the section Default Templates, you will find a list of sample templates. Click on the Add template link.
+   3. For the product list with thumbnails, enter the following code snippet in the Template field:
+   ```
+   {if $itemcount > 0}
+       <ul>
+           {foreach from=$itemlist item="item"}
+               <li style="list-style-type:none; float:left; margin: 20px;"><a href="{$item->detailurl}">
+               <img src="uploads/{$item->image->thumbnail}" alt="{$item->name}" /></a>
+               <br/>
+               {$item->detaillink}<br/>{$item->price} USD</li>
+           {/foreach}
+       </ul>
+   {/if}
+   ```
+   4. Click on Submit.