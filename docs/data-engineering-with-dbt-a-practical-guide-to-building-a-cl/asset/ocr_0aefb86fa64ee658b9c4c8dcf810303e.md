At this stage, you must test that PK/BK at the output of your STG model is unique and that, in the HIST model, the combination of PK/BK and LOAD_TS is also unique, as we want to store one change at each run.

+   - Test that HKEY and HDIFF have no hash collision (in the HIST model).

This is low-priority. The collision risk is tiny, especially if you do not have billions of rows.

The collision risk increases the more hashes you have in a table; therefore, it makes sense to check the hashes in the HIST model, as it contains all the hashes for one entity. The risk is always very small, and one collision would eventually compromise only one row of data, but it would be irresponsible to not test for it.

When tables have billions of rows, the test starts to be more useful but also slower, so it might make sense to run the hash collision test with a lower frequency.

+   - Create all the tests that you would have done in the source layer but that were impractical because they needed some data transformation.

It is quite common that some input data needs cleanup or transformation before working as expected. Common examples are dates and timestamps that are parsed from text (you want to be sure that you can parse them from their textual format, before being able to test what is expected by them), or text fields normalized by removing extra spaces or meaningless zeros, or changed to uppercase or lowercase.

# Tests on the refined layer

This is the business rules layer that depends on stored data being correct.

In this layer, we should assume that the data that comes from the storage layer is sound, and we must test only where we can introduce errors by doing transformations:

+   - Test the results of the BRs that calculate values. This is of high priority. We must be sure that we have a result and that it’s correct.

The minimum expectation is that you check that the calculated values produce a result and not null values (unless the value is optional), and if control data is available, test that the result is correct. The correctness of the BR is often tested using singular tests, with reference to well-known test cases.

+   - Test the PK/FK after risky or complex joins (to implement the BRs).

This is a top priority. Make sure that the data conforms to the data model of the BR.

Another big category of BRs is enrichments, where you join tables to bring in measures or descriptive values. We deal with having the value and its use in a calculation in the previous bullet point.