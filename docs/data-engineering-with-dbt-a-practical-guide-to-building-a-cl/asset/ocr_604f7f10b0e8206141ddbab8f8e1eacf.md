+   - When we get to BRs, it is also very important to test that we have all the pieces we need to calculate them and, if possible, to test that our calculations are correct. This last part requires support from the business users to provide usable test cases with the expected results.

+   - Getting into lower priority data categories such as measures, categories, or descriptive data, the importance should be considered case by case.

The person responsible for sales, the sales team, or the customer’s name can be of secondary importance in some contexts, while it might be crucial in others.

You need to consider adding tests for these data categories while balancing the pros and cons.

Now that we have a clear picture of what to test, let’s move on to analyze where it is best to perform our tests.

# Where to test what?

The general idea of where to do tests looks simple; the earlier we catch a problem, the better it is, so we should do a test as soon as a test is doable.

You also want to test what you are delivering, to be sure that despite all the intermediate transformations you have done, the result conforms to your expected data model.

The golden rules are as follows:

+   - Test your expectations about inputs on sources or as soon as possible

+   - Test your expectations about outputs when you deliver them

+   - In the intermediate transformations of the ELT pipelines, test the important things that might create a lot of problems or that you do not totally trust to always work as planned, such as BRs or complex joins

In the following sections, let’s discuss the typical tests for every layer of our data platform.

# Tests on the sources (input layer)

This is the entry point of the data, which you have not touched yet, so you can only test assumptions that do not need any change on incoming data.

Here, you have some of the most important tests:

+   - Test PKs and BKs to be not null and unique. This is a top priority that ensures that you correctly understand and store your entities.

If you have a source with a full history or even a partial history with multiple versions, you must move the unicity test to another layer, usually the STG layer (where you can filter to keep the latest version to be stored).