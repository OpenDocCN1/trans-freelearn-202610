
3. **Give it structure:** Group related tasks together so the checklist is easy to use.
4. **Include helpful resources:** Link to relevant tools, browser extensions, or testing instructions to help people complete each requirement.
5. **Iterate and update:** Improve the checklist over time based on feedback. Make changes as best practices evolve, and use versioning to indicate when updates are made.

> Most people are not accessibility experts. Write checklists and resources using plain language, and demonstrate testing methods in multiple formats (e.g., screenshots, videos, in writing). Help people feel confident, not confused.

However, keep in mind that checklists are just a helpful starting point. As mentioned in *Chapter 2*, accessibility itself is not just a checklist. It’s a commitment to quality, usability, and people. After all, digital accessibility impacts both the people who build the technology and those who use it.

If you are in the process of integrating accessibility into development workflows, remember that change is not easy. Everything in this chapter has a tangible impact on both the organization and the customer. We discussed the importance of prioritizing feedback from customers, but don’t miss out on the voice of your fellow designers, developers, and QA. Their feedback on tooling, checklists, and processes can lead to tailored improvements. Plus, listening to people builds relationships, trust, and better work environments.

Strive to make expectations actionable and manageable. You can do this by providing developers and QA engineers with the right tools and information to perform accessibility tasks as part of their day-to-day work.

Embedding accessibility into existing development workflows not only enables the creation of more inclusive, user-friendly products but also helps shape a culture of accessibility-first thinking. Ultimately, the end goal is to prevent new accessibility barriers from reaching end users. Find what works for your team, collect feedback along the way, and make iterative improvements.

## Summary

This chapter laid out the basics of ensuring your website or mobile app will be usable for people with disabilities, focusing on testing tools and techniques. We explored the dynamic duo of automated and manual testing methods, the importance of prioritization, and how to bring accessibility into development workflows.

The topics covered here are foundational to building a successful digital accessibility program, especially in the area of quality assurance. Effective testing and remediation ensure that inclusive design translates into a high-quality user experience. Accessibility is a journey that requires continuous monitoring, effort, and collaboration across all teams. Whether it is one line of code, one test case, or one bug – every small action adds up.

In the next and final chapter, you will look at how you can embed accessibility strategies into organizations.

## 12 Building an Inclusive Design Culture: Strategies for Organizations

By Denis Boudreau, CPWA

Here we are, down to the last chapter. Let me begin by repeating a lesson discussed throughout this book: inclusive design thrives when accessibility is embedded in an organization’s culture, not when it’s treated as an afterthought or a last-minute fix. Organizations that build accessibility into their workflows, track meaningful progress, and invest in continuous learning are better equipped to meet the needs of all users, including those living with various disabilities. Teams that recognize accessibility as a shared responsibility rather than a constraining obligation end up creating stronger, more inclusive digital experiences for everyone. Period.

As a way to wrap up this book, this chapter will explore practical strategies to make accessibility a lasting part of how organizations work. This transformation begins with building empathy through immersive experiences and by helping cross-functional teams understand the real impact of accessibility barriers on the audiences they are expected to serve.

In this final chapter, we will focus on four main strategies to build an inclusive design culture:

- Cultivating empathy through immersive experiences
- Integrating accessibility into design and development workflows
- Measuring and reporting on accessibility progress
- Weaving continuous learning into inclusive design

Whether you’re leading an organization, managing a team, or are directly involved in the design and development phases of digital products or services, the strategies presented in this chapter will help you move beyond awareness and straight into action. Teams that embed accessibility into their daily decisions are the ones who successfully create experiences that work for everyone.

After all, as the late John Slatin used to say, “*Good design is accessible design.*”

### Cultivating empathy through immersive experiences

I want to begin by sharing a few thoughts on empathy, as empathy is one of the strongest drivers for inclusive design. Not the feel-good “let’s all be nice to each other” version, but the raw, eye-level understanding that comes from witnessing real people struggle.

When design teams witness end users running into accessibility barriers, disability inclusion quickly goes from a constraining theoretical set of requirements to an eye-opening, meaningful shared responsibility.

Most experts would agree that organizations that actively embrace inclusive design are also the ones who ultimately reach more customers, reduce their legal risks, and stay ahead of changing user needs and expectations. That has certainly been my observation over the years.

And while accessibility training sessions, governance policies, and role-based checklists certainly have the potential to go a long way, truly transformational change doesn’t happen through an accessibility audit. It happens through engaging directly with those who have relevant lived experiences and consume the world differently.

### Understanding accessibility barriers firsthand

Most professionals in design, development, and leadership roles will probably never use a screen reader. Very few are even likely to one day navigate a website without their mouse or fill out a form using only their voice.

Without such firsthand exposure to alternative, yet valid means of navigating the digital space, it’s no surprise that for most people, accessibility feels disjointed from design.

Creative teams that interact with digital products the way people with disabilities do can more clearly see the gaps and challenges introduced by a blatant lack of digital inclusion. This realization leads to more thoughtful and inclusive design decisions, which, in turn, lead to more inclusive experiences online.

A few years ago, I guided a UX team at a major software company that needed to improve the accessibility of their platform but wasn’t sure where to start. As part of our engagement, I guided them through a workshop where they spent hours navigating their platform using only a keyboard and a screen reader, capturing observations along the way.

As an experiment, we also provided them with blindfolds, so they could go about uncovering major usability issues that had never been on their radar up until that point. Confusing button labels, missing alt text, keyboard navigation traps, and dozens of other accessibility issues suddenly became painfully apparent.

More than minor inconveniences, these issues were real barriers that prevented real people from using the platform independently. While unnoticed by the UX team up until that point, these issues had nonetheless been plaguing the user experience of people with disabilities for months, if not years.

This was no doubt a jaw-dropping experience for the team, but I knew better, so I had one more surprise in store for them. Once they were done experimenting with alternative ways of navigating their websites, I introduced them to Tim, a blind colleague of mine.

Using the exact same tools (minus the blindfold, which I knew was a misleading idea), my colleague went on to uncover a series of additional problems the team had failed to identify, such as poor use of ARIA landmarks, lack of feedback on dynamic interactions, overuse of ambiguous link text, unclear call to action states, and missing text alternatives to otherwise purely visual cues.

What stood out most to everyone was the nature of the issues Tim identified compared to those they had found. While the blindfolded team uncovered many surface-level barriers, Tim’s insights went far deeper. He noticed systemic navigation issues, gaps in feedback, and missing structural elements that only someone with lived experience would know to look for. His muscle memory, mental mapping, and intuitive expectations, shaped by years of using assistive technology, gave him access to problems that simulations simply couldn’t replicate.

I knew this was a lesson the team would only understand by going through the motions. Simulation exercises sometimes raise awareness, yes, but they don’t lead to sustainable, real-world solutions. They might start the conversation, but they can’t replace the insights that come from lived experience because true accessibility comes from listening to (and learning directly from) people with disabilities, proving without a doubt that you could only go so far when “faking your way into a disability.”
