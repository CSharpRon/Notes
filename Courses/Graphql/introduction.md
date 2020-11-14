---
title: "Introduction to GraphQL"
---

## What is Graphql?

Declarative data fetching specification created by Facebook in 2012

The problem with traditional REST is that many endpoints end up being needed to fetch subsets of data:

```http
GET: /api/florida/orlando
GET: /api/florida/orlando/people
GET: /api/florida/orlando/people/people_with_hats
etc...
```

GraphQL lets you access data in a *declarative* way. This means that you do not have to define a bunch of routes ahead of time. Here is how you can fetch data in GraphQL:

```graphql
# Gets all states. Returns a JSON object.
{
    state {
        name
        population
        people
    }
}
```

Another benefit is that you do not need multiple calls to get data of different shapes. For example, say you want to get all states AND all people in that state. In REST, you would either have to modify the endpoint to return both pieces of data or you would do 2 separate calls. With GraphQL, you just define the data you want returned!
