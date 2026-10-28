# # Manage profile

As we will have several bits of data to manage, we need to consider how best to present this to the user. Also, we don't want to create a bunch of different screens to manage the profile as it will become a nightmare for us. We will use a simple step-by-step Wizard type interface so that we can break the data collection/ management process into logical steps.

# ## Avatar

There are many ways to manage images in a system. The top two methods that have been debated heavily over the years are storing the images in the database or storing them on the file system. For everyday image galleries, we are all for storing them on the file system. However, something that is used as frequently as an avatar (given the diminutive size of an avatar) can be stored directly with the profiles in the database.

# ## Custom avatars

Most of us would think that uploading an image or a file is simple. However, resizing an image in a way that that doesn't make it look all distorted is indeed a challenge. Moreover, we want to upsize and/or crop the image so that it is a perfect square. But we can't squish the user's image into a square. Also we can't just pick out a square portion of the uploaded picture at random! The avatar could end up having just the nose rather than the head. So we need to either create a UI (or find one) that allows the user to specify which section of the uploaded image to use.

# ## Gravatar

Gravatar.com and other similar sites have another interesting idea. They allow you to store an avatar in one centralized location and reference it from other sites. This way, if you ever wanted to change your avatar, you could simply go to one location, make the change, and your avatar would be changed across all the sites that reference it. We felt this was something that we should support in our site.

# ## Public profile

As people join the site, they would like to know about other members of the site and interact with them. We need to present a public area for our users to express and share with the world. This will be a major launching pad for our other features in the following chapters.