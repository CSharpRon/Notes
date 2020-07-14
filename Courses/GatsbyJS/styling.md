---
title: "Styling"
keywords:
  - React
  - JavaScript
  - SASS
  - CSS
…

# Styling Options in Gatsby
- CSS
- SASS, LESS
- CSS-in-JS (Allows you to specify styles that can be used by all React components)
- CSS Module (A CSS file in which all class names are scoped locally by default)
    - Name-spaced class names
    - Preferred method since Gatsby supports it by default

## CSS Module Naming Convention
To make a class file for a particular page (module) all you have to do is call the file with the following format: componentName.module.css   
- Example: index.js -> index.module.css
- In the module file, you can specify css like normal:
```css
.header {
    color: red
}
```
To make use of the CSS in your React component, you just need to import it and use the className field:
```js 
import React from "react"
import styles from "../pages/404.module.css"

export default () => (
    <div>
        <h1 className={styles.header}>Page not found</h1>
        <p className={styles.errorMessage}>
            This appears to be one of those "glitch in the matrix" scenarios...
        </p>
    </div>
)
```

## Global Styles
To create a global styles, define a global.css in the src/styles folder
Then, in the gatsby-browser.js file (present in the root of the website), add an import statement to the global style:
```js
import "./src/styles/global.css"
```

## SASS
Syntactically Awesome Style Sheets (CSS with superpowers)
- Used for more powerful styling than CSS alone

To install:
```sh
npm install --save node-sass gatsby-plugin-sass
```

To Use:
- The SASS extension for style sheets is .scss
- Then, you can import it from a React component as you would import a regular CSS file.
- One important feature of SASS is the concept of nested classes. If you are using nested classes, then you will need to enforce the hierarchy in the html code. For example, if in SASS you had the following CSS class hierarchy (.content -> .header), then you can enforce this relationship via the div tag:
```js
<div className={styles.content}>
    <h1 className={styles.header}>Header</h1>
</div>
```

### Variables
SASS will let you define styling variables that can be imported and used in other SCSS sheets. This can be extremely helpful to define a color pallete or max-width. 

To create a new variable, define it with the $ symbol and save it somewhere it can be accessed:
```scss
// src/styles/global.scss
$max-width: 400px;
```