---
title: "Handling Data"
---

## Aliases

Up until now, we have covered how to query for a single set of information. For example, here is how you would get all of the repository information for the graphql repository from the GitHub API:

```graphql
{
    repository(name: "graphql", owner: "facebook") {
        id
        description
        homepageURL
    }
}
```

But if you also wanted to get information about the react API, you can't just copy and paste since GraphQL doesn't know how which result to actually return (conflict since two of the same object types are returned):

```graphql
# Won't Work
{
    repository(name: "graphql", owner: "facebook") {
        id
        description
        homepageURL
    }
    repository(name: "react", owner: "facebook") {
        id
        description
        homepageURL
    }
}
```

The solution is to **alias** the result so that it can appear as its own node in the JSON response:

```graphql
{
    graphqlProject: repository(name: "graphql", owner: "facebook") {
        id
        description
        homepageURL
    }
    reactProject: repository(name: "react", owner: "facebook") {
        id
        description
        homepageURL
    }
}
```

This will return:

![](C:\Users\ramarrer\AppData\Roaming\marktext\images\2020-07-16-10-56-46-image.png)

## Fragments

The above solution is great but we can do better than manually specifying the return fields for each data set.

Enter **fragments**. Fragments allow you to specify recurring fields, and then only pass around the fragment name to get those fields back. They are specified with the *fragment* keyword and the object type. They can then be called with `...fragmentName` (note the three dots)

```graphql
{
    graphqlProject: repository(name: "graphql", owner: "facebook") {
        ...repoFields
    }
    reactProject: repository(name: "react", owner: "facebook") {
        ...repoFields
    }
}

fragment repoFields on Repository {
    id
    description
    homepageURL
}
```

As you can imagine, fragments are heavily used in GraphQL to allow developers to reuse fields, especially in UI elements.

## Nested Fields

Not all data lives in the same list. That's pretty much a given. But how do you represent related lists in a query? For example, how would you query for a list of information from a particular GitHub user *and* get linked information for that user's repositories?

```graphql
{
    viewer {
        id
        name
        isEmployee
        location
        databaseId
        repositories(first: 5) {
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

Notice the new `edges` word. This is the GraphQL indicator for a related list. The edges have node objects which in this case is a repository object. Boom, just like that we got related information. No need to do multiple queries. 

This works thanks to **connections**:

![](C:\Users\ramarrer\AppData\Roaming\marktext\images\2020-07-16-11-50-31-image.png)

The `repositories` field that we specified in the query is of type `RepositoryConnection`. 

If you need to have multiple nested fields, it is very easy to do and does not require alias usage. 

```graphql
{
    viewer {
        id
        name
        isEmployee
        location
        databaseId
        repositories(first: 5) {
            edges {
                node {
                    id
                    name
                    watchers(first: 5) {
                        id
                        name
                        organization
}
                }
            }
        }
        organizations(last: 5) {
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


