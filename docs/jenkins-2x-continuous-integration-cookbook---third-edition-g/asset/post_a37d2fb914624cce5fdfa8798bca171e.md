
## Avoiding human bottlenecks

The simpler your testing environment is, the less skill you'll need to maintain it. As you learn to use the plugins and explore the potential of new tools and scripting languages, the more knowledge the organization needs in order to maintain a stable system. If you wish to go on holidays without random text messages asking for advice, make sure that your knowledge is transferred to at least a second person. This sounds obvious, but in the rush of your daily load, this principle is often forgotten or put to one side.

One of the easiest ways to share knowledge is to send a couple of developers off to the same conferences and events together (https://www.CloudBees.com/company/events/juc). Learn best practices from Jenkins user conferences and other conferences too.

This is where managers play a significant role in knowledge dissemination. They need to plan in time and activities for the sharing of knowledge, rather than expecting it to happen by magic.

## Avoiding groupthink

It is easy to be perfect on paper, defining the importance of a solid set of Javadocs and unit tests. However, the real world on its best days is chaotic. Project momentum, motivated by the need to deliver, is an elusive force to push back against.

Related to project momentum is the potential of groupthink (http://en.wikipedia.org/wiki/Groupthink) by the project team or resource owners. If the team has the wrong collective attitude, as a quality assurance professional it is much harder to inject hard-learned realism. Quality assurance is not only about finding and capturing defects as early as possible, it is also about injecting objective criteria for success or failure into the different phases of a project's cycle.

Consider adding measurable criteria into the Jenkins build. Obviously, if the code fails to compile, then the product should not go to acceptance and production. Less obvious, are the rules around code coverage of unit tests worth defending in release management meetings.

Try getting the whole team involved at the start of the project before any coding has taken place and agree on metrics that fail a build. One approach is to compare a small successful project to a small failed project. If later there is a disagreement, then the debate is about process and numbers rather than personality.

## Visibly rewarding successful developers

This is a call to resource managers.

Developers and testers specialize in technical matters that are at times hard to explain to the ones outside their problem domain. To reach the highest level of expertise and to keep track of trends requires time (sometimes a lot of their own time), energy, and motivation. Undermining their motivation or underestimating the time required to build their skills will ultimately decrease the quality of your products and will cost more in the end.

Consider what you can do to support them, from pay scale jumps, learning paths, reserving time in the week for developers to read and practice new ideas, to conferences and gadgets. For example, after a pay rise, Kickstarter (https://www.kickstarter.com/) is a great place to look for motivational rewards and to stimulate the developers' creative muscle.

Often, the best thing to do is to trust developers and testers and give themselves proper direction to excel to achieve greater heights, and only then they will be able to create a platform to self-motivate themselves.

Train them, challenge them, reward them, and motivate them; get innovations for free. You need to remember that every small appreciation motivates team members and at times you may have healthy competition in the team as well.

Finally, do not make developers do non-development stuff. In general, they need to be highly focused on the complex task of understanding detailed requirements and turning them into rock-solid code.

## Stability and code maintenance

This book mentions many plugins and a number of languages and testing tools. It is OK to experiment in development and then push to acceptance, but the more diversity you have in production, the more skills are needed to maintain and especially to write a fluent workflow. Subtle choices, such as pinning Jenkins plugins at known versions and keeping the production version of your Jenkins server stable for fixed periods, help with up-time. Just as importantly, monitoring the load and offsetting most of the jobs away from the master Jenkins ensures a high degree of determinism in the timing of the jobs.

To limit job maintenance implies keeping configuration simple and similar. This is not realistic in a complex organization with a high degree of diversity. Using a test-driven approach helps; conventions also simplify configuration. As the diversity increases, communicating and agreeing to the conventions becomes important. Simple strategies such as one source of documentation wisdom (for example, a communal wiki), regular lessons, learned meetings, and weekly reviews become vital.

## Resources on quality assurance

It is a mistake to consider testing to be the sole responsibility of the testers. Coders should feel responsible for the quality of their code, architects for the quality of their designs, managers for the ethos of the project and project planning, and so on. Here are some examples of a range of practical resources on actionable quality assurance-this is not just for the testers.

There are many wise words on avoiding classic mistakes based on years of hard knocks and bruising. A well thought-out set of comments can be found at http://www.exampler.com/testing-com/writings/classic/mistakes.html.

If unit tests cover your code thoroughly, then if you break a piece of code during an update, you will know this quickly during the next build. JUnit is arguably the most well-known framework in this genre. You can find the framework's home page at http://junit.org/.

The Jenkins home page (http://jenkins-ci.org/) covers a wealth of information around the practicalities of configuration, plugins, the community, and hints and tips.

The **Open Web Application Security Project (OWASP)** is a great source of information and tools on security testing. OWASP is focused on improving the security of software. Its mission is to make software security visible so that individuals and organizations worldwide can make informed decisions about true software security risks. You can find the OWASP home page at https://www.owasp.org/index.php/Main_Page.

One popular example of a commercial company selling Jenkins infrastructure in the cloud is Sauce Labs (https://docs.saucelabs.com/ci-integrations/jenkins/).

There are a number of excellent and free-to-download software testing magazines. The professional tester is one such example and is available at http://www.professionaltester.com/.

uTest is the world's largest open community dedicated to professional testers and software testing. Its sole purpose is to promote and advance the testing profession, and the people who do this vital work. For more information, visit http://www.utest.com/about-us.

There are more and more free MOOC courses and a number of them support the learning paths of software testers. You can find a full list of currently running MOOC courses at https://www.mooc-list.com/.

## And there's always more

There are always more points to consider. Here are a few of the cherry-picked ones:

- **Blurring the team boundary**: Tools such as FitNesse and Selenium IDE make it easier for non-Java programmers to write tests. The easier it is to write tests, the more likely it is that the relevant tests capture the quintessential details of user expectations. Look for new Jenkins plugins that support tools, which lower the learning curve.
- **Deliberately adding defects**: By rotating through Jenkins builds and then deliberately adding code that fails, you can test the alertness and response time of the team.
- **Increasing code coverage with link crawlers and security scanners**: A fuzzer discovers the inputs of the application it is attacking and then fires off unexpected input. Not only is this good for security testing, but also for boundary testing. If your server returns an unexpected error, then use a fuzzer to trigger a more thorough review. Fuzzers and link crawlers are a cheap way to increase the code coverage of your tests.

In your development environment, periodically review for new Jenkins plugins. The number of plugins is increasing rapidly and there may be new ways for Jenkins to connect different parts of your organization's infrastructure to Continuous Integration.
