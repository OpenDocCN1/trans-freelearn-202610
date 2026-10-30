
## 13 AI-Powered Recommendations for Learners

In Adobe Captivate Prime, the primary way for learners to take courses is to browse the course catalog and enroll themselves in the courses that they are interested in. This is what makes Captivate Prime a learner-centric LMS. However, Captivate Prime is also an enterprise LMS. As such, it should help organizations of all sizes manage their learning strategies by ensuring that the skills available within the organization match the skills required by their strategy, by the industry, and by local and international regulations.

So, Captivate Prime is on a mission to bridge the gap between the courses available in the course catalog, the demands of the organization, and the individual interests of each learner. To accomplish this complex task, Captivate Prime relies on a key concept: **skills**. On the one side, learners take courses to improve their skills or to acquire new skills. On the other side, organizations need to ensure that the skills available in their workforce are aligned with the organization's strategy, with the skills required by the industry, and with the rules and regulations that they should comply with.

Another important aspect to consider is that Captivate Prime has been designed to handle large deployments with thousands of learners and huge course catalogs, containing tens of thousands of courses provided by in-house instructors, third-party vendors, or that have been migrated over from a previous LMS. This huge amount of data and the corresponding opportunities for interaction are precisely what makes things both interesting and slightly complicated. Learners need to be guided through these huge course catalogs to find the exact course they need based on their profile, job description, past activities, peer activities, and personal interests, all while keeping an eye on the organization's strategy, industry developments, regulatory environment, and more.

This is not an easy challenge. How can you maintain the list of skills available on your Prime account when your course catalog is mostly made of third-party off-the-shelf content? How can you recommend courses to each of your learners when the size of your organization means that you have a wide variety of occupations and job profiles to train? How can you ensure that the skills available within your organization are aligned with the ever-changing demands of your industry? How can you tweak the course recommendations made to learners in order to implement the learning and development strategy specific to your organization? Those are the main problems that the **artificial intelligence (AI)** and **machine learning (ML)** algorithms built into the Captivate Prime platform help you resolve.

In this chapter, you will revisit the learner home page and discuss the various types of recommendations that are available. You will then discover the inputs that the artificial intelligence considers when providing highly individualized recommendations specific to each learner. Some of these inputs are automatically curated by the artificial intelligence without any administrator contribution; these are called *implicit inputs*. Others are manually entered into the system by administrators or even by the learners themselves. These are called *explicit inputs*. Lastly, other inputs are influenced by the administrator's choices while remaining largely automated.

To discuss these features and processes, this chapter contains the following main sections:

- A word on Adobe Sensei
- Revisiting the learner home page
- Using industry-aligned skills
- Recommendations based on peer activity
- Using manual recommendations

While reviewing the exercises of this chapter, bear in mind that AI needs a very large quantity of data to become relevant. In other words, the features discussed in this chapter are most useful on large accounts with hundreds of learners taking thousands of courses. This is probably not the case on the trial Prime account you are using while reading this book.

### Technical requirements

To review the exercises of this chapter, you will need to meet the following technical requirements:

- You must have administrator and learner access to a working Captivate Prime account (this can be a trial account) using two different user profiles (one administrator profile and one learner profile).

If you have completed the exercises in the previous chapters, you are good to go. If not, don't worry. You can use your own course content and user accounts. Just remember that the instructions provided in this chapter are based on the exercises from previous chapters, and so, you might need to adapt them to your specific situation.

### A Word on Adobe Sensei

Adobe Sensei is Adobe's in-house artificial intelligence (AI) and machine learning (ML) technology. It was first introduced in November 2016 and was developed in order to compete with other large technology players (such as Amazon, IBM, Microsoft, and Apple) who have also developed their own AI/ML solutions. Adobe has developed Sensei with specific use cases in mind so that it can be embedded in a wide variety of Adobe branded applications and solutions.

> **Note**
> Sensei is the Japanese word for a respected teacher or leader.

Nowadays, Adobe Sensei powers dozens of features in a wide range of solutions and applications developed by Adobe:

- Adobe Sensei is used in a variety of Adobe Creative Cloud applications, powering features such as Content-Aware Fill in Adobe Photoshop, enhanced search capabilities in Adobe Stock, the color matching algorithm in Adobe Color, advanced video effects in Adobe Premiere Pro and After Effects, and more.
- Adobe Sensei is at the core of the **Adobe Experience Cloud**, powering features such as predictive analytics, advertisement targeting, personalized content, recommendations, search engines, and more.
- Adobe Sensei is also present in the **Adobe Document Cloud** to help improve the scanning accuracy of PDF documents, simplify the filling out of PDF forms, or assist in the signing of PDF documents using Adobe Sign.

As you can see, Sensei has become a key technology in some of Adobe's most prolific products by powering some of its most remarkable, cutting-edge features.

When it comes to Captivate Prime, Adobe Sensei is the behind-the-scenes technology that powers the automatic curation process (as discussed in *Chapter 12, Enabling and Managing Social Learning*), as well as the powerful recommendation engine, which is the subject of this chapter.

More information on Adobe Sensei technology can be found on the official Adobe website at https://www.adobe.com/sensei.html.

### Revisiting the learner home page

Always bear in mind that Captivate Prime is a learner-centric LMS, where the primary way to enroll in a course is through self-registration. This means that each learner can log into the Prime account, browse the course catalog, and enroll in the course that they find most relevant. How learners determine which course is relevant depends on many factors such as personal interests, career plan, position in the organization, recommendations from peers, training history, etc.

The artificial intelligence and machine learning (AI/ML) algorithm built into Captivate Prime is meant to assist learners and organizations in discovering the most relevant courses based on a wide range of factors.

These recommendations are displayed on the learner home page using various widgets that the administrator can arrange on the page in many ways. In this section, let's review the learner home page along with the administrator options that allow you to customize it. To do so, you need to log into your Prime account twice: once as an administrator and once as a learner using the following steps:

- 1. Open a browser window and log into your Prime account as an administrator.
- 2. Open another browser, or a private window of the same browser, and log into your Prime account a second time as *Learner 01*.

Make sure you keep these two browser windows open for this entire chapter.
