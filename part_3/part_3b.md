# part 3b

> table of contents

## Inhaltsverzeichnis

- [Same origin policy and CORS](#Same-origin-policy-and-CORS)
- [Application to the Internet](#Application-to-the-Internet)
- [Frontend production build](#Frontend-production-build)
- [Serving static files from the backend](#Serving-static-files-from-the-backend)
- [The whole app to internet](#The-whole-app-to-internet)
- [Streamlining deploying of the frontend](#Streamlining-deploying-of-the-frontend)
- [Proxy](#Proxy)
- [exercises](#exercises)

> Next let's connect the frontend we made in part 2 to our own backend.

> In the previous part, the frontend could ask for the list of notes from the json-server we had as a backend, from the address http://localhost:3001/notes. Our backend has a slightly different url structure now, as the notes can be found at http://localhost:3001/api/notes. Let's change the attribute baseUrl in the src/services/notes.js like so:

```javascript
import axios from 'axios'
const baseUrl = 'http://localhost:3001/api/notes'

const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}

// ...

export default { getAll, create, update }
```

> Now frontend's GET request to http://localhost:3001/api/notes does not work for some reason:

!["fullstack content"](./images/part3b_image1.png?raw=true)

> What's going on here? We can access the backend from a browser and from postman without any problems.

## Same origin policy and CORS

> The issue lies with a thing called CORS, or Cross-Origin Resource Sharing.

> According to Wikipedia:

> - Cross-origin resource sharing (CORS) is a mechanism that allows restricted resources (e.g. fonts) on a web page to be requested from another domain outside the domain from which the first resource was served. A web page may freely embed cross-origin images, stylesheets, scripts, iframes, and videos. Certain "cross-domain" requests, notably Ajax requests, are forbidden by default by the same-origin security policy.

> In our context the problem is that, by default, the JavaScript code of an application that runs in a browser can only communicate with a server in the same origin. Because our server is in localhost port 3001, and our frontend in localhost port 3000, they do not have the same origin.

> Keep in mind, that same origin policy and CORS are not specific to React or Node. They are in fact universal principles of the operation of web applications.

> We can allow requests from other origins by using Node's cors middleware.

> In your backend repository, install cors with the command

```
npm install cors
```

```javascript
const cors = require('cors')

app.use(cors())
```

!["fullstack content"](./images/part3a_image2.png?raw=true)

> The react app running in the browser now fetches the data from node/express-server that runs in localhost:3001.

## Application to the Internet

> Now that the whole stack is ready, let's move our application to the internet.

> There are an ever growing number of services that can be used to host an app on the internet. The convenient for a developer are so called PaaS (i.e. Platform as a Service) platforms that take care of installing the execution environment (eg. Node.js) and could also provide various services such as databases.

> For a decade already, Heroku has been dominating the PaaS scene. In August 2022 Heroku announced that they will end their free tier on 27th November 2022. This is very unfortunate for many developers, especially students.

> One of the most promising replacements for Heroku is Fly.io which has a free plan, so we have selected Fly.io as the second "official" hosting platform of this course. You are of course allowed to use another service if you wish.

> There are also some other free options for Heroku replacements besides Fly.io, eg. Render that works well for the purposes of this course, at least for all parts other than the part 11 (CI/CD) that might have one tricky exercise for other platforms.

> Some course participants have also used the following

> - Railway
> - Cyclic
> - Replit
> - CodeSandBox

> If you know some other good and easy to use services for hosting NodeJS, please let us know!

> For both Fly.io and Heroku, we need to change the definition of the port our application uses at the bottom of the index.js file like so: 

```javascript
const PORT = process.env.PORT || 3001
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```

> Now we are using the port defined in the environment variable PORT or port 3001 if the environment variable PORT is undefined. Fly.io and Heroku configure the application port based on that environment variable. 

### Fly.io

> If you decide to use Fly.io begin by installing their flyctl executable following this guide. After that you should create a Fly.io account.

> By default everyone gets two free virtual machines that can be used for running two apps at the same time.

> Note that the Fly.io instructions have only been added to this course on the 28th of August 2022. If you run into problems, please ask for help on Discord!

> Start by authenticating via command line with the command

```
fly auth login
```

> Note if the command fly does not work on your machine, you can try the longer version flyctl. Eg. on MacOS, both forms of the command work.

> Initializing an app happens by running the following command in the root directory of the app

```
fly launch
```

> Give the app a name or let Fly.io auto generate one. Pick a region where the app will be run. Do not create a postgress database for the app since it is not needed.

> The last question is "Would you like to deploy now?", answer yes and your app is also deployed to the Fly.io servers.

> If all goes well, the app should now be up and running. You can open it in the browser with the command

```
fly open
```

> After the initial setup, when the app code has been updated, it can be deployed to production with the command

```
fly deploy
```

> A particularly important command is fly logs that can be used to view server logs. It is best to keep logs always visible!

> Fly.io creates a file fly.toml in the root of your app. The file contains all the configuration of your server. On this course we can mostly ignore the contents of the file.

> Note: In some cases (the cause is so far unknown) running Fly.io commands especially on Windows WSL has caused problems. If the following command just hangs

```
flyctl ping -o personal
```

> your computer can not for some reason connect to Fly.io. If this happens to you, this describes one possible way to proceed.

> If output of the below command looks like this:

```javascript
$ flyctl ping -o personal
35 bytes from fdaa:0:8a3d::3 (gateway), seq=0 time=65.1ms
35 bytes from fdaa:0:8a3d::3 (gateway), seq=1 time=28.5ms
35 bytes from fdaa:0:8a3d::3 (gateway), seq=2 time=29.3ms
...
```

> then there are no connection problems!

### Heroku

> Let us also look at how we would use the good old Heroku for hosting an app.

> - If you have never used Heroku before, you can find instructions from Heroku documentation or by Googling.

> Add a file called Procfile to the backend project's root to tell Heroku how to start the application. 

```
web: node index.js
```

> Create a Git repository in the project directory, and add .gitignore with the following contents

```
node_modules
```

> Create a Heroku account in https://devcenter.heroku.com/. Install the Heroku package using the command: npm install -g heroku. Create a Heroku application with the command heroku create, commit your code to the repository and move it to Heroku with the command git push heroku main.

> If everything went well, the application works:

!["fullstack content"](./images/part3a_image3.png?raw=true)

> If not, the issue can be found by reading heroku logs with command heroku logs.

> - NB At least in the beginning it's good to keep an eye on the heroku logs at all times. The best way to do this is with command heroku logs -t which prints the logs to console whenever something happens on the server.

> - NB If you are deploying from a git repository where your code is not on the main branch (i.e. if you are altering the notes repo from the last lesson) you will need to run git push heroku HEAD:master. If you have already done a push to heroku, you may need to run git push heroku HEAD:main --force.

> The frontend also works with the backend on Fly.io or Heroku. You can check this by changing the backend's address on the frontend to be the backend's address in Fly.io/Heroku instead of http://localhost:3001.

> The next question is, how do we deploy the frontend to the Internet? We have multiple options. Let's go through one of them next. 

## Frontend production build

> So far we have been running React code in development mode. In development mode the application is configured to give clear error messages, immediately render code changes to the browser, and so on.

> When the application is deployed, we must create a production build or a version of the application which is optimized for production.

> A production build of applications created with create-react-app can be created with command npm run build.

> NOTE: at the time of writing (20th January 2022) create-react-app had a bug that causes the following error TypeError: MiniCssExtractPlugin is not a constructor

> A possible fix is found from here. Add the following to the file package.json 

```javascript
{
  // ...
  "resolutions": {
    "mini-css-extract-plugin": "2.4.5"
  }
}
```

> and run commands

```javascript
rm -rf package-lock.json
rm -rf node_modules
npm cache clean --force
npm install
```

> fAfter these npm run build should work.

> Let's run this command from the root of the frontend project.

> This creates a directory called build (which contains the only HTML file of our application, index.html ) which contains the directory static. Minified version of our application's JavaScript code will be generated to the static directory. Even though the application code is in multiple files, all of the JavaScript will be minified into one file. Actually all of the code from all of the application's dependencies will also be minified into this single file.

> The minified code is not very readable. The beginning of the code looks like this: 

```javascript
!function(e){function r(r){for(var n,f,i=r[0],l=r[1],a=r[2],c=0,s=[];c<i.length;c++)f=i[c],o[f]&&s.push(o[f][0]),o[f]=0;for(n in l)Object.prototype.hasOwnProperty.call(l,n)&&(e[n]=l[n]);for(p&&p(r);s.length;)s.shift()();return u.push.apply(u,a||[]),t()}function t(){for(var e,r=0;r<u.length;r++){for(var t=u[r],n=!0,i=1;i<t.length;i++){var l=t[i];0!==o[l]&&(n=!1)}n&&(u.splice(r--,1),e=f(f.s=t[0]))}return e}var n={},o={2:0},u=[];function f(r){if(n[r])return n[r].exports;var t=n[r]={i:r,l:!1,exports:{}};return e[r].call(t.exports,t,t.exports,f),t.l=!0,t.exports}f.m=e,f.c=n,f.d=function(e,r,t){f.o(e,r)||Object.defineProperty(e,r,{enumerable:!0,get:t})},f.r=function(e){"undefined"!==typeof Symbol&&Symbol.toStringTag&&Object.defineProperty(e,Symbol.toStringTag,{value:"Module"})
```

## Serving static files from the backend

> One option for deploying the frontend is to copy the production build (the build directory) to the root of the backend repository and configure the backend to show the frontend's main page (the file build/index.html) as its main page.

> We begin by copying the production build of the frontend to the root of the backend. With a Mac or Linux computer, the copying can be done from the frontend directory with the command

```
cp -r build ../notes-backend
```

> If you are using a Windows computer, you may use either copy or xcopy command instead. Otherwise, simply do a copy and paste.

> The backend directory should now look as follows:

!["fullstack content"](./images/part3b_image4.png?raw=true)

> To make express show static content, the page index.html and the JavaScript, etc., it fetches, we need a built-in middleware from express called static.

> When we add the following amidst the declarations of middlewares

```javascript
app.use(express.static('build'))
```

> whenever express gets an HTTP GET request it will first check if the build directory contains a file corresponding to the request's address. If a correct file is found, express will return it.

> Now HTTP GET requests to the address www.serversaddress.com/index.html or www.serversaddress.com will show the React frontend. GET requests to the address www.serversaddress.com/api/notes will be handled by the backend's code.

> Because of our situation, both the frontend and the backend are at the same address, we can declare baseUrl as a relative URL. This means we can leave out the part declaring the server. 

```javascript
import axios from 'axios'
const baseUrl = '/api/notes'

const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}

// ...
```

After the change, we have to create a new production build and copy it to the root of the backend repository.

The application can now be used from the backend address http://localhost:3001:

!["fullstack content"](./images/part3b_image5.png?raw=true)

> Our application now works exactly like the single-page app example application we studied in part 0.

> When we use a browser to go to the address http://localhost:3001, the server returns the index.html file from the build repository. Summarized contents of the file are as follows: 

```html
<head>
  <meta charset="utf-8"/>
  <title>React App</title>
  <link href="/static/css/main.f9a47af2.chunk.css" rel="stylesheet">
</head>
<body>
  <div id="root"></div>
  <script src="/static/js/1.578f4ea1.chunk.js"></script>
  <script src="/static/js/main.104ca08d.chunk.js"></script>
</body>
</html>
```

> The file contains instructions to fetch a CSS stylesheet defining the styles of the application, and two script tags which instruct the browser to fetch the JavaScript code of the application - the actual React application.

> The React code fetches notes from the server address http://localhost:3001/api/notes and renders them to the screen. The communications between the server and the browser can be seen in the Network tab of the developer console:

!["fullstack content"](./images/part3b_image6.png?raw=true)

> The setup that is ready for product deployment looks as follows:

!["fullstack content"](./images/part3b_image7.png?raw=true)

> Unlike when running the app in a development environment, everything is now in the same node/express-backend that runs in localhost:3001. When the browser goes to the page, the file index.html is rendered. That causes the browser to fetch the product version of the React app. Once it starts to run, it fetches the json-data from the address localhost:3001/api/notes.

## The whole app to internet

> After ensuring that the production version of the application works locally, commit the production build of the frontend to the backend repository, and push the code to Heroku again. In the case of Fly.io the new deployment is done with the command

```
fly deploy
```

> The application works perfectly, except we haven't added the functionality for changing the importance of a note to the backend yet. 

!["fullstack content"](./images/part3b_image8.png?raw=true)

> Our application saves the notes to a variable. If the application crashes or is restarted, all of the data will disappear.

> The application needs a database. Before we introduce one, let's go through a few things.

> The setup looks like now as follows:

!["fullstack content"](./images/part3b_image9.png?raw=true)

> The node/express-backend now resides in the Fly.io/Heroku server. When the root address that is of the form https://glacial-ravine-74819.herokuapp.com/ is accessed, the browser loads and executes the React app that fetches the json-data from the Heroku server.

## Streamlining deploying of the frontend

> To create a new production build of the frontend without extra manual work, let's add some npm-scripts to the package.json of the backend repository.

### Fly.io

> The script looks like this

```javascript
{
  "scripts": {
    // ...
    "build:ui": "rm -rf build && cd ../part2-notes/ && npm run build && cp -r build ../notes-backend",
    "deploy": "fly deploy",
    "deploy:full": "npm run build:ui && npm run deploy",    
    "logs:prod": "fly logs"
  }
}
```

> The script npm run build:ui builds the frontend and copies the production version under the backend repository. npm run deploy releases the current backend to Fly.io.

> npm run deploy:full combines these two scripts.

> There is also a script npm run logs:prod to show the Fly.io logs.

> Note that the directory paths in the script build:ui depend on the location of repositories in the file system.

### Heroku

> In case of Heroku, the scripts look like the following

```javascript
{
  "scripts": {
    //...
    "build:ui": "rm -rf build && cd ../part2-notes/ && npm run build && cp -r build ../notes-backend",
    "deploy": "git push heroku main",
    "deploy:full": "npm run build:ui && git add . && git commit -m uibuild && npm run deploy",    
    "logs:prod": "heroku logs --tail"
  }
}
```

> The script npm run build:ui builds the frontend and copies the production version under the backend repository. npm run deploy releases the current backend to Heroku.

> npm run deploy:full combines these two and contains the necessary git commands to update the backend repository.

> There is also a script npm run logs:prod to show the Heroku logs.

> Note that the directory paths in the script build:ui depend on the location of repositories in the file system.

> - NB On Windows, npm scripts are executed in cmd.exe as the default shell which does not support bash commands. For the above bash commands to work, you can change the default shell to Bash (in the default Git for Windows installation) as follows:


```
npm config set script-shell "C:\\Program Files\\git\\bin\\bash.exe"
```

> Another option is the use of shx.

## Proxy

> Changes on the frontend have caused it to no longer work in development mode (when started with command npm start), as the connection to the backend does not work. 

!["fullstack content"](./images/part3b_image10.png?raw=true)

> This is due to changing the backend address to a relative URL: 

```javascript
const baseUrl = '/api/notes'
```

> Because in development mode the frontend is at the address localhost:3000, the requests to the backend go to the wrong address localhost:3000/api/notes. The backend is at localhost:3001.

> If the project was created with create-react-app, this problem is easy to solve. It is enough to add the following declaration to the package.json file of the frontend repository. 

```javascript
{
  "dependencies": {
    // ...
  },
  "scripts": {
    // ...
  },
  "proxy": "http://localhost:3001"}
```

> After a restart, the React development environment will work as a proxy. If the React code does an HTTP request to a server address at http://localhost:3000 not managed by the React application itself (i.e. when requests are not about fetching the CSS or JavaScript of the application), the request will be redirected to the server at http://localhost:3001.

> Now the frontend is also fine, working with the server both in development- and production mode.

> A negative aspect of our approach is how complicated it is to deploy the frontend. Deploying a new version requires generating new production build of the frontend and copying it to the backend repository. This makes creating an automated deployment pipeline more difficult. Deployment pipeline means an automated and controlled way to move the code from the computer of the developer through different tests and quality checks to the production environment. Building a deployment pipeline is the topic of part 11 of this course.

> There are multiple ways to achieve this (for example placing both backend and frontend code to the same repository ) but we will not go into those now.

> In some situations it may be sensible to deploy the frontend code as its own application. With apps created with create-react-app it is straightforward.

> Current code of the backend can be found on Github, in the branch part3-3. The changes in frontend code are in part3-1 branch of the frontend repository.

## exercises

> The following exercises don't require many lines of code. They can however be challenging, because you must understand exactly what is happening and where, and the configurations must be just right.

### 3.9 phonebook backend step9

> Make the backend work with the phonebook frontend from the exercises of the previous part. Do not implement the functionality for making changes to the phone numbers yet, that will be implemented in exercise 3.17.

> You will probably have to do some small changes to the frontend, at least to the URLs for the backend. Remember to keep the developer console open in your browser. If some HTTP requests fail, you should check from the Network-tab what is going on. Keep an eye on the backend's console as well. If you did not do the previous exercise, it is worth it to print the request data or request.body to the console in the event handler responsible for POST requests.

### 3.10 phonebook backend step10

> Deploy the backend to the internet, for example to Heroku.

> NB the command heroku works on the department's computers and the freshman laptops. If for some reason you cannot install Heroku to your computer, you can use the command npx heroku.

> Test the deployed backend with a browser and Postman or VS Code REST client to ensure it works.

> PRO TIP: When you deploy your application to Heroku, it is worth it to at least in the beginning keep an eye on the logs of the heroku application AT ALL TIMES with the command heroku logs -t.

> The following is a log about one typical problem. Heroku cannot find application dependency express:

!["fullstack content"](./images/part3b_image11.png?raw=true)

> The reason is that the express package has not been installed with the npm install express command, so information about the dependency was not saved to the file package.json.

> Another typical problem is that the application is not configured to use the port set to environment variable PORT: 

!["fullstack content"](./images/part3b_image12.png?raw=true)

> Create a README.md at the root of your repository, and add a link to your online application to it.

### 3.11 phonebook full stack

> Generate a production build of your frontend, and add it to the internet application using the method introduced in this part.

> NB If you use Heroku, make sure the directory build is not gitignored

> Also make sure that the frontend still works locally (in development mode when started with command npm start).

> If you have problems to get the app working make sure that your directory structure matches the one of the example app.