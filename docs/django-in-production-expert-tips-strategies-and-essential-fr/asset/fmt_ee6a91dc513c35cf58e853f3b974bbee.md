
*   **Always use cache**: GitHub Actions support caching of steps; using cache can improve the execution time of long-running repetitive steps such as `pip install`. For more details, visit [https://github.com/actions/cache](https://github.com/actions/cache).

*   **Create semantic versions**: We have learned how to use semantic versioning for our project; it is recommended to use GitHub Actions to make releases. Similarly, we can create automated release notes for each of our releases.

*   **Verify GitHub Actions locally**: It becomes a challenge to verify GitHub Actions since each update in the YAML configuration will mean we have to run the pipeline again. Hence, a better way to verify the changes would be by using the project at [https://github.com/nektos/act](https://github.com/nektos/act).

*   **Upload artifact**: As the team matures, there will be actions that use the same build steps and artifacts, so rather than repeating the same build/creation process, it is recommended to use `actions/upload-artifact` to share artifacts between jobs. For more details, visit [https://github.com/actions/upload-artifact](https://github.com/actions/upload-artifact).

## Recommended GitHub Actions resources

The experience of using the CI pipeline in GitHub has improved ever since the introduction of GitHub Actions in 2018. Every week, developers create more open source projects for GitHub Actions and share them with different developers across the globe. Therefore, before building anything in GitHub Actions from scratch, I would recommend one go and search for the existing actions and see if they can solve your use case. I generally prefer to go to [https://github.com/sdras/awesome-actions](https://github.com/sdras/awesome-actions) and search for the relevant actions before custom building. However, one should also check if the action is maintained currently or not. I generally ask these questions before using any third-party action:

*   How many users have used the actions? We can find this information on the GitHub project repository page.

*   What are the active issues in the project? Is the community actively responding to GitHub Issues? Do the active issues have security vulnerabilities that the project maintainer is not addressing?

*   When was the last time the project saw any code update?

*   How many stars do the actions have?

The preceding questions can only be considered as an outline that can help the developer decide whether they should use the third-party GitHub Actions.

Now, let us check a few of the top actions that I use frequently, apart from the official GitHub Actions:

*   Send slack message whenever there is some action performed on the code repository using the pipeline, such as deployment ([https://github.com/marketplace/actions/slack-send](https://github.com/marketplace/actions/slack-send))

*   Perform linting using GitHub Actions ([https://github.com/marketplace/actions/super-linter](https://github.com/marketplace/actions/super-linter))

*   Check for any secret key accidentally committed in the code repository ([https://github.com/marketplace/actions/trufflehog-oss](https://github.com/marketplace/actions/trufflehog-oss))

Once our feature is tested and we have all the pipelines passing, the next task for us is to perform a code review. Code review is a process that is pivotal for any development team. In the next section, we shall discuss how we can incorporate a good code review process and structure for a development team.

# Setting up code review guidelines

Code review is the quality assurance process of any code change made to the code base. It is one of the loved and equally hated processes in the development cycle. The code review process, if implemented properly, can help create a great experience for the developers and fast track the product development cycle. But if the code review process is implemented appropriately, then it can become a bottleneck for the velocity of the company, and developers might become hostile toward doing code review, and a lot of times they may cause more issues. The code review process should be implemented by the early members of the team and should be continuously improved and adapted as the company/product/team grows. There are no definitive guidelines and it falls to team members to set up the guidelines that suit their situation. However, let us look into a few code review guidelines that are followed across the industry.

## Context and description

Whenever a code change is sent for review, making sure that there is a good context and description added to the PR helps produce a better and faster review. We can add the following:

*   **What?**: What does the code change intend to achieve? Explain the outcome we are trying to achieve via the code change. You should try to add all the context here and mention all the relevant non-technical documents (if applicable).

*   **How?**: How is the solution implemented in the code? Give a high-level overview of how the code is implemented. It is also helpful to attach any architecture doc or design doc in this section.

*   **Why?**: Why are we implementing the code changes (if applicable)?

Though in this book we are focusing primarily on backend changes, code review guidelines can be applicable across different coding teams. We should always try to attach all the relevant screenshots or videos linked to the code changes in the PR description.

## Short code changes to review

One of the most crucial recipes to create a good code review process is to make sure the code changes are short. Setting a guideline to have a maximum of 300-500 lines of code to be reviewed in a PR helps in creating a good process.

But then how would a feature that needs more than 1,000-2,000 lines of changes be reviewed? For such features, it is always better to have a main feature branch and let developers create a smaller feature branch and merge them into the main feature branch. By using this approach, the code reviewer will always have a smaller code change to review and once the code changes are merged to the main feature branch, it is easier to glance at all the changes made to the humongous branch and review them.

## Review when the code is ready

The code review should be done only when the PR is ready to be reviewed. We can use a few checklists that can help developers understand if their code is ready to be reviewed or not. Here are a few checklist items that I generally follow:

*   Remove all the debug statements or print statements or any commented statements (these can be taken care of by the linter itself).

*   Test the code changes in the development environment.

*   Make sure there are no merge conflicts.

*   Unit tests and integration tests are added to the code and all the tests are passing. Also, make sure that the code test coverage is as per the team standard.

*   PR description is self-explanatory.

*   Make sure the code follows the coding guideline standards.

Each team creates an extensive checklist that they can use within the team.

## Good code reviewer

Once the PR has ticked all the checkboxes, now the task of code reviewer begins. The most important thing for a code reviewer is to make sure you are empathetic toward the person asking for a review. Here are a few good qualities/pointers I have generally seen in good code reviewers:

*   **Be respectful**: The most important quality of a code reviewer is to be respectful while doing the code review. None of the comments should be personal or demeaning.

*   **Be verbose**: Code reviewers who are verbose and communicate the reasoning behind their comments are always preferred by coders.
