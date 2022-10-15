# GraphQL
>Source: https://graphql.org/

- A query language and a runtime to fulfill those queries
- Ask for exactly what you want
- Makes it easier to evolve APIs over time. Because clients asks for exactly what they want, we can add fields without affecting them
- Get many resources in a single request compared to hitting multiple URLs in RESTful APIs
- Describe what you want with a type system instead of endpoints in RESTful APIs
- Powerful dev tools, know what data you can request, highlight potential issues before sending requests, this is most probably because APIs are defined as types
- GraphQL APIs leverage existing data without enforcing any specific storage engine

Example:
```graphql
type Query {
  hero: Character
}

type Character {
  name: String
  friends: [Character]
  homeWorld: Planet
  species: Species
}

type Planet {
  name: String
  climate: String
}

type Species {
  name: String
  lifespan: Int
  origin: Planet
}
```

```graphql
{
  hero {
    name
    friends {
      name
      homeWorld {
        name
        climate
      }
      species {
        name
        lifespan
        origin {
          name
        }
      }
    }
  }
}
```

# Best practice
>Source: https://graphql.org/learn/best-practices/

- Serve everything on a single HTTP route
- JSON with Gzip
- No need for versionning
- Everything is nullable by default
- Server-side Batching & Caching using a tool like Facebook's [DataLoader](https://github.com/facebook/dataloader).
- Pagination, read more about this in the article on [Pagination](https://graphql.org/learn/pagination/).

## Caching
>Source: https://graphql.org/learn/caching/

Since GraphQL is exposed at the same HTTP URL, clients can't use HTTP caching to easily avoid refetching resources, and for identifying when two resources are the same.

A standard solution for this is to identify resources with a Globally Unique ID across all types but this could be problematic if we're migrating existing APIs to GraphQL, this could be solved by exposing the previous APIs ID in a separate field, if not the client can also derive the globally unique identifier. Oftentimes, this would be as simple as combining the type of the object (queried with `__typename`) with some type-unique identifier.

## Thinking in graphs
>Source: https://graphql.org/learn/thinking-in-graphs/

- [ ] Get back to the page and document it

# Queries and Mutations
>Source: https://graphql.org/learn/queries/

### Fields
GraphQL is all about asking for specific fields on objects
```graphql
{
  hero {
    name
    # Queries can have comments!
    friends {
      name
    }
  }
}
```

```json
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "friends": [
        {
          "name": "Luke Skywalker"
        },
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}
```
### Arguments
Arguments can be of many different types, servers can also define custom types. [Read more about the GraphQL type system here.](https://graphql.org/learn/schema)

```graphql
{
  human(id: "1000") {
    name
    height(unit: FOOT)
  }
}
```

```json
{
  "data": {
    "human": {
      "name": "Luke Skywalker",
      "height": 5.6430448
    }
  }
}
```
### Aliases
```graphql
{
  empireHero: hero(episode: EMPIRE) {
    name
  }
  jediHero: hero(episode: JEDI) {
    name
  }
}
```

```json
  "data": {
    "empireHero": {
      "name": "Luke Skywalker"
    },
    "jediHero": {
      "name": "R2-D2"
    }
  }
}
```

### Fragments
```graphql
{
  leftComparison: hero(episode: EMPIRE) {
    ...comparisonFields
  }
  rightComparison: hero(episode: JEDI) {
    ...comparisonFields
  }
}

fragment comparisonFields on Character {
  name
  appearsIn
  friends {
    name
  }
}
```

```json
{
  "data": {
    "leftComparison": {
      "name": "Luke Skywalker",
      "appearsIn": [
        "NEWHOPE",
        "EMPIRE",
        "JEDI"
      ],
      "friends": [
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        },
        {
          "name": "C-3PO"
        },
        {
          "name": "R2-D2"
        }
      ]
    },
    "rightComparison": {
      "name": "R2-D2",
      "appearsIn": [
        "NEWHOPE",
        "EMPIRE",
        "JEDI"
      ],
      "friends": [
        {
          "name": "Luke Skywalker"
        },
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}
```

##### Using variables inside fragments
```graphql
query HeroComparison($first: Int = 3) {
  leftComparison: hero(episode: EMPIRE) {
    ...comparisonFields
  }
  rightComparison: hero(episode: JEDI) {
    ...comparisonFields
  }
}

fragment comparisonFields on Character {
  name
  friendsConnection(first: $first) {
    totalCount
    edges {
      node {
        name
      }
    }
  }
}
```

```json
{
  "data": {
    "leftComparison": {
      "name": "Luke Skywalker",
      "friendsConnection": {
        "totalCount": 4,
        "edges": [
          {
            "node": {
              "name": "Han Solo"
            }
          },
          {
            "node": {
              "name": "Leia Organa"
            }
          },
          {
            "node": {
              "name": "C-3PO"
            }
          }
        ]
      }
    },
    "rightComparison": {
      "name": "R2-D2",
      "friendsConnection": {
        "totalCount": 3,
        "edges": [
          {
            "node": {
              "name": "Luke Skywalker"
            }
          },
          {
            "node": {
              "name": "Han Solo"
            }
          },
          {
            "node": {
              "name": "Leia Organa"
            }
          }
        ]
      }
    }
  }
}
```

### Operation name
```graph
operation_type operation_name(variable_definition) { operation_content }
```
The _operation type_ is either _query_, _mutation_, or _subscription_ and describes what type of operation you're intending to do. The operation type is required unless you're using the query shorthand syntax, in which case you can't supply a name or variable definitions for your operation.

### Variables
Variables avoids clients to dynamically manipulate the query string at runtime and serialize it into a GraphQL format.

When we start working with variables, we need to do three things:
1.  Replace the static value in the query with `$variableName`
2.  Declare `$variableName: variableType = [defaultValue]` as one of the variables accepted by the query
3.  Pass `variableName: value` in the separate, transport-specific (usually JSON) variables dictionary

```graphql
query HeroNameAndFriends($episode: Episode) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
```

```json
# variables
{
  "episode": "JEDI"
}
```

```json
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "friends": [
        {
          "name": "Luke Skywalker"
        },
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}
```

### Directives
```graphql
query Hero($episode: Episode, $withFriends: Boolean!) {
  hero(episode: $episode) {
    name
    friends @include(if: $withFriends) {
      name
    }
  }
}
```

```json
# variables
{
  "episode": "JEDI",
  "withFriends": true
}
```

```json
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "friends": [
        {
          "name": "Luke Skywalker"
        },
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}
```

The core GraphQL specification includes exactly two directives, which must be supported by any spec-compliant GraphQL server implementation:
-   `@include(if: Boolean)` Only include this field in the result if the argument is `true`.
-   `@skip(if: Boolean)` Skip this field if the argument is `true`.

Directives can be useful to get out of situations where you otherwise would need to do string manipulation to add and remove fields in your query.

Servers are free to add experimental features for custom directives

### Mutations
an operation type that mean to modify server-side data, query operations shouldn't be used to do that, think GET vs POST in REST APIs.

we can mutate and query the new value of the field with one request.

```graphql
mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) {
  createReview(episode: $ep, review: $review) {
    stars
    commentary
  }
}
```

```json
# variables
{
  "ep": "JEDI",
  "review": {
    "stars": 5,
    "commentary": "This is a great movie!"
  }
}
```

```json
{
  "data": {
    "createReview": {
      "stars": 5,
      "commentary": "This is a great movie!"
    }
  }
}
```

#### Multiple fields in mutations
While query fields are executed in parallel, mutation fields run in series, one after the other.

This means that if we send two `incrementCredits` mutations in one request, the first is guaranteed to finish before the second begins, ensuring that we don't end up with a race condition with ourselves.

### Inline Fragments
Inline Fragments are useful when dealing with union types. [Learn about them in the schema guide.](https://graphql.org/learn/schema/#interfaces)

```graphql
query HeroForEpisode($ep: Episode!) {
  hero(episode: $ep) {
    name
    ... on Droid {
      primaryFunction
    }
    ... on Human {
      height
    }
  }
}
```

```json
# variables
{
  "ep": "JEDI"
}
```

```json
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "primaryFunction": "Astromech"
    }
  }
}
```

In this query, the `hero` field returns the type `Character`, which might be either a `Human` or a `Droid` depending on the `episode` argument. In the direct selection, you can only ask for fields that exist on the `Character` interface, such as `name`.

Named fragments can also be used in the same way, since a named fragment always has a type attached.

### Meta fields
```graphql
{
  search(text: "an") {
    __typename
    ... on Human {
      name
    }
    ... on Droid {
      name
    }
    ... on Starship {
      name
    }
  }
}
```

```json
{
  "data": {
    "search": [
      {
        "__typename": "Human",
        "name": "Han Solo"
      },
      {
        "__typename": "Human",
        "name": "Leia Organa"
      },
      {
        "__typename": "Starship",
        "name": "TIE Advanced x1"
      }
    ]
  }
}
```

In the above query, `search` returns a union type that can be one of three options. It would be impossible to tell apart the different types from the client without the `__typename` field.

GraphQL services provide a few meta fields, the rest of which are used to expose the [Introspection](https://graphql.org/learn/introspection/) system.


# Execution
>Source: https://graphql.org/learn/execution/

You can think of each field in a GraphQL query as a function or method of the previous type which returns the next type. In fact, this is exactly how GraphQL works. Each field on each type is backed by a function called the _resolver_ which is provided by the GraphQL server developer. When a field is executed, the corresponding _resolver_ is called to produce the next value. If a field produces a scalar value like a string or number, then the execution completes.

Let's take this example
```graphql
type Query {
  human(id: ID!): Human
}

type Human {
  name: String
  appearsIn: [Episode]
  starships: [Starship]
}

enum Episode {
  NEWHOPE
  EMPIRE
  JEDI
}

type Starship {
  name: String
}
```

```graphql
{
  human(id: 1002) {
    name
    appearsIn
    starships {
      name
    }
  }
}
```

```json
{
  "data": {
    "human": {
      "name": "Han Solo",
      "appearsIn": [
        "NEWHOPE",
        "EMPIRE",
        "JEDI"
      ],
      "starships": [
        {
          "name": "Millenium Falcon"
        },
        {
          "name": "Imperial shuttle"
        }
      ]
    }
  }
}
```

## Root fields & resolvers
At the top level of every GraphQL server is a type that represents all of the possible entry points into the GraphQL API, it's often called the _Root_ type or the _Query_ type.

A resolver function accesses the database and returns the the field value, it receives four arguments:
-   `obj` The previous object, which for a field on the root Query type is often not used.
-   `args` The arguments provided to the field in the GraphQL query.
-   `context` A value which is provided to every resolver and holds important contextual information like the currently logged in user, or access to a database.
-   `info` A value which holds field-specific information relevant to the current query as well as the schema details, also refer to [type GraphQLResolveInfo for more details](https://graphql.org/graphql-js/type/#graphqlobjecttype).

Resolvers can be asynchrnous, GraphQL will wait for Promises, Futures, and Tasks to complete before continuing and will do so with optimal concurrency.

```javascript
Query: {
  human(obj, args, context, info) {
    return context.db.loadHumanByID(args.id).then(
      userData => new Human(userData)
    )
  }
}
```

## Scalar coercion
```javascript
Human: {
  appearsIn(obj) {
    return obj.appearsIn // returns [ 4, 5, 6 ]
  }
}
```

Notice that our type system claims `appearsIn` will return Enum values with known values, the type system knows what to expect and will convert the values returned by a resolver function into something that upholds the API contract. In this case, there may be an Enum defined on our server which uses numbers like `4`, `5`, and `6` internally, but represents them as Enum values in the GraphQL type system.

## Introspection
It's often useful to ask a GraphQL schema for information about what queries it supports. GraphQL allows us to do so using introspection root fields like `__schema`, `__type` and `__typename`

**Examples:**
1. List all available types
```graphql
{
  __schema {
    types {
      name
    }
  }
}
```

```json
{
  "data": {
    "__schema": {
      "types": [
        {
          "name": "Query"
        },
        {
          "name": "String"
        },
        {
          "name": "ID"
        },
        {
          "name": "Mutation"
        },
        {
          "name": "Episode"
        },
        {
          "name": "Character"
        },
        {
          "name": "Int"
        },
        {
          "name": "LengthUnit"
        },
        {
          "name": "Human"
        },
        {
          "name": "Float"
        },
        {
          "name": "Droid"
        },
        {
          "name": "FriendsConnection"
        },
        {
          "name": "FriendsEdge"
        },
        {
          "name": "PageInfo"
        },
        {
          "name": "Boolean"
        },
        {
          "name": "Review"
        },
        {
          "name": "ReviewInput"
        },
        {
          "name": "Starship"
        },
        {
          "name": "SearchResult"
        },
        {
          "name": "__Schema"
        },
        {
          "name": "__Type"
        },
        {
          "name": "__TypeKind"
        },
        {
          "name": "__Field"
        },
        {
          "name": "__InputValue"
        },
        {
          "name": "__EnumValue"
        },
        {
          "name": "__Directive"
        },
        {
          "name": "__DirectiveLocation"
        }
      ]
    }
  }
}
```

2. Check the fields of a given type
```graphql
{
  __type(name: "Droid") {
    name
    fields {
      name
      type {
        name
        kind
        ofType {
          name
          kind
        }
      }
    }
  }
}
```

```json
{
  "data": {
    "__type": {
      "name": "Droid",
      "fields": [
        {
          "name": "id",
          "type": {
            "name": null,
            "kind": "NON_NULL",
            "ofType": {
              "name": "ID",
              "kind": "SCALAR"
            }
          }
        },
        {
          "name": "name",
          "type": {
            "name": null,
            "kind": "NON_NULL",
            "ofType": {
              "name": "String",
              "kind": "SCALAR"
            }
          }
        },
        {
          "name": "friends",
          "type": {
            "name": null,
            "kind": "LIST",
            "ofType": {
              "name": "Character",
              "kind": "INTERFACE"
            }
          }
        },
        {
          "name": "friendsConnection",
          "type": {
            "name": null,
            "kind": "NON_NULL",
            "ofType": {
              "name": "FriendsConnection",
              "kind": "OBJECT"
            }
          }
        },
        {
          "name": "appearsIn",
          "type": {
            "name": null,
            "kind": "NON_NULL",
            "ofType": {
              "name": null,
              "kind": "LIST"
            }
          }
        },
        {
          "name": "primaryFunction",
          "type": {
            "name": "String",
            "kind": "SCALAR",
            "ofType": null
          }
        }
      ]
    }
  }
}
```

