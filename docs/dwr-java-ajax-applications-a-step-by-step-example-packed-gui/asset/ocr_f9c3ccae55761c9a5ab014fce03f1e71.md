```
}
    });
}
}

function refreshChapterIfVisible(bookTitle)
{
    var chaptersForTitle=dwr.util.byId('chaptersForTitle');
    if(chaptersForTitle!=null || chaptersForTitle!=undefined)
    {
        if(chaptersForTitle.innerHTML.indexOf(bookTitle)>-1)
        {
            getChaptersForTitle(bookTitle);
        }
    }
}

function clearChapterViewIfVisible(bookTitle)
{
    var chaptersForTitle=dwr.util.byId('chaptersForTitle');
    if(chaptersForTitle!=null || chaptersForTitle!=undefined)
    {
        if(chaptersForTitle.innerHTML.indexOf(bookTitle)>-1)
        {
            clearContentAreas();
        }
    }
}

function notifyBookPublished(bookTitle)
{
    alert('Book '+bookTitle+' is published. Authors have voted.');
    clearChapterViewIfVisible(bookTitle);
}

function getChapterContent(bookTitle,chapterTitle)
{
    var readonly="";
    var editor="";
    BookDatabase.isChapterEdited(bookTitle,chapterTitle,{
        async:false,
        callback:function(isEdited)
        {
            if(isEdited!=null)
            {
                readonly="readonly";
```