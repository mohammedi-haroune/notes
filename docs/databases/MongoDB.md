# MongoDB Schema Design Best Practices
>Source: https://www.mongodb.com/developer/products/mongodb/mongodb-schema-design-best-practices/

- Designing a proper MongoDB schema is the most critical part of deploying a scalable, fast, and affordable database
- Don't think of it as a legacy relational design, document databases have a rich vocabulary that is capable of expressing data relationships in more nuanced ways than SQL.
- There are many things to consider when picking a schema. Is your app read or write heavy? What data is frequently accessed together? What are your performance considerations? How will your data set grow and scale?

## Schema Design Approaches – Relational vs. MongoDB
We normalize relational schemas using 3rd normal form which aims to split data in tables, so we don't duplicate it. We don't do that in mongo, there are no rules or format process to desing the database, we have to take into consideration three points though: storing the data, providing a good query performance and requiring a reasonalbe amount of hardware

Example:

![[mongodb-example.png]]

```json
{
    "first_name": "Paul",
    "surname": "Miller",
    "cell": "447557505611",
    "city": "London",
    "location": [45.123, 47.232],
    "profession": ["banking", "finance", "trader"],
    "cars": [
        {
            "model": "Bentley",
            "year": 1973
        },
        {
            "model": "Rolls Royce",
            "year": 1965
        }
    ]
}
```

## Embedding vs. Referencing
- Embedding allows getting all the data in a single query, referencing means we have to run at least two queries or following `$lookup` operator
- Embedding makes updates Atomic by default, however relying too much on that is anti-pattern (I don't know why !)
- Embedding could results in large documents and we may hit the 16mb limit sooner, also large documents means more overhead (poor query performance) if most fields aren't relevant
- Referecing reduces the amount of data duplication, we shouldn't avoid that if it results in a better schema though

## Rules
- **Rule 1**: Favor embedding unless there is a compelling reason not to.
- **Rule 2**: Needing to access an object on its own is a compelling reason not to embed it.
- **Rule 3**: Avoid joins/lookups if possible, but don't be afraid if they can provide a better schema design.
- **Rule 4**: Arrays should not grow without bound. If there are more than a couple of hundred documents on the "many" side, don't embed them; if there are more than a few thousand documents on the "many" side, don't use an array of ObjectID references. High-cardinality arrays are a compelling reason not to embed.
- **Rule 5**: As always, with MongoDB, how you model your data depends – entirely – on your particular application's data access patterns. You want to structure your data to match the ways that your application queries and updates it.

## Type of Relationships
-   **One-to-One** - Prefer key value pairs within the document
-   **One-to-Few** - Prefer embedding
-   **One-to-Many** - Prefer embedding
-   **One-to-Squillions** - Prefer Referencing
-   **Many-to-Many** - Prefer Referencing