```
Scenario: Inserting a product in the
registration            # ProductAddition.
feature:11
    Given there is a product called "Troop
    Power Battery", which costs $2814 # ProductAdditionContext::thereIsAProductCalledWhichCosts()
    TODO: write pending definition
    When I add this product to the
    registration    # ProductAdditionContext::iAddThisProductToTheRegistration()
    Then I should have a product called "Troop Power Battery" in the table products # ProductAdditionContext::iShouldHaveAProductCalledInTheTable Products()
    And the overall product price should be $2814 # ProductAdditionContext::theOverallProductPriceShouldBe()
1 scenario (1 pending)
4 steps (1 pending, 3 skipped)
0m0.01s (9.31Mb)
```

Now, the phrases of the scenario have, as comments, the name of the correspondent methods in the context class. In the end, we can see that scenario is not undefined anymore but pending. The steps have changed to 1 pending and 3 skipped. The state pending means that the context class has methods to implement the steps of the scenario; however, these methods have no content yet. It’s like a PHPUnit test case failing because there is no code to be tested yet.

We have to complete the scenario implementation. For this, we need to implement the models and object-relational mappers related to the scenario. We will start this in the next section.

# Creating models from user stories

In a grammar class, you may have heard the question, “In this phrase, who or what is the subject?” In a similar way, we ask the following: “In an Inserting a product in the registration scenario, what is the model?” Yes, exactly – it is the product! Then, we need to instantiate a Product class in our behavior test class. For this, we must define a $product attribute in the ProductAdditionContext class:

```
private ?Product $product = null;
```

We can instantiate the Product class in the constructor of the ProductAdditionContext class and use this instance in the thereIsAproductCalledWhichCosts method, as follows: