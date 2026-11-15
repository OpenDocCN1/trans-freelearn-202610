
I hope that this section has given you some ideas on how you can improve and impress with PowerPoint. Using these newly acquired techniques, you might not even need Prezi after all. However, this chapter wouldn't be complete without mentioning story points, a feature that will bring you a long way if you don't want to use PowerPoint at all.

# Story points and dashboards for presentations

**Story points** are Tableau's answer to PowerPoint. As such, both share fundamental similarities. A PowerPoint presentation provides a linear approach to communication. So do story points in Tableau. A PowerPoint presentation provides slides, each of which is a blank canvas that provides a user with infinite possibilities to communicate ideas. A story in Tableau provides story points for the same purpose.

Although there are fundamental similarities, it is important to understand that Tableau story points and PowerPoint presentations often do not fulfill the same role. Each has advantages over the other, and so a list of pros and cons should be considered.

We can view the pros and cons of PowerPoint and Tableau story points in the following table:

| | PowerPoint | | Tableau story points | |
| --- | --- | --- | --- | --- |
| Pros | Cons | Pros | Cons |
| Can be quick and easy to create | Difficult to automate based on changes to underlying data | Automatically updates as underlying data changes | Can be more difficult to create |
| Easily fits different resolutions | Difficult to create a presentation that allows for a departure from linear thought | Easily allows nonlinear exploration in the middle of a presentation | Requires forethought regarding resolution size |

So how do you decide between using PowerPoint and Tableau when making presentations? Perhaps the following list will help:

| When to use PowerPoint | When to use Tableau story points |
| --- | --- |
| A presentation where the audience has little opportunity for immediate feedback | A presentation where immediate feedback is likely and exploratory questions may be asked |
| A one-time presentation comprising clearly delineated points | A reusable presentation with updates mostly based on changes to underlying data |
| A presentation where the monitor resolution size may not be known in advance | A presentation where the monitor resolution size is known or assumed |

The basics of creating and using story points are straightforward, and also, at the time of writing, there is not much beyond the basics. Since the focus of this book is on more advanced Tableau ideas and concepts, those basics will not be considered here. If you need to quickly understand story point basics, I recommend reading through the following pages: <https://help.tableau.com/current/pro/desktop/en-us/stories.htm>.

I like story points and I like to use them especially when my audience has its own version of Tableau open in front of them and can follow along with their own actions. It is a great way to give people a deeper understanding because it allows you to show more complexity than a static screenshot. If you are looking to improve your presentation skills in general, continue reading the next section.

# Presentation resources

Using story points effectively in Tableau is perhaps more of an artistic consideration than a technical one. Although this book attempts to consistently consider best practices for data visualization and to encourage attractive and user-friendly worksheets and dashboards, a discussion of effective presentation techniques is beyond its scope.

If you would like to improve your ability to create effective presentations, consider the following resources:

*   **Tableau story points**: Learning by example is invaluable. Fine examples of story points can be found on Tableau. To learn from one of the most engaging, navigate to <https://help.tableau.com/current/pro/desktop/en-us/story_example.htm>.
*   **TEDx talks**: TEDx talks are great for learning and can even be listened to while commuting or being away from your desk in general. One video I would recommend is *The 3 Magic Ingredients of Amazing Presentations* by Phil Waknell: <https://www.youtube.com/watch?v=yoD8RMq2OkU>.
*   **Blogs**: One of the more interesting posts discussing Tableau story points in combination with good presentation methodology can be found here: <https://www.artofvisualization.com/blog/10-secrets-perfect-tableau-story>.

Even though these presentation skills are beyond the scope of this chapter, I do hope that the preceding resources have given you food for thought.

# Summary

We began this chapter by exploring various screen capture tools as well as Tableau's native export capabilities. Next, we turned our consideration to PowerPoint, where we explored various methods for creating PowerPoint presentations from Tableau workbooks, and even explored how to embed a live instance of Tableau into PowerPoint. Next, we considered Tableau animation. Lastly, we explored how to use story points and dashboards for presentations. In *Chapter 11*, *Designing Dashboards and Best Practices for Visualizations*, we will turn our attention to dashboarding, where we will push beyond the normal boundaries of the typical dashboard, but not lose focus of the practical applications.

# Learn more on Discord

To join the Discord community for this book – where you can share feedback, ask questions to the author, and learn about new releases – follow the QR code below:

<https://packt.link/tableau>

![](`Images/QR_Code2044012316175764640.png`)

# 11

# Designing Dashboards and Best Practices for Visualizations

Now, we will delve into the essential principles and best practices of data visualization, focusing specifically on dashboard design. By exploring how to present data in a visually compelling and informative manner, you will gain the knowledge and learn techniques that will help to create impactful dashboards to effectively communicate insights, drive decision-making, and engage your audience. This chapter was influenced by some giants in data visualization: *The Functional Art* by Alberto Cairo, *Information Dashboard Design* by Stephen Few, *The Big Book of Dashboards* by Steve Wexley, Jeffrey Shaffer, and Andy Cotgraeve, *Universal Principles of Design* by William Lidwell, Kritina Holden, and Jill Butler, *Data Story* by Nancy Duarte, and *The Visual Display of Quantitative Information* by Edward Tufte.

In this chapter, we will cover the following topics:

*   Visualization design theory
*   Formatting rules
*   Color rules
*   Visualization type rules
*   Keeping visualizations simple
*   Dashboard design
*   Dashboard best practices
*   Something extra

Now that we have defined the topics of this chapter, let's dive into design theory, which will provide you with rules that can be applied to every single dashboard you will build.

# Visualization design theory

Your visualization thought process should always begin with the basics of design from the lowest level. In other words, we will start talking about the worksheets displayed on dashboards and ensure that those worksheets are well designed.

Therefore, our discussion will begin with a consideration of visualization design principles, such as the following collection, which is inspired by the *7 principles of design*: <https://www.turing.com/kb/what-are-the-7-principles-of-design-detailed-breakdown>:

*   **Emphasize** what is most important
*   **Amplify** important data points
*   **Align and balance** elements with appropriate use of color, size, and texture
