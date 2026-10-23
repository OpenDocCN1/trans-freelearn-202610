CookBot is not just any AI. It's conceived as an advanced culinary assistant that not only suggests recipes based on the available ingredients but also understands the nuances of user queries, adapts to individual dietary preferences and restrictions, and generates insightful culinary recommendations.

Our objective was to infuse CookBot with RAG, ELSER, and RRF technologies. These technologies are designed to enhance the semantic understanding of queries, optimize information retrieval, and generate relevant, personalized responses. By harnessing the capabilities of these advanced tools, we aimed for CookBot to be able to provide seamless, context-aware culinary assistance tailored to each user's unique needs.

![](bbox=[0.33, 0.275, 0.62, 0.505])

Figure 9.3: CookBot powered by Elastic

# Dataset overview – an introduction to the Allrecipes.com dataset

The Allrecipes.com dataset, in its raw CSV format, is a treasure trove of diverse and detailed culinary information. Thus, it is the perfect foundation to train our CookBot. It houses an extensive range of recipes, each encapsulated in a unique entry brimming with an array of information.

You can find and download the dataset here, as it will be used later in the chapter:

https://www.kaggle.com/datasets/nguyentuongquang/all-recipes

To illustrate the richness of this dataset, let's consider a single entry:

```
"group","name","rating","n_rater","n_reviewer","summary","process","ingredient"
"breakfast-and-brunch.eggs.breakfast-burritos","Ham and Cheese Breakfast Tortillas",0,0,44,"This is great for a special brunch or even a quick and easy dinner. Other breakfast meats can be used, but the deli ham is the easiest since it is already fully cooked.","prep: 30 mins,total: 30 mins,Servings: 4,Yield: 4 servings","12 eggs + <U+2153> cup milk + 3 slices cooked ham, diced + 2 green onions, minced + salt and pepper to taste + 4 ounces Cheddar cheese, shredded + 4 (10 inch) flour tortillas + cup salsa"
```