
## One-to-One Relationships

Each person can have at most one note, and each note belongs to exactly one person. The relationship is shown as `1---1`, meaning there is no duplication on either side.

![](Images/B34553_07_02.png)

Figure 7.2: One-to-one relationship

Looking at the data:

*   **John Doe** (id = 1) is linked to a single note: “Client since 2021” (note id = 1)
*   **Jane Doe** (id = 2) has no note in this example
*   **Mika Jones** (id = 3) is linked to: “Met at conference” (note id = 3)

Even though not every person has a note, the rule still holds. No person has more than one note, and no note is shared between people.

In a real schema, this is usually enforced by placing a unique foreign key in the `notes` table, for example:

```
person_id INTEGER UNIQUE
```

That `UNIQUE` constraint is what turns a one-to-many structure into a true one-to-one relationship. The foreign key still lives on one side (for example, the `notes` table), but the uniqueness constraint prevents duplicates, so the relationship cannot expand into many.

## Many-to-Many Relationships

Sometimes both sides can have many related rows. A film can have many actors, and an actor can appear in many films. This cannot be represented with a single foreign key.

Instead, you create a **junction table**, sometimes called a link table or join table. This table contains at least two foreign keys, one pointing to each side of the relationship. Each row represents one relationship instance.

For example, a `credits` table might contain `film_id` and `person_id` as foreign keys, plus extra columns such as `character_name` or `role`.

Figure 7.3 shows a many-to-many relationship between `people` and `addresses`, implemented using an intermediate table called `people_addresses`.

At a conceptual level:

*   A person can have multiple addresses (home, work, and so on)
*   An address can belong to multiple people (a shared household or workplace)

That means neither table can hold a simple foreign key without causing duplication or breaking the model. Instead, the relationship is handled through the `people_addresses` table.

![](Images/B34553_07_03.png)

Figure 7.3: Many-to-many relationship

How the data connects:

*   `people.id` identifies each person
*   `addresses.id` identifies each address
*   `people_addresses` links them using pairs of IDs

Looking at the sample rows:

*   **John Doe** (id = 1) resides at address 1: 742 Evergreen Terrace
*   **Jane Doe** (id = 2) is linked to address 1 and address 2 so she shares the home address with John and works at address 2 (100 Main Street)
*   **Mika Jones** (id = 3) works at address 2 with Jane

A many-to-many relationship cannot be represented directly with a single foreign key. To support many on both sides, you need a separate table that:

*   Stores combinations of related rows
*   Allows multiple entries per person and per address
*   Keeps the schema normalized and avoids duplication

The intermediate table turns one complex many-to-many relationship into two simple one-to-many relationships:

*   `people` is related to `people_addresses`
*   `addresses` is related to `people_addresses`

This is the standard and correct way to model many-to-many relationships in relational databases.

## Referential Integrity and Cascading Rules

Foreign keys are usually defined with rules that say what happens when the referenced row changes. Common options include:

*   `RESTRICT` or `NO ACTION`: prevent deletion if related rows exist
*   `CASCADE`: automatically delete or update related rows
*   `SET NULL`: keep the row but clear the foreign key

Choosing these rules is a design decision. Cascading deletes can be convenient, but they can also remove large amounts of data very quickly if used carelessly.

## NULL Values

`NULL` is one of the most misunderstood concepts in SQL, and many subtle bugs come from treating it like an ordinary value. In SQL, `NULL` does not mean zero, an empty string, or false. It means *unknown* or *not applicable*.

A `NULL` does not represent data that exists but happens to be blank. It represents the absence of a value. This distinction matters because SQL must account for uncertainty when comparing or filtering data.

Comparisons involving `NULL` do not behave the way most programmers expect. For example, the following expression is not `true`:

```
universe = NULL
```

But it is also not `false`! It is *unknown*. This is why SQL has special syntax for checking for `NULL`:

```
universe IS NULL
universe IS NOT NULL
```

Any time you want to test for missing data, you must use `IS NULL` or `IS NOT NULL`.

### Three-Valued Logic

SQL uses three-valued logic, not two-valued logic. A condition can evaluate to: `TRUE`, `FALSE`, or `UNKNOWN`. Any comparison involving `NULL` produces `UNKNOWN`, not `TRUE` or `FALSE`. For example:

```
NULL = 'Star Trek'   evaluates to UNKNOWN
NULL <> 'Star Trek'  evaluates to UNKNOWN
NULL = NULL          evaluates to UNKNOWN
```

This has a practical consequence: the `WHERE` clause keeps only rows where the condition is `TRUE`. Rows where the condition is `FALSE` or `UNKNOWN` are excluded. This explains why queries sometimes lose rows unexpectedly when `NULL` values are involved.

`NULL` values can also affect joins in subtle ways. Join conditions that rely on equality will not match rows where the join key is `NULL`, because `NULL` never equals anything, including another `NULL`.

This is one reason why foreign key columns are often defined as `NOT NULL` unless there is a strong modeling reason to allow `NULL`.

Most `NULL` bugs come from forgetting that SQL has three truth states, not two, and that `UNKNOWN` is treated as *exclude this row* in filters.

**Prompt**: Please show me some common bugs caused by misunderstanding `NULL` and how to avoid them.

## Constraints

Constraints are rules enforced by the database to protect the integrity of your data. They act as a safety net that prevents invalid, inconsistent, or nonsensical values from ever being stored. Instead of relying on application code to *do the right thing*, constraints make the database itself responsible for guarding its own correctness.

Common constraints are shown in Table 7.1:

| Constraint | Description |
| --- | --- |
| `NOT NULL` | Ensures that a column must always have a value. It prevents rows from being inserted or updated with missing data in fields that are required for the record to make sense. For example, a title name, an email address, or a release year usually cannot be `NULL`. Allowing `NULL` values in these columns forces every query and every application layer to constantly check for missing values. If a value is genuinely optional, allow `NULL`. If it is not, say so explicitly in the schema. |
| `UNIQUE` | Ensures that no two rows share the same value in a column, or in a combination of columns. It enforces logical uniqueness rather than identity. This is different from a primary key, which enforces row identity. For example, you might allow only one user account per email address, or only one episode number per season. In those cases, the values must be unique within a defined scope. They are especially important in multi-user systems, where two requests can arrive at nearly the same time. |
