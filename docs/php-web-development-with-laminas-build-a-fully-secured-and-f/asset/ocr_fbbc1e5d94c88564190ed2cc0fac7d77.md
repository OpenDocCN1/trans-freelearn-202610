+   • **A programming systems product**: Software that is nine times more complex and expensive than a single program whose logic fits in the head of a single programmer.

This software requires a big team to maintain it. It cannot fit in a single person's head. It is not easy to explain how it works or how it is organized. But, according to Brooks, it is the aim of most software development efforts.

> **Note**
> The Mythical Man-Month is a classic text for software engineering. Although it was written before the rise of agile software development, its analysis of the complexity of producing software and the difficulty in parallelizing tasks remains current.

Complex software under continuous change requires some characteristics to be maintained for a long time. We are talking about software made to last and not to throw away after a week. Some software is ephemeral, so you do not have to worry so much about its structure. Automatic code generation tools are great for generating code that will soon be discarded. But software that will evolve requires a deep understanding of how software works. In fact, software made to last requires some key characteristics: minimal complexity, loose coupling, extensibility, and reusability. Each one of these desirable characteristics is related to one critical characteristic: ease of maintenance.

It seems contradictory to say that a complex system must have minimal complexity. In fact, when we say that a programming systems product is complex, we are talking about a system that has many parts and that is big. Minimal complexity refers to the internal organization of these parts and how they are connected. It is not expected that every member of the development team knows everything about the entire system at all times, but it is desirable that any member of the team can easily understand a part of the system so that they can change it when it is necessary.

Erich Gamma, when he was interviewed by Bill Venners, stated that current software environments are too complex to create applications without the reuse of existing code. Gamma presumes that OOP is the standard paradigm of development because it offers the extensibility that is essential to reuse. Gamma states that there are three levels of reuse in OOP: reuse of classes, reuse of patterns, and frameworks. He emphasizes that frameworks are the highest level of reuse because they identify the key abstractions for solving problems and offer implementations that contain the knowledge and experience of several developers.

> **Note**
> You can read the complete interview with Erich Gamma at https://www.artima.com/articles/erich-gamma-on-flexibility-and-reuse.