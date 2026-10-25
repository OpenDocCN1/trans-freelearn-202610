```
box.style.top = ctop + 'px';
}

var popupMenuInitialised=false;
// Shows a box if it wasn't shown yet or is hidden
// or hides it if it is currently shown
function show_box(html, width, height, borderStyle,id)
{
    // Create box object through DOM
    var boxdiv = document.getElementById(id);
    boxdiv.style.display='block';
    if(popupMenuInitialised==false)
    {
        //boxdiv = document.createElement('div');
        boxdiv.setAttribute('id', id);
        boxdiv.style.display = 'block';
        boxdiv.style.position = 'absolute';
        boxdiv.style.width = width + 'px';
        boxdiv.style.height = height + 'px';
        boxdiv.style.border = borderStyle;
        boxdiv.style.textAlign = 'right';
        boxdiv.style.padding = '4px';
        boxdiv.style.background = '#FFFFFF';
        boxdiv.style.zIndex='99';
        popupMenuInitialised=true;
        //document.body.appendChild(boxdiv);
    }

    var contentId=id+'Content';
    var contents = document.getElementById(contentId);
    if(contents==null)
    {
        contents = document.createElement('div');
        contents.setAttribute('id', id+'Content');
        contents.style.textAlign= 'left';
        boxdiv.contents = contents;
        boxdiv.appendChild(contents);

    }
    move_box(html, boxdiv);

    contents.innerHTML= html;

    return false;
}
```