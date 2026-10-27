# Main uses of keys

In the previous section, we saw that the PK of an entity is used to identify the instances of the entity and that FKs are used to put in relation the instances of the entities.

These are the most common operations where the keys are central:

+   - **Tracking and applying changes**: The PK is used to identify the instance that the incoming data belongs to so that we can compare if anything has changed compared to what we have already stored.

+   - **Extracting the current or historical version**: The PK is used to collect all the versions that are stored and extract the most recent one or the one that was active at some point in time.

+   - **Joining data**: Joining tables by equating the FK in a table with the PK of another table is the main operation that we do in the REF layer to put together the data that we need across the existing relations.

Creating relations between tables by putting the PK of another table as the FK in a table is the most powerful thing that relational DBs exist for. It is the most important ability for understanding and designing data models.

You should only join tables while following the direct FK-PK relationships or the associative tables that are used to implement many-to-many relations. In the latter case, you should expect to potentially have multiple rows returned by the join for each row of the starting table.

Beware of blindly joining two tables on a common foreign key as this is what brings you directly to the chasm trap data problem we described in Chapter 3.

+   - **Referential integrity**: When we have an FK in a table, we should ensure that the instance referenced by the FK exists in the parent table hosting the PK. If there is no PK to match an FK, we are missing some information. In such cases, we say that the FK is an orphan as there is no entry in the parent table.

Ensuring proper referential integrity is very important to guarantee that we can query our tables and retrieve all the expected information.

This is the common case of fact tables that store multiple FKs, each pointing to a parent table that is a dimension.

Now that we’ve learned about keys and what they are used for, let’s delve into how to use master data management to extend our ability to identify entities beyond their initial domain.

# Master Data management

When we refer to Master Data, we are talking about the descriptive data that is at the core of an organization and the processes to ensure that we can understand when different units are referring to the same instance of a concept.