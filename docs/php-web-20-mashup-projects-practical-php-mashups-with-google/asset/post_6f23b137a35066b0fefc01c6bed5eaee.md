
## YouTube Developer API

Like some of the other web services we've encountered, YouTube's API requires a developer ID that needs to be passed to the server when calling services. You can sign up for a developer ID at http://www.youtube.com/signup?next=my_profile_dev. After you have an ID, you can dig into the documentation. The documentation can be found at http://www.youtube.com/dev_docs.

The available methods fall into two categories. A method can either be related to user information, or related to video viewing. YouTube uses a quasi-Java, dot notation to name their web service methods. For example, the method to get a user's profile is named youtube.users.get_profile, and the method to get a list of featured YouTube videos is named youtube.videos.list_featured.

Each method can be called either using REST or XML-RPC. A REST request takes the method name as a parameter along with any other parameters needed. The format used is http://www.youtube.com/api2_rest?method=METHOD_NAME&parameter1=VALUE1&parameter2=VALUE2. For example, let's look at get_profile, which gets a user's profile. The documentation for this method (located at http://youtube.com/dev_api_ref?m=youtube.users.getprofile) says this method requires three parameters:

- method is the method name itself. For get_profile, the value for method is the method's formal name, youtube.users.getprofile. This parameter is only needed for REST requests, which we are using in this example.
- dev_id, which is your developer ID.
- user, the profile name of the user whom you want to retrieve.

The REST request for get_profile would be http://www.youtube.com/api2_rest?method=youtube.users.getprofile&dev_id=YOUR_DEVELOPER_ID&user=PROFILE_NAME.

The same method names are used when interfacing with XML-RPC. To use XML-RPC, the request is a standard XML-RPC call that takes only one param element, a struct, and each YouTube method parameter is sent as a value of the struct. The name of the method is not passed into the struct. Instead, follow XML-RPC standards by putting the name of the method in the methodName element of the call. Under XML-RPC, the call to get_profile would look like this:

```
<?xml version="1.0" ?>
<methodCall>
    <methodName>youtube.users.get_profile</methodName>
    <params>
```
```
<param>
    <value>
        <struct>
            <member>
                <name>dev_id</name>
                <value>
                    <string>YOUR DEVELOPER ID</string>
                </value>
            </member>
            <member>
                <name>user</name>
                <value>
                    <string>PROFILE NAME</string>
                </value>
            </member>
        </struct>
    </value>
</param>
</params>
</methodCall>
```

The server will always return an XML response. Each method's response is detailed in the documentation. If REST is used, the response is just the XML in string form. If XML-RPC is used, that same documented XML string is escaped and encased in an XML-RPC response wrapper.

We can see how this works with get_profile. Consult the documentation to see how the response is structured. According to the documentation, this is how the response will be returned if we are using REST:

```
<user_profile>
    <first_name>Shu</first_name>
    <last_name>Chow</last_name>
    <about_me>I pound on a keyboard for a living</about_me>
    ...
    <friend_count>3</friend_count>
    <favorite_video_count>7</favorite_video_count>
    <currently_on>false</currently_on>
</user_profile>
```

If we made the call with XML-RPC, the XML structure would be the same, but this time, the XML is a string within a methodResponse element:

```
<?xml version='1.0' ?>
<methodResponse>
    <params>
```
```
<param>
  <value><string>
    <user_profile>
      <first_name>Shu</first_name>
      <last_name>Chow</last_name>
      <about_me>
        I pound on a keyboard for a living
      </about_me>
      ...
      <friend_count>3</friend_count>
      <favorite_video_count>7</favorite_video_count>
      <currently_on>false</currently_on>
    </user_profile>
  </string></value>
</param>
</params>
</methodResponse>
```

As you can see, the YouTube API is simple, consistent, and well-documented.

## Last.fm Overview

Last.fm (http://www.last.fm) describes itself as a social music networking site. It is an interesting way to find new music based on user contributions. Users register for free at the site. You then download a small client program to run on your machine. This program monitors what music you are playing through software media players such as iTunes and WinAmp. You can also tag the songs through the program. The program uploads the song information, including tags, back to the Last.fm server. Based on the artist, song, genre, and what other people have tagged songs, Last.fm builds a music profile for you. It uses this profile to build a custom streaming radio station for you that you can also listen to through the client program, and recommends other artists and songs that are similar to the music that you like.

![](img/c637937d44400f1da21fb7cca101de80_156_0.png)

> Vigilant users may, understandably, bristle at installing a desktop program that sends their music listening habits off to a server somewhere. However, Last.fm does a good job of protecting your privacy. First off, the client program is open source. It does not contain any malware that would compromise your privacy or your computer's security, and the code is opened up for peer review. Second, their privacy policy explicitly states they do not share your personal information to outside parties. Finally, the registration process does not ask for information such as address or names, although you can enter your zip or postal code to see music events in your area. The music profile that it generates is tied to your account and profile you create. Any private information regarding you, the individual, is not required.

As we walk through the project and the examples, I will be demonstrating things using my own personal account. This will guarantee that the examples indeed work. To truly personalize this project as your own, you should consider creating an account and uploading some track data through the Last.fm client program.

## Audioscrobbler Web Services

Audioscrobbler Web Services API is the web service that allows you to access data displayed on Last.fm. Audioscrobbler's home is at http://www.audioscrobbler.net/data/webservices. This web service is basically a collection of RSS feeds that fall within several categories:

- **User Profile Data**: The largest collection of feeds, this category provides data about a certain user. For example, there are feeds that list the top artists they've listened to, and the top tracks.
- **Artist Data**: This category aggregates data about all artists in the Last.fm database. By providing an artist name, you can get things such as their most listened to tracks on Last.fm, their largest fans, the users that have listened to them the most, etc.
- **Album Data**: This category holds information about albums. Currently, there is only one feed associated with this category. By providing a title name to the Info feed, you can get a track listing for the album.
- **Track Data**: Information about specific tracks is given here.
- **Tag Data**: Like YouTube, Last.fm allows users to tag songs with their own descriptions.
- **Group Data**: Like other social networking sites, Last.fm provides groups users can create and join. Information about what members are listening to in these groups is available in the Group Data feeds.
- **Forum Data**: Last.fm has a community forum. The posts are offered in an RSS feed.
- **LiveJournal Protocol**: Audioscrobbler can interact with the LiveJournal web service through this web service.

Looking at the Audioscrobbler Web Services API home page, we see that each feed can be in up to four formats — plain text, generic XML, XSPF, or RSS. Often, the available formats are dictated by their purpose. For example, it would not make sense to output user information in XSPF, a format made just for representing song playlists.
