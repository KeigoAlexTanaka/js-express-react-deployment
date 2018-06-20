# Express & React application deployment

## Create a new React application

```bash
cra-minimal project-3;
cd project-3;
```

## Initialize a git repository

```bash
git init
```

## Install basic dependencies required for Express:

```bash
npm install --save express pg-promise pg-monitor bluebird
```

## Setup `server.js`:

```js
const express = require('express');
const path = require('path');
// Create a new Express application (web server)
const app = express();

// Set the port based on the environment variable (PORT=8080 node server.js)
// and fallback to 4567
const PORT = process.env.PORT || 4567;

// In production, any request that doesn't match a previous route
// should send the front-end application, which will handle the route.
if (process.env.NODE_ENV == "production") {
  app.get("/*", function(request, response) {
    response.sendFile(path.join(__dirname, "build", "index.html"));
  });
}

// Start the web server listening on the provided port.
app.listen(PORT, () => { 
  console.log(`Express web server listening on port ${PORT}`);
});
```

## Add a `proxy` property in your package.json

Add the following to your package.json:

```
"proxy": "http://localhost:4567"
```

Where `4567` is the port that your Express server runs on.

This allows the `create-react-app` server (usually running on 3000) [to proxy requests](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#proxying-api-requests-in-development) to our Express API back-end.

## Create a Procfile

A [`Procfile`](https://devcenter.heroku.com/articles/procfile) lets us specify what commands should be run on Heroku when we our application starts.

Create a `Procfile` in your repository, and build a static copy of the our assets (JavaScript, CSS, HTML, etc.) when the application starts:

```Procfile
web: npm run build; node server.js
```

## Make a commit and a repository

Make a git commit and a repository under one of your Github users, and give admin access on the repository to all of your teammates.

## Create a Heroku application

Follow our [Heroku cheat sheet](https://git.generalassemb.ly/wdi-nyc-tesseract/tesseract-class-info/wiki/Heroku-Deployment-Cheat-Sheet) to setup and deploy your app to Heroku. 

Add your teammates as collaborators on the Heroku app under the `Access` tab while viewing the project on their website.
