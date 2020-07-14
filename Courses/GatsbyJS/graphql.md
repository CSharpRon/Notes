---
title: "GraphQL"
keywords:
  - React
  - GraphQL
…

Data is a first class citizen in Gatsby.

**GraphQL**
- A declarative way to interact with data
- Data can exist in databses, markdown, file systems, etc.
- Implemented in Gatsby via plugins
- Just a specification

Gatsby uses GraphQL to fetch data at build-time (Native behavior) and then creates static resources. 
- Gatsby can also fetch data at run time

Two types of GraphQL Queries:
- Page Queries: Run on page Components (ex index.js/about.js)
- Static Queries: On Non-Page components (ex header.js)

## *Note: Adding Metadata*
- Metadata for the site (such as the site title) can be set in the gatsby-config.js

## GraphiQL
- An in browser IDE for viewing GraphQL Data sources
- After running `gatsby develop`, a link will be provided to access the GraphiQL page.
- Running the following command (assuming you specified a title in the metadata) will return a JSON object:

``` json
{
	site {
    	siteMetadata {
        	title
        }
    }
}

/* Returns
{
	"data": {
    	"site": {
        	"siteMetadata": {
            	"title": "Gatsby Starter"
            }
        }
    }
}
*/
```

The awesome thing about this response is that you can call it directly in your pages. To do so, you will have to define a constant query object in your page component:
```js
import React from 'react'
import Layout from '../components/layout'
import Title from '../components/title' 

export default ({data}) => (
	<Layout>
		<Title text='{data.site.siteMetadata.title}'/>
    </Layout>
)

// Notice how the query matches the query we applied earlier in GraphiQL
export const query = graphql `query {
   site {
	siteMetadata {
		title
    }
   } 
}`
```

## Querying data from components
Unfortunately, the process is a little more involved when wanting to add queries to components (note components differ from pages since they can have their own functions):

- To make use of query data in components, you must first import the corresponding components from gatsby:
``` js
import React from 'react'
import { StaticQuery, graphql } from 'gatsby'

export default () => (
	<StaticQuery
    
    	query = {graphql `
			query {
				site {
					siteMetadata {
						title
                    }
                }
            }
		`}

		render = { data => (
			<header>
                  <h1 text='{data.site.siteMetadata.title}' />
            </header>
        )}
    
    />
)
```