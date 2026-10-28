# WebContext

There are a few changes to the WebContext class that we will discuss here.

# SelectedRatings

The SelectedRatings property handles a Dictionary<int, int> collection that is stored in the Session. This collection is responsible for handling ratings that are collected for various options attached to an object. The first int is the RatingOptionRecordID and the second int is the Rating (score) that was applied to that SystemObjectRatingOptionID. This collection is built up prior to the user saving their rating options. What we mean to say is that every time a user adds a dictionary item to the SelectedRatings property, it adds the item to a collection stored in the Session rather than resetting the value all together. We will see how this plays out once we start to build our Ratings control.

If you are wondering about how this collection is handled—if it continues to grow—you should know that there is another method (coming up next) that handles the clearing of this collection.

```
//Fisharoo/Components/WebContext.cs
public Dictionary<int, int> SelectedRatings
{
    get
    {
        Dictionary<int, int> result = new Dictionary<int, int>();
        if(GetFromSession("SelectedRatings") != null)
        {
            result = GetFromSession("SelectedRatings") as Dictionary<int, int>;
        }
        return result;
    }
    set
    {
        //make sure that we add to the existing rating store rather
        //than creating a new one
        Dictionary<int, int> result = new Dictionary<int, int>();
        if (GetFromSession("SelectedRatings") != null)
        {
            result = GetFromSession("SelectedRatings") as Dictionary<int, int>;
            foreach (KeyValuePair<int, int> pair in value)
            {
                if (!result.ContainsKey(pair.Key))
```