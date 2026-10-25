```
function cancelEditNotes(ccode)
{
    var countryData=CountryDB.getCountryData(ccode, {
    callback:function(data)
    {
        var notes=data[4];
        if(notes==undefined)
        {
            notes='&nbsp;';
        }
        var html=getNotesHtml(ccode,notes);
        var tdId=ccode+'_NotesCell';
        var td=dwr.util.byId(tdId);
        td.innerHTML=html;
    }
    });
}
function saveCountryNotes(ccode)
{
    var editBox=dwr.util.byId(ccode+'NotesEditBox');
    var newNotes=editBox.value;
    CountryDB.saveCountryNotes(ccode,newNotes, {
    callback:function(newNotes)
    {
        var ccode=newNotes[0];
        var notes=newNotes[1];
        var notesHtml=getNotesHtml(ccode,notes);
        var td=dwr.util.byId(ccode+"_NotesCell");
        td.innerHTML=notesHtml;
    }
    });
}
```

There are lots of functions for table samples, and we go through each one of them.