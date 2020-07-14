---
title: "Creating Static pages"
keywords:
  - React
  - JavaScript
  - Static-Pages
…

# JSX
- Gatsby Pages are essentially React components 
- JSX code that gets written is transpiled into JavaScript by Babel

# Creating Pages
The default way add pages to Gatsby is to put the pages in the /src/pages directory
> Gatsby will automagically set up routes for you

The pages added should be React components to fully benefit from the React integration. Also, by default the Gatsby cli only recognizes Javascript pages in the folder.

Sample Page:
```javascript
import React from "react"

export default () => (
    <div>
        <h1>This is the index page</h1>
        <div>
            <a href="/">Home</a> | <a href="/about">About</a>
        </div>
        <p>
            Welcome to the landing page!
        </p>
    </div>
) 
```

# Linking Pages
Even though Gatsby handles routing for you, you still need a way to add Links to your site.
Gatsby has this covered with a special <Link> component that can be used with internal pages:

- Add a new import ```import { Link } from "gatsby"```
- Add the link to the component: ```<Link to="/">Home</Link>```
- For external pages, continue to use HTML <a> tags

- Using Links for internal pages also results in much faster switching of pages, since the engine does not have to interpret the path to the Link tag each time it is called, unlike the <a> tag.