+   2. We also need to specify the parent element of the panel by setting the `data-parent` attribute's value to the ID of the parent panel group. Next, set the anchor element's `href` attribute to the ID of the `<div>` element that contains the content.
+   
+   3. Finally, we also need to set the panel body element's class to `panel-collapse collapse`. In the following code, we'll create an accordion component that contains two panel groups. The first panel group is automatically visible when the page loads because we'll set its class to `panel-collapse collapse in`:

```
```html
<div class="panel-group" id="accordion">
    <div class="panel panel-default">
        <div class="panel-heading">
            <h4 class="panel-title">
                <a data-toggle="collapse" data-parent="#accordion"
                   href="#questionOne">
                    How do I order from you?
                </a>
            </h4>
        </div>
        <div id="questionOne" class="panel-collapse collapse in">
            <div class="panel-body">
                Mauris egestas sem a ante accumsan tincidunt.
                Vivamus id nisl vulputate, placerat arcu et.
                sapien.
            </div>
        </div>
    </div>

    <div class="panel panel-default">
        <div class="panel-heading">
            <h4 class="panel-title">
                <a data-toggle="collapse" data-parent="#accordion"
                   href="#questionTwo">
                    Do you ship internationally?
                </a>
            </h4>
        </div>
        <div id="questionTwo" class="panel-collapse collapse">
            <div class="panel-body">
                Integer aliquet ligula eget elit faucibus, quis
                pretium justo iaculis. Quisque quis dolor
                volutpat, varius tellus eget, luctus nibh.
                Cras tempus tincidunt.
            </div>
        </div>
    </div>
</div>
```
```