```
print(movie)

    await client.close()
    except Exception as e:
        raise Exception("Unable to find the document due to the following error: ", e)

# Run the async function
asyncio.run(main())
```

Implementing asynchronous patterns within MongoDB client libraries enables you to build highly responsive and performant applications. By leveraging constructs such as Promises, async/await, or, as is the case with the preceding example, language-specific concurrency features, developers can ensure database operations do not block the application’s main thread.

Concurrent execution, coupled with non-blocking I/O, prevents applications from freezing or becoming unresponsive, ultimately leading to a smoother and more efficient user experience, especially under high load.

# Surfacing and handling failure conditions

To manage transient failures and network issues, you should implement robust error handling. MongoDB drivers provide mechanisms for retryable reads and writes, which can automatically retry operations in case of temporary failures:

+   - **Retryable writes:** MongoDB drivers support automatic retries for write operations that fail due to transient network issues or replica set elections. This feature is enabled by default but can be disabled using the retryWrites=false connection string option.
+   - **Retryable reads:** Similarly, read operations are configured to automatically retry on certain failures by default. You can disable this behavior using the retryReads=false option.

As robust as these automatic retries may be, they should be coupled with appropriate error-handling patterns in your application code to gracefully handle database exceptions, such as with the following Node.js code snippet, which illustrates how specific error conditions can be caught and logged when performing an insert operation:

```
try {
    const result = await collection.insertOne(document);
    console.log(`Document inserted with _id: ${result.insertedId}`);
} catch (err) {
```