
## User Profiles

While user accounts are a requirement for the system to work, user profiles are a must for your community to work. A user profile allows your users to share all sorts of details about themselves. It should be very flexible so that you can easily extend the capabilities of your users' profiles as your community matures and morphs over time.

In this chapter we will discuss the basics of setting up a user profile. This will include collecting various personal tidbits of data about your users, contact information, and so on. We will also go over the concept of allowing your users to upload an **avatar** (an icon or image associated with a user's profile) as well as integrating with a third-party avatar service, **Gravatar**. Part of the uploading of a custom avatar will take us into some image manipulation so that all avatars are of equal size and shape on our site!

With this out of the way we can move on to putting the user's public profile together and discuss how that profile can be accessed with a custom homepage or fancy URL. Once we have collected some data about our users—for other users of our site to see—we will need to discuss giving our users control of their privacy settings and allowing them to lock down the display of that data.

The last part of this chapter will get into the creation of a news feed / alert, that keeps track of what our users are doing. This will be a very important feature once the concept of Friends comes into play in the following chapter.

![](img/15a62e134135dde036b0ac3cbb01dab6_130_0.png)

### Problem

We are not going to discuss the profiles capability of .NET for the same reason that we chose not to write about the membership capabilities of .NET. This topic is widely covered in many books and on many of the top blog sites. We thought it might be more interesting to discuss a custom implementation of creating a profile and dynamic attributes so that a member's profile could be expanded with time. Also, this will get us a bit deeper into LINQ and how our framework works.

Another interesting topic that must be discussed absolutely while building profiles is avatars. An avatar is a small icon or image that is associated with each user's profile. This allows you to visually pick a profile out of the group with ease. Generally, an avatar is displayed next to just about everything a user does or interacts with. This could be their blog posts, forum posts, comments, and so on. It provides a sort of virtual face-to-face feeling.

Of course, we could take an easier approach and not allow our users to create custom avatars—certain communities do this. We could just provide a gallery of canned generic avatars for a user to associate with their profile. But the biggest draw to any community site is its ability for the users to have as much free expression as possible. For this reason, we will discuss the other end of the spectrum when it comes to custom avatars.

We need a way for the users to upload their own pictures of just about any size (though we will have a file size constraint! No 10 MB images to process please!). Knowing that an avatar is usually closer to the size of an icon rather than a poster, we will need to tackle resizing an image. Also, as we know that images can come in just about any shape, it would be really cool if we could figure out a UI that allows the user to select a specific section of the uploaded image and constrain that selection to a specific shape. We would prefer a square shape as it is the easiest to work with.

![](img/15a62e134135dde036b0ac3cbb01dab6_131_0.png)

In addition to custom avatars, there are many services available these days that provide centralized avatars. This allows a user to upload and manage their avatars in one place and have them automatically feed out to all the sites that they are a member of. We will take a look at how to use one of the largest and most popular services, Gravatar.

Once we have all the data collection and profile configuration utilities out of the way, we will tackle the issue of displaying that data to our community. A good feature to take on here is the concept of a fancy URL, that will allow our users to have their own personal homepage within our community, something like this: www.fisharoo.com/profiles/atul gupta.

A very popular feature of Facebook is the concept of a continuous feed about you, your friends, and activities in the community. This sort of feature has become so popular that MySpace has recently implemented a version. This is an important feature as it gives us something new to look at on a daily basis. The more friends you are tracking (covered in Chapter 6), the more entries you will have to follow. As these feeds or alerts generally go on a user's private homepage, and as it is a major subsystem that many other features will dump into, we should address the underlying framework for this now rather than later.

### Design

Let's take a look at what the design for these features would look like.

#### Profile

Our profiles will collect basic data about a user. Some items that we will collect are:

- Various IM (Instant Messaging) IDs
- The users' post/comments
- Some basic fish-related information such as how many fish tanks they have and how many fishes they have

In addition to this we will allow our profiles to have dynamic attributes. While these could be dynamic from a user's point of view, we will currently restrict this feature to allow only an administrator of the site to add additional attributes. Some examples of dynamic attributes are "about you", "occupation", and "your (reef/aquarium) setup".

### Manage profile

As we will have several bits of data to manage, we need to consider how best to present this to the user. Also, we don't want to create a bunch of different screens to manage the profile as it will become a nightmare for us. We will use a simple step-by-step Wizard type interface so that we can break the data collection/ management process into logical steps.

#### Avatar

There are many ways to manage images in a system. The top two methods that have been debated heavily over the years are storing the images in the database or storing them on the file system. For everyday image galleries, we are all for storing them on the file system. However, something that is used as frequently as an avatar (given the diminutive size of an avatar) can be stored directly with the profiles in the database.

#### Custom avatars

Most of us would think that uploading an image or a file is simple. However, resizing an image in a way that that doesn't make it look all distorted is indeed a challenge. Moreover, we want to upsize and/or crop the image so that it is a perfect square. But we can't squish the user's image into a square. Also we can't just pick out a square portion of the uploaded picture at random! The avatar could end up having just the nose rather than the head. So we need to either create a UI (or find one) that allows the user to specify which section of the uploaded image to use.

#### Gravatar

Gravatar.com and other similar sites have another interesting idea. They allow you to store an avatar in one centralized location and reference it from other sites. This way, if you ever wanted to change your avatar, you could simply go to one location, make the change, and your avatar would be changed across all the sites that reference it. We felt this was something that we should support in our site.

#### Public profile

As people join the site, they would like to know about other members of the site and interact with them. We need to present a public area for our users to express and share with the world. This will be a major launching pad for our other features in the following chapters.

### Custom homepage

Most users of the Internet are used to seeing an URL that looks something like this:

http://www.domain.com/somepage.aspx?id=asduiw8921s1cm&t=89889

While computers can read this easily, it is absolutely meaningless to a human. As we already have a public profile page for each of our users, why not allow an easy way for curious folk to locate that page? More importantly why not provide an easy way for our users to share their public profile? Instead of the previous URL, we will make an URL that looks more like **http://www.fisharoo.com/profiles/YourUsername**.

### Privacy

Once we have provided our users with all these tools to enter their personal data, it is very important that we provide them with a way to manage who sees that data. In this chapter, we will focus on creating the base system that we can later extend upon in the following chapters. In this chapter, we will look at how to keep the user's data either public or private. The system that we will create should be flexible enough to allow us to protect a single piece of data, or an entire section of data. An example could be that we want to protect a user's social security data individually (we won't store socials though!) and protect all the users' IM accounts with one flag.

Here the flags are the actual user settings. The flag types are the specific data or areas of data to be protected. As this will be statically defined in the system, this data need not be directly manageable. The visibility level boils down to public, private, and friends-only. With this structure, the end user will be able to configure each flag type independently.

### News feed

As we had mentioned earlier, the news feed section is one of our favorite features of any community. We think if this area is created correctly, it will have the most activity and possibly the most value for the users. It essentially allows a user to keep track of other users.

There are several ways in which we can execute this type of functionality. We could keep the data scattered about for all the various types of notifications that we want to track. Or we could centralize it and disconnect it from the relational model. We are opting to keep things simple (as usual). So in our case, when a notification is generated it will be stored in a simple structure that will allow us to be more efficient with regards to getting this data quickly to our users.

Alerts are generated by each user. All these alerts will show up in a user's homepage so that he/she can see what they are up to chronologically.
