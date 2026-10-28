# Level of experience

Once we have our base profile container in place, we can create some of the surrounding tables. The easiest of which is the level of experience system. This will be used as other features of the site are built. Initially, a user can come in and set up his/her profile and claim a level of experience. Then as the user adds posts to the forum, creates new blog entries, or interacts with our community in any other way, we can adjust the user's level of experience dynamically. This can be based on the medals earned by the user, which we will discuss in Chapter 11.

# The Attributes table

The next easiest portion of the profile system is the dynamic attributes system. Recall that this will allow the site administrator to easily extend the data that is collected for each user. There are two tables—ProfileAttribute and ProfileAttributeType. The first one is for user entered attributes and the second one is for the various attributes that are used in the site.

# The Privacy table

Now we can discuss the privacy system. This is simply a one-to-many relationship system with some configuration/lookup tables on the child side of the relationship. We have PrivacyFlag for storing the user created values. We then have PrivacyFlagType to define what the flag is protecting. And finally, we have the VisibilityLevel, which defines who can see the protected data.

# The Alerts table

Now, we are on to the tables that will support our news feed or "Alerts" concept on the profile page. This includes the Alert table and the AlertType table.

An interesting field to discuss here is the IsHidden field. We will use this later so that a user can hide an alert from his/her news feed. We can also use it so that the user can hide a whole set of alerts based on its alert type.