# Differences

However, even when authenticated, the results we get through a Graph URL search don't always match the results we see in a Full Search. This is particularly true when looking for people; by default, all apps are able to access an authenticated user's friends' names and IDs, but other information may be hidden.

For example, by entering an e-mail address as the query in a User search, we can find anyone with that exact email address – unless they've noted in their privacy settings that they don't want that information to be available to other apps. (See also the options in Account | Privacy Settings | Applications and websites | Information accessible through your friends, as described in Chapter 3.)

Sometimes this will cut particular results out entirely; sometimes it will make the same results appear, but in a different order. Remember this when testing: If the results you get when searching through your application are different from those you get when searching through the web site, that doesn't necessarily mean there's a bug in your application.

# Restrictions

The sub-options available when searching via a Graph URL are very limited when compared to the Full Search. We can't filter Users by their location or school, Groups by their type, or Events by their date (even using since/until paging).

Nor can we restrict Posts to a specific language, which makes them far less useful than they might be (as we saw when searching for "Packt", a common German word). But we can do something similar.

There is a parameter called locale which accepts languages like "en_US" (for US English) and "fr_FR" (for French). This can be used in two ways.

First, we can use it to transform information that we receive. For example, here's Facebook CTO Bret Taylor's User object:

```
https://graph.facebook.com/btaylor
{
  "id": "220439",
  "name": "Bret Taylor",
  "first_name": "Bret",
  "last_name": "Taylor",
  "link": "http://www.facebook.com/btaylor",
  "gender": "male",
  "locale": "en_US"
}
```