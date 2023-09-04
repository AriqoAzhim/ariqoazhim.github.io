Specify all of the fields as well as their types. GraphQL is a strongly typed language

The most basic components of a GraphQL schema are object-types. There are also Scalar types

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/11472ead-5b1c-4d01-8410-22c4438ad4d8/Untitled.png)

Every GraphQL service defines a set of types which completely describe the set of possible data you can query on that service. This is a schema

When a query comes in, the data checks that it matches the schema.

## Objects

The most basic components of a GraphQL schema are object types, which just represent a kind of object you can fetch from your service, and what fields it has. In the GraphQL schema language, we might represent it like this:

```graphql
type Character {
  name: String!
  appearsIn: [Episode!]!
}
```

- `Character` is a *GraphQL Object Type*, meaning it's a type with some fields. Most of the types in your schema will be object types.
- `name` and `appearsIn` are *fields* on the `Character` type. That means that `name` and `appearsIn` are the only fields that can appear in any part of a GraphQL query that operates on the `Character` type.
- `String` is one of the built-in *scalar* types - these are types that resolve to a single scalar object, and can't have sub-selections in the query. We'll go over scalar types more later.
- `String!` means that the field is *non-nullable*, meaning that the GraphQL service promises to always give you a value when you query this field. In the type language, we'll represent those with an exclamation mark.
- `[Episode!]!` represents an *array* of `Episode` objects. Since it is also *non-nullable*, you can always expect an array (with zero or more items) when you query the `appearsIn` field. And since `Episode!` is also *non-nullable*, you can always expect every item of the array to be an `Episode` object.