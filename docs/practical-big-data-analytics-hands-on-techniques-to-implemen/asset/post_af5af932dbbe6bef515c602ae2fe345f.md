
Later in the paper, Turing writes:

> The original question, "Can machines think?" I believe to be too meaningless to deserve discussion. Nevertheless I believe that at the end of the century the use of words and general educated opinion will have altered so much that one will be able to speak of machines thinking without expecting to be contradicted. I believe further that no useful purpose is served by concealing these beliefs.

Turing's work on AI was followed by a series of seminal events in machine learning and AI. The first neural network was developed by Marvin Minsky in 1951, Arthur Samuel began his work on the first machine learning programs that played checkers in 1952, and Rosenblatt invented the perceptron, a fundamental unit of neural networks, in 1957. Pioneers such as Leo Breiman, Jerome Friedman, Vladimir Vapnik and Alexey Chervonenkis, Geoff Hinton, and Yann LeCun made significant contributions through the late 1990s to bring machine learning into the limelight. We are greatly indebted to their work and contributions, which have made machine learning stand out as a distinct area of research today.

In 1997, IBM's Deep Blue beat Kasparov and it immediately became a worldwide sensation. The ability of a machine to beat the world's top chess champion was no ordinary achievement. The event gave some much-needed credibility to machine learning as a formidable contender for the intelligent machines that Turing envisaged.

## Factors that led to the success of machine learning

Given machine learning, as a subject, has existed for many decades, it begs the question: why hadn't it become as popular as it is today much sooner? Indeed, the theories of complex machine learning algorithms such as neural networks were well known by the late 1990s, and the foundation had been established well before that in the theoretical realm.

There are a few factors that can be attributed to the success of machine learning:

- **The Internet**: The web played a critical role in democratizing information and connecting people in an unprecedented way. It made the exchange of information simple in a way that could not have been achieved through the pre-existing methods of print media communication. Not only did the web transform and revolutionize the dissemination of information, it also opened up new opportunities. Google's PageRank, as mentioned earlier, was one of the first large-scale and highly visible successes in the application of statistical models to develop a highly successful web enterprise.

- **Social media:** While the web provided a platform for communication, it lacked a level of flexibility akin to how people interacted with one another in the real world. There was a noticeable, but understated, and arguably unexplored gap. Tools such as IRC and Usenet were the precursors to social network websites such as Myspace, which was one of the first web-based platforms intended to create personal networks. By early-mid 2000, Facebook had emerged as the leader in social networking. These platforms provided a unique opportunity to leverage the Internet to collect data at an individual level. Each user left a trail of messages, ripe for collection and analysis using Natural Language Processing and other techniques.

- **Computing hardware:** Hardware used for computers developed at an exponential rate. Machine learning algorithms are inherently compute and resource intensive, that is, they require powerful CPUs, fast disks, and high memory depending on the size of data. The invention of new ways to store data on **solid state drives (SSDs)** was a leap from the erstwhile process of storing on spinning hard drives. Faster access meant that data could be delivered to the CPU at a much faster rate and reduce the I/O bottleneck that has traditionally been a weak area in computing. Faster CPUs meant it was possible to perform hundreds and thousands of iterations demanded by machine learning algorithms in a timely manner. Finally, the demand led to the reduction in prices for computing resources, allowing more people to be able to afford buying computing hardware that was prohibitively expensive. Algorithms existed, but the resources were finally able to execute them in a reasonable time and cost.

- **Programming languages and packages:** Communities such as R and Python developers seized the opportunity, and individuals started releasing packages that exposed their work to a broader community of programmers. In particular, packages that provided machine learning algorithms became immediately popular and inspired other practitioners to release their individual code repositories, making platforms such as R a truly global collaborative effort. Today there are over 10,000 packages in R, up from 2000 in 2010.

## Machine learning, statistics, and AI

Machine learning is a term that has various synonyms - names that are the result of either marketing activities by corporates or just terms that have been used interchangeably. Although some may argue that they have different implications, they all ultimately refer to machine learning as a subject that facilitates the prediction of future events using historical information.

The commonly heard terms for machine learning include predictive analysis, predictive analytics, predictive modeling, and many others. As such, unless the entity that publishes material explaining their interpretation of the term and more specifically, how it is different, it is generally safe to assume that they are referring to machine learning. This is often a source of confusion among those new to the subject, largely due to the misuse and overuse of technical verbiage.

Statistics, on the other hand, is a distinct subject area that has been well known for over 200 years. The word is derived from the new Latin, *statisticum collegium* (council of state, in English) and the Italian word *statista*, meaning statesman or politician. You can visit https://en.wikipedia.org/wiki/History_of_statistics#Etymology for more details on this topic. Machine learning implements various statistical models, which due to the rigor of computation involved, is distinct from the branch of classical statistics.

AI is also closely related to machine learning, but is a much broader subject. It can be loosely defined as systems (software/hardware) that, in the presence of uncertainties, can arrive at a concrete decision in (usually) a responsible and socially aware manner to attain a target end objective. In other words, AI aims to produce actions by systematically processing a situation that involves both known and unknown (latent) factors.

AI conjures up images of smart and sometimes rebellious robots in sci-fi movies, just as much as it reminds us of intelligent systems, such as IBM Watson, that can parse complex questions and process ambiguous statements to find concrete answers.

Machine learning shares some of the same traits - the step-wise development of a model using training data, and measuring accuracy using test data. However, AI has existed for many decades and has been a familiar household term. Institutions in the US, such as Carnegie Mellon University, have led the way in establishing key principles and guidelines of AI.

The online resources/articles on AI versus machine learning do not seem to provide any conclusive answers on how they differ. However, the syllabus of AI courses at universities makes the differences very obvious. You can learn more about AI at https://cs.brown.edu/courses/csci1410/lectures.html.

A simple explanation would help to illustrate this concept. By now, most of us are familiar with the concept of spam in emails. Most email accounts also contain a separate folder known as Junk, Spam, or a related term. A cursory check of the folders will usually indicate the presence of several emails, many of which were presumably unsolicited and contain meaningless information.

The mere task of categorizing emails as spam and moving them to a folder involves the application of machine learning. Andrew Ng highlighted this elegantly in his popular MOOC course on machine learning.

In Mitchell's terms, the spam classification process involves:

- **Task T**: Classifying emails as spam/not spam
- **Performance P**: Number of emails accurately identified as spam
- **Experience E**: The model is provided emails that have been marked as spam/not spam and uses that information to determine whether a new email is spam or not

Broadly speaking, there are two distinct types of machine learning:

- Supervised machine learning
- Unsupervised machine learning

We shall discuss them in turn here.

## Supervised and unsupervised machine learning

Let us start with supervised machine learning first.

## Supervised machine learning

**Supervised machine learning** refers to machine learning exercises that involve predicting outcomes with labelled data. Labelled data simply refers to the fact that the dataset we are using to make the predictions (as well as the outcome we will predict) has a definite value (irrespective of what it is). For instance, classifying emails as spam or not spam, predicting temperature, and identifying faces from images are all examples of supervised machine learning.
