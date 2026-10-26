The RemoveTagPair() method removes the <a></a> tags but leaves the text between them. For example, if the link is: <a href="/NR/rdonlyres/8C78AC41-591D-4626-AFC8-19AC8375914E/0/myresource.gif">My Resource </a>, then running the text through the RemoveTagPair() method would leave only the string, My Resource.

```
private static string RemoveTagPair(string text, string tag, string endtag, string recursecompare)
{
    // Get the first occurrence of the <a tag within the content
    int start = text.ToLower().IndexOf(tag.ToLower());

    // Initialize the end point to -1
    int end = -1;

    while (start >= 0)
    {
        // Find the end point
        end = FindTag(text, endtag, start, recursecompare);

        // Got the end point. Remove the end tag, </a>
        if (end > start)
        {
            text = text.Remove(end, endtag.Length);
        }

        // Remove the start tag <a>
        text = text.Remove(start, tag.Length);

        // Continue the search for <a></a> tags
        start = text.ToLower().IndexOf(tag.ToLower());
    }
    return text;
}
```

The FindTag() method handles nested anchor tags. We may have a situation where one anchor tag is nested within another like this:

<a href="/NR/rdonlyres/8C78AC41-591D-4626-AFC8-19AC8375914E/0/myresource.gif">My Resources @ <a href="http://www.mywebsite.com">MyWebSite</a>. Check it out now!</a>

While nesting anchor tags does not reflect good programming, it is still a possible scenario, especially when authors use the built-in WYSIWYG editors and don't get a chance to see how the code looks in HTML.

FindTag() handles nested tags by checking to see if there are multiple <a></a> tag pairs. If there are, it continues its search until it finds the final ending tag, </a>, that matches the first. Add FindTag() directly below the RemoveTagPair() method.

```
private static int FindTag(string text, string tag, int start, string recursecompare)
{
    // Get the position of the next </a> tag
    int pos = text.ToLower().IndexOf(tag.ToLower(), start);

    // Check to see if there are nested hyperlinks
    int check = text.ToLower().IndexOf(recursecompare.ToLower(), start+1);

    // Found a nested <a></a> tag
    if ((check >= 0) & (check < pos))
    {
```