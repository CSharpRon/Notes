---
title: "Components in GatsbyJS"
keywords:
  - React
  - JavaScript
…

# Types of React Components:
> *Page Components*
> - React components with a .js file extension under src/pages
> - Represents complete page, with UI and logic.   

> *Layout Components*
> - Also a React component. 
> - Represents the look of the page.
> - Reused throughout the site (like a header and a footer).   

> *Regular Components*
> - A React component too.
> - Building blocks of all we do in Gatsby.
> - If something needs to be reused, build it as a component.
> - Can be bundled together with other components to create larger components. 

## Creating Regular Components
Components that will be used by other components often make use of state. Specifically `props`.
```js
import React from 'react'

export default ({children}) => (
	<div className={styles.Layout}>
    	{children} // Will render any children elements inside this div
    </div>
)
--- New File ---
// To use in another component just call:
import React from 'react'
import Layout from '../components/{filename}'

export default () => (
	<Layout>
    	<p>Welcome to my page!</p
    </Layout>
)
```

## Creating Header Components
Header components should go under the src/components folder and can be imported into other components as we have seen before.
- Header components will often be used in all pages of a particular site, so make sure that you make it reusable (use global styling).

## Creating Header Link Components
Rather than creating Links with classNames all over the place, you can create a new component called HeaderLink (the name doesn't matter, this is one example) which defines the style for the link and returns a valid link object through props:
```js
const HeaderLink = props => (
	<Link className={styles.link} to={props.to}>{props.text}</Link>
)

export default () => (
	<div className={styles.container}>
    	<HeaderLink to='/' text='Home' />
        <HeaderLink to='/about' text='About' />
    </div>
)
```

## Good Example - Creating aa Social Media Button:
React components can have logic inside of them, especially if that logic is based on the props passed in. Check out this example of creating a social media button from the props passed in:
```js
// SocialButton component
// takes in the site and username to return an anchor tag
const SocialButton = (props) => (

	let style = '';
    let url = '';
    
    if (props.site === 'twitter') {
    	style = styles.buttonTwitter;
    	url = "https://twitter.com/" + props.username;
	}

	else if (props.site === 'Linkedin') {
        style = styles.buttonLinkedin;
        url = "https://www.linkedin.com/in/" + props.username;
    }

	else if (props.site === 'github') {
        style = styles.buttonGithub;
        url = "https://www.github.com/" + props.username;
    }

	return (
    	<a href={url} target="_blank" rel="noopener noreferrer">
        	<div className={style}>{props.children}&nbsp;</div>
        </a>
    )
)
```