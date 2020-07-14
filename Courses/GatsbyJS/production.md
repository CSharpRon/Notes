---
title: "Deploying Gatsby sites"
keywords:
  - React
  - Database
  - GraphQL
---

## Building a Production Site
Up until now, we have been using the command ```gatsby develop``` to build and access our site.

With the ```gatsby build``` command, we can build all of the static files for the site and they get saved into the 'public' folder. From there, these files can be served on any web server.

## Previewing the Production site
If you want to preview the production build of the website, use ```gatsby serve```.

## Static site hosting advantages:

- No server to set up or databases to deploy
- Very quick compared to server-side rendered pages
- Very cheap to host (you can even host it on a CDN instead of Azure/AWS)


