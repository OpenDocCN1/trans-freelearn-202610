
1. First, let's code a simple DataAccess class that uses DatabaseTemplate to fetch the number of articles we have.

```
class DataAccess(object):
    def __init__(self, conn_factory):
        self.dt = DatabaseTemplate(conn_factory)

    def count_wiki_articles(self):
        return self.dt.query_for_object("SELECT COUNT(*) FROM ARTICLE")
```

This simple data access layer has one method: count_wiki_articles. It utilizes the code we wrote earlier involving the DatabaseTemplate. In this example, DataAccess expects to be initialized with a connection factory.

Now, to test this out, we need DatabaseTemplate to do its job, but we want to catch it at the right point in order to inject some pre-built values. The piece of code that does the heavy lifting is Spring Python's cursor object, which is supplied by a connection. This means we need to code a special stubbed out connection factory that will hold a mocked cursor.

2. Let's write a specialized class to act as a mocked connection.

```
class MockedConnection(object):
    def __init__(self):
        self.mockCursor = None
    def cursor(self):
        return self.mockCursor
```

This connection will supply DatabaseTemplate with a special type of mocked cursor. Further down, we will see how mockCursor gets populated.

3. In order to use this, let's code a connection factory for DatabaseTemplate that produces this type of connection.

```
class MockingDBFactory(ConnectionFactory):
    def __init__(self):
        ConnectionFactory.__init__(self, [types.TupleType])
        self.mockedConnection = MockedConnection()
    def connect(self):
        return self.mockedConnection
```

When this connection factory is asked to connect to the database, it will return a MockedConnection.

4. To tie this rigging together, we need to setup a MockTestCase. This is a special type of unit test that provides extra hooks to library calls of pmock.

```
from pmock import *

class DataAccessMockTestCase(MockTestCase):
    def setUp(self):
        # Create a mock instance to record events
        self.mock = self.mock()
        conn_factory = MockingDBFactory()
        conn_factory.mockedConnection.mockCursor = self.mock
        self.data_access = DataAccess(conn_factory)
```

Here in the setUp method for our test, we grab an instance of mock(). This object has APIs to record expectations. The object is meant to be injected so that function calls are then made against it, and at the end of a MockTestCase test method, the results are compared with the expectations.

In this situation, the mockCursor is the key holder of the mock. There is also a local copy, so that the MockTestCase has a handle to check out the results.

5. After setting all this up, let's define a mocked test method.

```
def testCountingArticles(self):
    self.mock.expects(once()).method("execute")
    self.mock.expects(once()).method("fetchall")
        .will(return_value([[(2,)]]))

    count = self.data_access.count_wiki_articles()
    self.assertEquals(count, 2)
```

The first two steps of this test use the mock object to define expectations. The mock is expected to receive an execute method call once, and also a fetchall method call. The fetchall will return a value of [ (2,) ].

Not only do we get to check the assertions, but pmock will verify that each of these methods was invoked by DatabaseTemplate.

## How much testing is enough?

For this test scenario, we dug down deep. This type of testing can't get much closer to the hardware of the database server without directly talking to a live database.

It could be viewed that we were really testing the core of DatabaseTemplate. In many development situations, this isn't needed. Testing a 3rd party library would probably be out of scope.

This example test scenario is largely based on automated tests used to confirm Spring Python's functionality. Adequate testing for your business needs may involve stubbing or mocking out the data access layer as was shown in earlier parts of this book.

This should introduce you to the concept of mocking, where you measure method calls and answers. This type of testing may perfectly fit other test needs. If the SQL queries have been well isolated in this layer, then it may be safe to say that the only testing needed would be the queries themselves against a live database and in turn, not require any mocking or stubbing at all.

## Summary

This chapter has shown a way to write pure SQL without having to deal with the low level cursor and connection management. This removes a lot of boiler plate and error handling that is perfect for a framework to handle.

We have also explored how DatabaseTemplate and ORMs can work together to make persistence management easier for developers.

We took a look at mocking, and how we were able to get inside the querying process to verify that the right method calls were being made. Then we stepped back and considered how much testing is enough.

In this chapter we learned:

- The classic SQL query issue that affects APIs in many modern programming languages extends into Python as well
- Using Spring Python's DatabaseTemplate lets us get rid of query boiler plate code that gets in the way of solving use cases
- DatabaseTemplate is useful for writing SQL code without dealing with cursor management
- DatabaseTemplate combined with an ORM can help us build robust applications
- Mocking is a valuable tool in automated testing, but we must choose the right tool for the right situation

In the next chapter, we will look at augmenting our data access layer with non-invasive transactions.

## 5

## Adding Integrity to your Data Access with Transactions

SQL operations are used in lots of applications in order to supply data. A key ingredient used to grow applications to enterprise scale are transactions. They add integrity to data management by defining an atomic unit of work.

An atomic unit of work means that the whole sequence of steps when completed must appear like a single step. If there is any failure in the chain of steps, everything must rollback to the state before the transaction started. For SQL transactions, this means that the state of the database must update or rollback atomically.

This chapter will inspect the pattern of coding and using SQL transactions, and how Spring Python makes it easy to code an otherwise monotonous pattern.

In this chapter, you will learn:

- The classic pattern of SQL transactions and the issues imposed when coding transactions by hand
- That it is easy to add transactions to a banking application using Spring Python's `@transactional` decorator
- You can choose between coding transactions programmatically or by decorator
- You can choose between using and not using the IoC container
- Spring Python provides the means to non-intrusively mix in transactions to non-transactional code without editing the source

## Classic transaction issues

Transactions have a simple pattern of execution as shown in this block of pseudo-Python.

```
# Start transaction
try:
    #*********************************
    # Execute business logic that
    # contains database operations.
    #*********************************

    # Commit transaction
except:
    # Rollback transaction
```

Let's look at the steps involved in defining a transaction:

- 1. First, the transaction must be *started*.
- 2. Then, a block of business code is executed, which contains several database operations, including `select`, `update`, and `insert`.
- 3. Finally, the transaction is *committed*. If any type of error occurs, an exception handler *rolls back* the transaction, undoing all the changes that were made.

The problem with writing transactions manually is similar to the problem with writing SQL operations manually. There is a lot of boilerplate code that must be written. The boilerplate has a tangling effect because it must be written before and after the business code in order for the transaction to be handled correctly. This pattern must then be copied into every location where some business logic needs to be wrapped in a transaction.

This violates the **DRY (Don't Repeat Yourself)** principle. It also tangles our code with not only business logic, but transactional logic as well. And it breaks the **Single Responsibility Principle (SRP)** by having our code depend on both business and integrity requirements.

Violating these principles puts us at risk. We might introduce bugs when changes to the transactional logic are needed, but we fail to repeat them in all the right places.

This is a crosscutting problem because it extends beyond our class hierarchy. Steps contained within the unit of work can quickly cut across several classes. This is a perfect use case for aspect oriented programming that was discussed earlier in this book. As we delve into this in more detail in this chapter, we will see how to solve it with ease using Spring Python.
