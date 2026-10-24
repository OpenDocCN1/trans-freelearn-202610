# 12

# Managing States with NgRx 275

+   - Technical requirements 275
+   - Understanding complexities in managing the states of large applications 276
+     - The flow of data without state management 276
+     - The flow of data with state management 277
+   - State management and the global state 278
+     - Global state/store 278
+     - Actions 278
+     - Reducers 279
+     - Selectors 279
+     - Effects 280
+   - Getting started and setting up NgRx 283
+     - Adding NgRx to our Angular project 283
+     - Installing NgRx DevTools 284
+   - Writing an action 286
+     - Implementing actions in the project 286
+   - Writing an effect 287
+   - Writing a reducer 290
+     - The state interface 290
+     - The initial state 290
+     - The reducer function 291
+   - Writing a selector and using a selector and dispatching it in a component 292
+     - Selecting root states 292
+     - Selecting feature states 293
+     - Implementing selectors in our project 293
+   - Configuring the store 295
+     - Runtime checks 295
+   - Summary 300

# 13

# Saving, Deleting, and Updating with NgRx 301

+   - Technical requirements 301
+   - Removing an item without a side effect using NgRx 301
+     - Creating the delete action 302
+     - Creating the delete reducer 302
+     - Dispatching the action 303
+   - Removing an item with side effects using NgRx 305
+     - Creating a new action type 305
+     - Creating the delete effect 306
+     - Modifying the dispatch 306
+   - Adding an item with side effects using NgRx 307
+     - Creating the actions 307
+     - Creating the effect 308
+     - Creating the reducer 309
+     - Dispatching the action 310
+   - Updating an item with a side effect using NgRx 311
+     - Creating the actions 311
+     - Creating the effect 312
+     - Creating the reducer 313
+     - Dispatching the action 314
+   - Summary 317