# Summary

With this case study, we have examined the various building blocks of Spring Python and utilized them to develop a simple, yet sophisticated banking site. While it may not have all the visual appeal, that task can easily be handed over to a web interface expert for improvement.

What is important is that key functional concepts have been implemented such as security and integrity. Customer data cannot be seen by other customers. It also has some simple banking protocols implemented such as overdraft prevention. Transactions are also implemented with little impact to the code.

The use cases involving managers and supervisors were not implemented with the intent that the reader could implement them as exercises.

With a functional banking application, the door is opened to further enhancements and implementation or more use cases needed to meet the customers' and the bank's needs.

In this chapter we learned how to:

+   - Wire together the components of our banking application using dependency injection
+   - Easily setup user authentication
+   - Restrict what pages a user can visit based on their role and whether or not they own the data being viewed
+   - Export raw data using Pyro to another trusted machine inside the enterprise
+   - Export raw data for the customer, to be used by some external tool
+   - Write an aspect that audits the database operations performed by the controller
+   - Easily mark up atomic transfer operations with the `@transactional` decorator

In the next chapter, we will take a look at Spring Python's command-line application, coily, which can help us build Spring Python applications quicker. We will also see how to write our own plugins to add functionality as we need.