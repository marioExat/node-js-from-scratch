# Node.js API From Scratch Using TypeScript, Express And MongoDB #1 

[Video Link](https://www.youtube.com/watch?v=1o9YOHeKhNQ&t=885s)

## Setup

1) First Use `cd` command to go to the right folder eg. VS Code Projects and create the project folder and open it using `mkdir <project_name> && cd <project_name` in the terminal.

2) Initalise the node project using the command
`npm init -y`.

3) Add and install the dependencies using hte command.
`npm i -D typescript tsc-watch eslint prettier eslint-config-prettier eslint-plugin-prettier @typescript-eslint/parser @typescript-eslint/eslint-plugin @types/node @types/express`
i - install
-D - dependencies to install
@types/node - add types of node
@types/express - add types to express
eslint - js linter

4) run `npm i express dotenv`
dotenv - environmental files

5) Get config file using `npx tsc  --init`

6) in tsconfig.json, change `base url` to `./src` all our code will be in source folder, in out dir change it to `"dist"`, this is where the vanilla js compiled from typescript will live.
Add paths as
```
{
"@/resources/*" :["resources/*"],
"@/utils/*" : ["utils/*"],
"@/middleware/*" : ["middleware/*"],
}
```
It will create these folders
Controllers and models will live in resources,

Doing the above will help us import these using a call like
`import something from '@/resources'` instead of typing full path


7) Go to package .json and add the below scripts which will be used to run the code

```
"test" : keep it as it is,
"start": "node dist/index.js",
"dev" : "tsc-watch --onSuccess \"node ./dist/index.js"",
"build": "tsc",
"postinstall: "npm run build"
```
start is used to run production code(js code)

To start an app in production we type `npm run start`

Then we add `dev to run locally`, to run in development type `npm run dev` the dev command here will keep and eye on typescript file and if anything changes it will rebuild the project and restart server

postinstall - runs after we do npm install,useful if someone pulls our code from github and when they run npm install it automatically builds the project from them, used in production as well alternatively we can run npm run build manually if you dont want to put it here

8) ESLint and prettier - download plugins for vscode from extensions, add two files in root folder `.eslintrc.js` and `.prettierrc.js`

In Prettier add
```
module.exports = {
    tabWidth :4,
    singleQuote: true,
    semi: true,
};
```
semi - forces you to end a statment with semicolon

In eslint add
```
module.exports = {
    parser: "@typescript-eslint/parser",
    extends: [
        'plugin:@typescript-eslint/recommended',
        'prettier/@typescript-eslint',
        'plugin:prettier/recommended',
        
    ],
    parseOptions : {
        ecmaVersion: 2018,
        sourceType: "module"
    },
    rules: {}, 
};
```

9) Pushing to source control 
- add a .gitignore to root
Add the below to it, these are folders we dont want to push
```
dist
node_modules
.env
```
.env - has all private information like api keys etc

create a file .env.example which is an example of what your env folder should contain, it has
```
NODE_ENV=development
PORT=3000
MONGO_PATH=
MONGO_USER=
MONGO_PASSWORD=
```

Create an actual .env file to hold the information and fill it
```
NODE_ENV=development
PORT=3000
MONGO_PATH=
MONGO_USER=
MONGO_PASSWORD=
```
Two ways to use mongodb
1) Through Docker- docker compose
2) can use normally with atlas and compass

10) Add the module alias package using `npm i module-alias` this is related to the paths we added in TSconfig file. tsconfig applies only in  ts, to package.json we have to add,the below copied from the tsconfig file
```
  "_moduleAliases": {
    "@/resources/*" :["dist/resources/*"],
    "@/utils/*" : ["dist/utils/*"],
    "@/middleware/*" : ["dist/middleware/*"]
  }
```

TS files will use source folder, js will use dist folder

11) Create project structure
In root folder create src using vs code or terminal command `mkdir src && cd src` to enter src
In src, create 3 folders `middleware`, `resources`, `utils` and a file app.ts which is the setup file, to initalise routers, mongodb etc
In src create another file called `index.ts` which will be the entry point of our code.

12) In app.ts import some files
```
import express, { Application } from 'express;
import mongoose from 'mongoose';
import compression from 'compression';
import cors from 'cors';
import morgan from 'morgan';
import Controller from '@/utils/interfaces/controller.interface';
import ErrorMiddleware from '@/middleware/error.middleware;
import helmet from 'helmet';

class App {
    public express: Application;
    public port: number;

    constructor(controllers: Controller[], port: number) {
        this.express = express();
        this.port = port;

        this.initaliseDatabaseConnection();
        this.initialiseMiddleWare();
        this.initaliseControllers(controllers);
        this.initaliseErrorHandling();
    }
}
```
morgan - logging tool useful for getting requests made and errors
helmet - helps prevent common attacks for api's

13) Install everything now
`npm i mongoose compression cors morgan helmet`, these are the normal dependencies

Use the below command for development dependencies
`npm i -D @types/compression @types/cors @types/morgan`

Two ways to export a file in node
```
export defaultinterface Controller {
    path: string,
    router: Router
}
```
or
```
interface Controller {
    path: string,
    router: Router
}

export default Controller;
```
Doing this second approach keeps the code out of the way in big classes

You can click on any function/library in import to see whole api, eg click on Document