```
```javascript
cy.fixture("anti-heroes").then(function (data) {
  cy.get("[data-cy=row]").should("have.length", 24);
});

it("should remove a card after clicking a delete button", () => { const index = 1;
  cy.get("[data-cy=delete]").eq(index).click();
  cy.get("[data-cy=row]").should("have.length", 20);
});

it("should add a new hero", () => { const firstName = "Bucky";
  const lastName = "Barnes";
  const house = "Marvel";
  const knownAs = "The Winter Soldier";
  cy.get("[data-cy=create]").click();
  cy.get("[data-cy=firstName]").type(firstName);
  cy.get("[data-cy=lastName]").type(lastName);
  cy.get("[data-cy=house]").type(house);
  cy.get("[data-cy=knownAs]").type(knownAs);
  cy.postCommand("/api/v1/anti-heroes", {
    firstName,lastName,house,knownAs,});
  cy.get("[data-cy=action]").click();
  cy.fixture("anti-heroes").then(function (data) {
    cy.get("[data-cy=row]").should("have.length", 24);
  });
});
```
```

The preceding code shows the tests we are going to use for Cypress. You can see that fixture needs a string, which is the name of the JSON file. The data from fixture is the parameter of the anonymous function.

We are asserting 24 because there are four elements for every object we use in data-cy="row", which is how we built the HTML elements on the user interface. There are also six objects inside the array of the anti-heroes.json file.

The newly added tests show how we can pick a particular object from a list or array of rendered UIs using the eq keyword and an index number.

The newly added tests also show how to write text into an input field by calling the click and type functions. Then, you can use the postCommand custom chainable command to intercept HTTP POST requests.