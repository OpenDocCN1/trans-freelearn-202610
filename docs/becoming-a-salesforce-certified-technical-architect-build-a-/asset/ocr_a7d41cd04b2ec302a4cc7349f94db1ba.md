Explore another question raised by the judges:

In your environment and release strategy, can you explain further who would be moving the metadata between the environments and how? Can you also explain how the conflicts will be detected and resolved, and by whom?

This is a question you have seeded during the presentation. You should be ready to answer it:

The developers would be working on a new feature branch for every developed feature. Once they are done, they can initiate a pull request to merge this branch with the CI/build branch.

The pull request will notify the release manager (who is sometimes known as the DevOps admin). The release manager will validate the pull request and detect any conflicts. SCMs provide mechanisms and tools for that. The release manager will attempt to resolve minor conflicts (such as changes in page layouts) and revert the pull request for more complicated conflicts. The developer will have to fix it and initiate another pull request.

Once the pull request is accepted, the automated build tool will attempt to deploy the code base in the SCM to the build environment. The release manager will deploy these functionalities to upper environments in due time based on predefined criteria (such as being accepted by the QA team).

Remember not to get carried away with too many details. Keep an eye on the ticking clock. That concludes the Q&A stage. You covered a different set of questions than what you came across in Chapter 11, Communicating and Socializing Your Solution, and Chapter 13, Present and Defend: First Mock. But, as you would expect, there is a vast number of possible questions. However, you have now learned how to handle them.

Understand the questions, accept being challenged, defend and adjust your solution if needed, and answer with enough details without losing valuable time. Enjoy being challenged by such senior and talented architects. Cherish it as this is a worthwhile experience that you may not encounter many times in your career.