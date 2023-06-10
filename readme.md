# MongoDB - Data that is accessed together should be stored together

## Documents
- The model has all structure that will be saved, and it is similar as a JSON. For example:
```
{
  "key": value,
  "key": value,
  "key" : value
}
```

## Collections
- A group of documents

## Database
- A container of collections

# Data modelling using mongodb - Relationships
- Put together data that is accessed together, avoiding search for relations amongst our models, saving
time and processing. So, when we have relationships we prefer to put it together.

##  Embedding: (when we really group the documents "together" in one document)
- This is used for one-to-many or many-to-many relationships.
- PROS:
  - Avoid joins.
  - Provides better performance for read operations.
  - Allows developers to update related data in a single write operation.
- WARNING:
  - Data duplication.
  - Large documents.
    - This large documents have to be read into memory in full, and it can make our application slow.
    - Adding data without limit creates unbounded documents.
```
Movies and casts
{
  "_id": ObjectID("1234"),
  "title": "Harry potter",
  "casts": [
    { "actor": "Daniel" },
    { "actor": "Emma" },
    { "actor": "Joao" },
  ]
}
```

##  Referencing: (when we do relations putting just the _id of another set)
- It is used when we want to store related information in separated documents.
- Using references is called linking or data normalization.
- PROS:
  - No duplication of data.
  - Smaller documents.
- WARNING:
  - Querying for multiple documents costs extra resources and impacts read performance.
```
Movies and casts
{
  "_id": ObjectID("1234"),
  "title": "Harry potter",
  "locations": [
    ObjectID("1233"),
    ObjectID("1234"),
    ObjectID("1235"),
  ]
}
```

## One-to-one
- When we have a direct relation between two data entities sets, for example:
```
Movies and director
{
  "_id": ObjectID("123"),
  "title": "Harry potter",
  "director": "JK"
}
```

## One-to-many
- When one entity set is connected to any number of data entities in another set, for example:
```
Movies and casts
{
  "_id": ObjectID("1234"),
  "title": "Harry potter",
  "casts": [
    { "actor": "Daniel" },
    { "actor": "Emma" },
    { "actor": "Joao" },
  ]
}
```

## Many-to-many
- When any number of data entities in one set are connected to any number of data entities in another set, for example:

# Scaling a data model

## Avoid unbonded documents (That grows infinitely)

### The problem:
  - It can happens when we use document embedding.
  - In this case we can grow the comments infinitely.
    - We will get a blog and it will load all comments, making the app slow because it's not possible to paginate.
    - We also have a maximum document size of 16MB and it can be a problem on future.
  For example:
  ```
    blog and comments
    {
      "_id": ObjectID("123"),
      "name": "my blog",
      "comments": [
        { ... }, { ... },
      ]
    }
  ```

### The solution
  - To use reference and not embedding documents.
  For example:
  ```
    `blog_post` collection
    {
      "_id": ObjectID("123"),
    }

  `comments` collection
  {
    "name": "John"
    "blog_entry_id": ObjectID("123")
  }
  ```

# Avoid schema anti-patterns
- Massive arrays.
- Massive number of collections.
- Bloated documents.
- Unnecessary indexes.
- Queries without indexes.
- Data that's accessed together, but stored in different collections.

# MongoDB atlas
- A way to host MongodDB on the cloud with data visualization and text search.
- There is also a lot of features to, for example:
  - Analyze your documents schemas and database performance giving you advices to make it better.