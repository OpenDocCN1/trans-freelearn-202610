Since these paging links are bundled with the JSON, they're often easier to implement than working out our own system based on `limit` and `offset` parameters. That's not really true in our case, because we would have to extract the `since` and `until` parameters from the `next` and `previous` URLs and then feed them to a `GraphRequest` object – it would be easier to extract the dates of the earliest and latest Graph Objects in the collection, and use those.

Still, this may come in useful for other projects you work on in the future, so it's worth mentioning.

# Requesting multiple IDs at once

Earlier in the chapter we learned how to retrieve a list of wall posts from a specific date range. How would you get the basic information about all the people that wrote these posts?

Based on what we've seen so far, the obvious answer is to loop through all of the IDs of all of the people, and request the User object for each of them.

That's fine, but it's a bit clunky, and we have no way of associating those people with each other. Wouldn't it be nicer if we could obtain them all in a single List?

# Time for action – using the ids parameter in a Graph URL

Load the following URL in your browser:

```
https://graph.facebook.com/?ids=facebook,packtpub
{
  "facebook": {
    "id": "20531316728",
    "name": "Facebook",
    "picture": "http://profile.ak.fbcdn.net/hprofile-ak-snc4/hs624.ash1/27535_20531316728_5553_s.jpg",
    "link": "http://www.facebook.com/facebook",
    "category": "Technology",
    "username": "facebook",
    "founded": "February 4, 2004",
    "company_overview": "«overview»",
    "mission": "Facebook's mission is to give people the power to share and make the world more open and connected.",
    "fan_count": 12672143
  },
  "packtpub": {
    "id": "204603129458",
    "name": "Packt Publishing",

```