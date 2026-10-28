```
"picture": "http://profile.ak.fbcdn.net/hprofile-ak-snc4/hs302.ash1/23274_204603129458_7460_s.jpg",
"link": "http://www.facebook.com/PacktPub",
"category": "Products_other",
"username": "PacktPub",
"company_overview": "«overview»",
"fan_count": 437
}
```

# What just happened?

We passed the IDs of two Pages at once to the Graph, using the ids parameter, and it returned a JSON containing the basic information of both of them.

So this is a List, right?

Wrong!

Well, it's not a List according to the definition we've been using so far, as there's no data node. I'll call it a Compound Object. Compare the above to the result you get if requesting the Facebook Page directly:

```
https://graph.facebook.com/facebook
{
  "id": "20531316728",
  "name": "Facebook",
  "picture": "http://profile.ak.fbcdn.net/hprofile-ak-snc4/hs624.ash1/27535_20531316728_5553_s.jpg",
  "link": "http://www.facebook.com/facebook",
  "category": "Technology",
  "username": "facebook",
  "founded": "February 4, 2004",
  "company_overview": "«overview»",
  "mission": "Facebook's mission is to give people the power to share and make the world more open and connected.",
  "fan_count": 12672338
}
```

You can see that when requesting more than one Graph object at once, the objects are jammed together as sub-nodes of a single object, each labeled with their ID.

Naturally, you don't have to use Page IDs; you can use the ID of any object in the Graph, including Albums, Photos, and Users. What's more, you can mix the types of ID you request, so you could request an Event, three Users, and a dozen Photos all at once, and they'll be returned in the same JSON.