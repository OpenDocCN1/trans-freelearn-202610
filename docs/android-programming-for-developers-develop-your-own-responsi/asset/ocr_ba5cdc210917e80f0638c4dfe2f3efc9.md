And a simple int variable to hold a question number is declared as:

```
int questionNumber;
```

And here, we declare our array of arrays called countriesAndCities. The outer array holds the arrays:

```
String[][] countriesAndCities;
```

Now, we allocate space within our arrays. The first outer array will now be able to hold 5 arrays and each of the inner arrays will be able to hold 2 Strings.

```
countriesAndCities = new String[5][2];
```

Now, we initialize our arrays so that they hold country names and their corresponding capital city names. Note that with each pair of initializations, the outer array number stays the same, indicating that each country/capital pair is within one inner array (a String array). And of course, each of these inner arrays is held in one element of the outer array (which holds arrays), as shown in the following code:

```
countriesAndCities [0][0] = "UnitedKingdom";
countriesAndCities [0][1] = "London";

countriesAndCities [1][0] = "USA";
countriesAndCities [1][1] = "Washington";

countriesAndCities [2][0] = "India";
countriesAndCities [2][1] = "New Delhi";

countriesAndCities [3][0] = "Brazil";
countriesAndCities [3][1] = "Brasilia";

countriesAndCities [4][0] = "Kenya";
countriesAndCities [4][1] = "Nairobi";
```

To make the upcoming for loop clearer, we declare and initialize int variables to represent the country and capital names from our arrays. If you glance back at the array initialization, all the country names are held in position 0 of the inner array and all the corresponding capital city names are held at position 1:

```
int country = 0;
int capital = 1;
```