Something I have noticed in my studies of microservices that has always puzzled me when some folks describe microservices in articles and blogs, as well as in some books about microservices, is that you see very little mention of how much DDD plays a role or influences the creation of microservices and how it really serves, in my humble opinion, as a blueprint or the basis of microservices; it was around before microservices and has kind of gotten the cold shoulder as far as I am concerned.

The DDD and microservice concepts were introduced around the same time and share many of the same characteristics, such as single responsibility, a single code base, separated databases for each service, team alignment, and boundaries.

Eric Evans' book *Domain-Driven Design* came out in the 2003-2004 time frame, and soon after microservices in 2005, Peter Rodgers introduced the term *Micro-Web-Services* during a presentation at the *Web Services Edge* conference. As you study these concepts, you'll see many similarities that these two concepts share.

Let's now explore the similarities between microservices' characteristics and DDD and how they align. First up, we will look at autonomous microservices and DDD's bounded contexts. We will also look at other microservices and DDD characteristics and compare them.

# Autonomous

Microservices are small single-responsibility applications that are loosely coupled with one another. A single team of developers can build, test, deploy, and maintain a single small microservice. Each microservice owns a separate code base that a small team of developers can manage. This concept is described in *Domain-Driven Design* when the book addresses characteristics of a bounded context, which has its own code base and is aligned with a single team of developers.

# Independently deployable

Microservices can be deployed independently. This frees up a team to update a microservice without rebuilding and redeploying the entire application. DDD supports this concept as well, as each domain and supporting domains generally all have their own code base and adhere to the SOLID design's **single-responsibility principle (SRP)**. So, when something in one service changes, we should not have to deploy other services at the same time due to that change in another microservice.