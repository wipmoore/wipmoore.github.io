---
layout: post
title:  "Frontend development project bootstrap notes"
date:   2016-12-30 10:00:00 +0100
categories: jekyll update
---

# Webpack and Babel

## Webpack

Is a module bundler, it takes modules with dependencies and emits static assets representing those modules.  It works with more than just JavaScript but for JavaScript it uses CommonJs.  It is a reactions against the amount of effort and bolierplate that is needed with tools like *Gulp* and *Grunt*.  It also supports transformation such as Babel where before bundling it will run the module through another process such as babel.

## Babel 

Allows you to take advantage of the latest JavaScript language features ( _ES2015_ ) and then make that code ES5 compatible.  This lets you use the lates language features and ( _mainly_ ) not have to worry about browser compatability.  It uses a mix of transpiling ( converting ) and pollyfilling ( adding the missing language features ). 

# Bootstrapping a project

To bootstrap a project that is going to use _Webpack_ and _Babel_ there are the following steps:

1. Install webpack
2. Initialise the project
3. Install the dependencies
4. Configure Webpack


## Install webpack

Install using npm as a global dependency.

```
npm i webpack -g
```

## Initialise the project

* create the root folder
* create the project folder structure under the root

```
 root
  + src
     + app.js // entry point for the application
  + index.html
```

_app.js_

```
console.log( 'app loaded' );
```

_index.html_

```
<!DOCTYPE html>
<html>

    <head>
        <meta charset="UTF-8">
        <title>bootstrap</title>
    </head>

    <body>
        <script src='app.js' ></script>
    </body>

</html>
```


* initialise the project using npm, in the root folder:

```
  npm init -y
```

## Install the dependencies

in the root folder

```
npm i babel-polyfill babel-runtime --save
npm i webpack-dev-server babel-core babel-loader babel-preset-es2015 babel-plugin-transform-runtime --save-dev
```

* *babel-core* is the babel npm package
* *babel-loader* is the babel module loader for Webpack
* *babel-preset-es2015* is a babel preset for all es2015 plugins.
* *babel-plugin-transform-runtime* Babel bakes a number of smaller helpers directly into your compiled code. This is OK for single files, but when bundling with Webpack, repeated code will result in a heavier file size. It is possible to replace these helpers with calls to the babel-runtime package by adding the transform-runtime plugin. 


## Configure Webpack

Create the webpack configuration file *webpack.config.js* in the root folder.

```
var path = require('path');

module.exports = {
  entry: [
    'babel-polyfill', // use babel polyfil to set up an es6 environment
    './src/app.js'
  ],
  output: {
      publicPath: '/', 
      filename: 'app.js'
  },
  module: {
    loaders: [
      {
        test: /\.js$/,  // files that should be loaded
        include: path.join(__dirname, 'src'),  // path to load files from
        loader: 'babel-loader',
        query: {
          plugins: [ 'transform-runtime'],
          presets: [ 'es2015' ],  
        }
      }
    ]
  }  
}
```

## Webpack-dev-server

To run the development webserver use the following command from the root folder:

```
webpack-dev-server --inline 
```

The development server will listen on _http://localhost:8080_ and the command invokes the server with hotloading so if any changes are made to the javascipt the site is reloaded to relect them.


# References

* https://medium.com/@dabit3/beginner-s-guide-to-webpack-b1f1a3638460
* http://jamesknelson.com/using-es6-in-the-browser-with-babel-6-and-webpack/
* https://medium.com/@jcse/clearing-up-the-babel-6-ecosystem-c7678a314bf3#.jjvg4m5ly
* https://babeljs.io/