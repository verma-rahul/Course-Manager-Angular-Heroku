## Course Management Application: Standalone Angular 5 Application on Heroku

An application developed in Angular 5 standAlone on Heroku


## Installaiton Help:

### Pre Requisitve:
 Node.js and npm  [here](https://www.npmjs.com/get-npm).(check `with node -v and npm -v`)


### Creating Angular App
- `npm install -g @angular/cli` to enable Angular ClI Globally
- `ng new <app-name>` to create started Angular App

## Linking to heroku (cd to angualar app first):
- `heroku create <heroku-app-name>` to create Heroku App of the Angular App.
- `heroku buildpacks:add --index 1 heroku/nodejs --app <heroku-app-name>` &
  `heroku buildpacks:add --index 2 https://github.com/heroku/heroku-buildpack-static.git --app <heroku-app-name>`
  to add the Heroku builpacks (Node and Nignix-Static-Server)

### Setup Proxy for Heroku :
- add `"postinstall": "ng build --prod"` to `scripts` section of `package.json` (This builds the app as Static asset, to be later used by NIGNIX static server used internally by static-buildpack)
- add a file `static.json` as given below:
- add the static.json as mentioned below, where `$API_URL` is to be set in config
variables on heroku to point to Backend-Server. (Please see the [https://github.com/heroku/heroku-buildpack-static] for more help)
`{
  "root": "dist/",
  "routes": {
    "/**": "index.html"
  },
  "proxies": {
    "/api": {
      "origin": "${API_URL}"
    }
  }
}`

### Set Proxy for Local:
- add `proxy.conf.json` file to root of project and add the below content. More Information here [https://github.com/angular/angular-cli/blob/master/docs/documentation/stories/proxy.md]
`{
  "/api": {
    "target": <BACKEND-SERVER-TARGET>,
    "secure": false,
    "logLevel": "debug",
    "changeOrigin": true
  }
}`
- change `start` in `scripts` section of `package.json` or add new script as below value.
`ng serve --proxy-config proxy.conf.json`


### Commands at hand
  - `npm install` : To install client side dependencies
  - `npm start`: To Run application
  - `npm run build` To Build Static Assests

### References:
- https://juristr.com/blog/2016/11/configure-proxy-api-angular-cli/

- https://www.codewithjason.com/deploy-angular-cli-webpack-project-heroku/

- https://paucls.wordpress.com/2016/11/25/deploy-angular-2-cli-app-to-heroku/

- https://m.alphasights.com/using-nginx-on-heroku-to-serve-single-page-apps-and-avoid-cors-5d013b171a45

- https://devcenter.heroku.com/articles/using-multiple-buildpacks-for-an-app

- https://github.com/angular/angular-cli/blob/master/docs/documentation/stories/proxy.md

- https://angular.io/guide/quickstart
