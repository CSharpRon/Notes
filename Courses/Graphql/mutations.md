---
title: "Variables, Operations, and Mutations"
---

## Operations

So far, our queries have been implicitly defined (the first character is the opening curly brace). If you want to explicitly define the operation, you just preface it with query:

```graphql
query {
    organization(login: "facebook") {
        id
        name
        members(first: 5) {
            edges {
                node {
                    id
                    name
                }
            }
        }
    }
}
```

But what happens when you want to reuse this query in other parts of your software? Well, we will need to give it a name so that we can reference it later. To do this, just add a helpful name after the `query` keyword:

```graphql
query FirstFiveOrgMembers{
    organization(login: "facebook") {
        id
        name
        members(first: 5) {
            edges {
                node {
                    id
                    name
                }
            }
        }
    }
}
```

## Variables

Now that we can define reusable queries, we will extend our understanding to allow for input parameters. Let's add a required input for the login name. Notice how we also have to specify the type:

```graphql
query FirstNOrgMembers($login: String!, $n: Int!){
    organization(login: $login) {
        id
        name
        members(first: $n) {
            edges {
                node {
                    id
                    name
                }
            }
        }
    }
}
```

## Mutations

Mutations are similar to PUT or DELETE in REST. However, instead of sending them as a route, you send data as a *payload* in a mutation. Since mutations are another operation, you define them with the `mutation` keyword.

```graphql
mutation NewComment($input: AddCommentInput!) {
    addComment(input: $input) {
        clientMutationId
        subject {
            id
        }
    }
}
```

Above, we are building on the GitHub addComment mutation. Once it is defined, GraphiQL will ask us to define the input. After we pass that in, the data gets updated!

> Mutations are not created by default, that is up to the GraphQL API. In other words, if you want someting to be mutable you will need to create a mutation!
