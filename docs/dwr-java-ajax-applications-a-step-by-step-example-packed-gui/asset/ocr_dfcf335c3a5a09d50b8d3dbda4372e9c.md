```
```javascript
var contentFunctions='';
var scriptToBeEvaled='';
var contentHtml='';
for(var i=0;i<htmlArray.length;i++)
{
    var html=htmlArray[i];
    if(html.toLowerCase().indexOf('<script>')>-1)
    {
        if(html.indexOf('TO BE EVALED')>-1)
        {
            scriptToBeEvaled=html.substring(html.indexOf('>')+1,
                                            html.indexOf('</'));
        }
        else
        {
            eval(html.substring(html.indexOf('>')+1,html.indexOf('</')));
            contentFunctions+=html;
        }
    }
    else
    {
        contentHtml+=html;
    }
}

contentScriptArea=dwr.util.byId("contentAreaFunctions");
contentScriptArea.innerHTML=contentFunctions;
contentArea=dwr.util.byId("contentArea");
contentArea.innerHTML=contentHtml;
if(scriptToBeEvaled!='')
{
    eval(scriptToBeEvaled);
}
}
</script>
</head>
<body onload="loadMenuItems()">
<h1>DWR Easy Java Ajax Applications</h1>
<ul class="basictab" id="dwrMenu">
</ul>
<div id="contentAreaFunctions">
</div>
<div id="contentArea">
</div>
</body>
</html>
```
```