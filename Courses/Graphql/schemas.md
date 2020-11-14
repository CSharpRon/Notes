---
title: "GraphQL Schemas"
---

Within GraphiQL, you can see the schemas for queries you can execute as well as they types for those objects (String, Floats, Null, Enum, List, Object, etc.)

>  An exclamation point means that it is required:

<img src="file:///C:/Users/ramarrer/AppData/Roaming/marktext/images/2020-07-16-10-22-39-image.png" title="" alt="" width="670">

Other than GraphiQL, you can also query the schema directly to see all of the query types:

```graphql
query {
    __schema {
        queryType {
            name
            description
            fields {
                name
                description
                isDeprecated
                deprecationReason
            }
        }
    }
}
```

## Deprecated Fields

What is interesting about the above query is that you will also find fields that are deprecated, with a reason showing *why* they are deprecated. This is a huge advantage over REST api. In REST, you would usually define a new route or group (/api/v1 /api/v2, etc). With GraphQL, you can just mark the field as deprecated and preserve backwards compatibility!
