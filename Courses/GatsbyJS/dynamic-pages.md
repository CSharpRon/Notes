---
title: "Creating Dynamic Pages Programatically"
keywords:
  - React
  - Database
  - GraphQL
---

Imagine only defining pages (like blog posts) in Markdown and then dynamically creating web pages. With Gatsby, we can make this a reality!

These are known as build-time pages and are possible thanks to the Gatsby API.

## Gatsby API
First, know that node is an object

``` js
if (node.internal.type === 'MarkdownRemark') {
    // Do something interesting
}
```

The gatsby-node.js is the configuration file that Gatsby requires to know what to do with each node:
``` js
// example gatsby-node.js
exports.onCreateNode = ({ node }) => {
    /// Runs custom code to the onCreateNote build process
    if (node.internal.type === 'MarkdownRemark') {
        console.log(`*** I am processing a node with type: ${node.internal.type}`)
    }
}
```

If you want to create a slug dynamically, you can do it with the gatsby-source-filesystem plugin:
``` js
// Note that this is CommonJS syntax, not ES6, since it works better with Node
const { createFilePath } = require('gatsby-source-filesystem')

// This code adds a new field to each node called slug so that it can be used to create paths
exports.onCreateNode = ({ node, getNode, actions }) => {
    if (node.internal.type === 'MarkdownRemark') {
        // Restructuring assignment: ES6 statement that simplifies (const createNodeField = actions.createNodeField)
        const { createNoteField } = actions 
        const slug = createFilePath({ node, getNode, basePath: 'markdown' })
        createNodeField({
            node,
            name: 'slug',
            value: slug,
        })
    }
}
```

## Creating a Page Template
You should define a JSX component to define how the dynamic page will look. Since this is a template used at build time, you should save it in the src/templates folder

``` js
// src/templates/page.js

import React from 'react'
import { graphql } from 'gatsby'
import Layout from '../components/layout'
import Title from '../components/title'
import styles from './post.module.scss'

export default ({ data }) => {
    const post = data.markdownRemark
    
    return (
		<Layout>
        	<div className={styles.container}>
        		<Title text={post.frontmatter.title}></Title>
				<div style={{ width: '100%', high: '200px', backgroundColor: '#fafafa'}} />
				<div className={styles.content} dangerouslySetInnerHTML={{ __html: post.html }} />
        </Layout>
    )
}

export const query = qraphql`
	query($slug: String!) {
		markdownRemark(fields: { slug: { eq: $slug } }) {
			html
			frontmatter {
				title
				keywords
            }
        }
    } 
`
```

## Actually creating the page (last part)
Ok, up until here we have added the code to dynamically generate the slug (url) for new posts and we even defined a template for how the page will look. Now we just need to tell Gatsby to actually build the pages. Once again, we will go back to gatsby-node.js

``` js
// Note that this is CommonJS syntax, not ES6, since it works better with Node
const { createFilePath } = require('gatsby-source-filesystem')
cont path = require('path')

// This code adds a new field to each node called slug so that it can be used to create paths
exports.onCreateNode = ({ node, getNode, actions }) => {
    if (node.internal.type === 'MarkdownRemark') {
        // Restructuring assignment: ES6 statement that simplifies (const createNodeField = actions.createNodeField)
        const { createNoteField } = actions 
        const slug = createFilePath({ node, getNode, basePath: 'markdown' })
        createNodeField({
            node,
            name: 'slug',
            value: slug,
        })
    }
}

// Create post pages programatically
exports.createPages = ({ graphql, actions }) => {
    const { createPage } = actions
    
    return new Promise(resolve => {
        graphql(`
		{
			allMarkdownRemark {
				edges {
					node {
						fields {
							slug
                        }
                    }
                }
            }
        }`)
    }).then(result => {
        result.data.allMarkdownRemark.edges.forEach(({ node })) => {
            createPage({
                path: node.fields.slug,
                component: path.resolve('./src/templates/post.js'),
                context: {
                    slug: node.fields.slug,
                },
            })
        })
        resolve()
    })
}
```