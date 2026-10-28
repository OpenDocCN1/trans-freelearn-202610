# What about connections?

So that's essentially the FQL equivalent of retrieving a single Graph Object. But what's the equivalent to getting a Graph Object's connections?

Understanding the way connections are formed in a database model is a much harder concept to grasp than with the Graph API model. (I wouldn't be surprised if this were one of the main motivations beyond the Graph API.) You have to work backwards, in a sense.

Let's use the idea of an album full of photos as an example. First, imagine we create an empty album called "Michael's Awesome Vacation Photos." It'll look like this, in the database model:

| album |
|---|---|---|
| aid | owner | name |
| «automatic» | «my user ID» | Michael's Awesome Vacation Photos |

The `album` table has a `cover_pid` field, which is set to the ID of a picture that I want to set as the cover photo. So let's suppose I take a picture of the airport and upload it as my cover photo; it'll be auto-assigned an ID.

Now we can enter the airport photo's ID as the value for the album's `cover_pid` field:

| album |
|---|---|---|---| 
| aid | owner | name | cover_pid |
| «automatic» | «my user ID» | Michael's Awesome Vacation Photos | «ID of airport photo» |

We can illustrate this like so:

![](bbox=[0.35, 0.61, 0.66, 0.79])