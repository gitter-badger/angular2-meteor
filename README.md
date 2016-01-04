# Meteor-Angular2

[![Join the chat at https://gitter.im/vamsivarikuti/angular2-meteor](https://badges.gitter.im/vamsivarikuti/angular2-meteor.svg)](https://gitter.im/vamsivarikuti/angular2-meteor?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
Angular2 + Meteor integration.

##### Angular2 version: beta-0.

## Table of Contents
* [Tutorial](../../#tutorial)
* [Quick Start](../../#quick-start)
    * [Install package](../../#install-package)
    * [Import Angular2 into your app](../../#import-angular2-into-your-app)
    * [Start using Meteor in your angular2 app](../../#start-using-meteor-in-your-angular2-app)
* [Demos](../../#demos)
* [Server Side](../../#server-side)
* [TypeScript Support](../../#typescript-support)
* [Contribution](../../#contribution)

## Change Log

* v0.3.2: Angular 2 upgraded to the first beta version. 
* v0.3.0: Angular 2 seems to be on the finish line to a beta version. In the latest releases (alpha-50 and up) there appeared some **breaking** changes:
 `angular2/angular2` namespace were divided into some new ones, mainly, `angular2/core`, `angular2/common`; also the dash symbol were removed in the selectors of the main directives, i.e., instead of `ng-if` you should use `ngIf` now. For more information about the latest API, please, refer to the official [docs](https://angular.io/docs/ts/latest/api). To get new definition files, you'll need to remove `typings\angular2` folder and `typings\angular2.d.ts` file.

## Tutorial

You can check out a 10-step Angular2+Meteor beginners [tutorial](http://www.angular-meteor.com/tutorials/socially/angular2).

If you are already familiar with Meteor and Angular 1, proceed to the Quick Start.

## Quick Start

### Install package:
````
    meteor add urigo:angular2-meteor
````

This package adds own HTML processor, so you'll also need to remove the standard HTML processor:

````
   meteor remove blaze-html-templates
````

For the explanation, please, read "HTML" [paragraph](http://www.angular-meteor.com/tutorials/socially/angular2/bootstrapping) in the above mentioned tutorial.

### Import Angular2 into your app:
Package supports TypeScript and Babel (.jsx file extension) as languages for development with Angular2.

ES6 modules are supported via SystemJS module loader library.

To start, create `app.ts` file, import `Component` and `View` and then bootstrap the app:
````ts
    import {Component, View} from 'angular2/core';
    import {bootstrap} from 'angular2/bootstrap';

    @Component({
      selector: 'socially'
    })
    @View({
      template: "<p>Hello World!</p>"
    })
    class Socially {}

    bootstrap(Socially);
````

Add `index.html` file to the app top folder:
````html
    <body>
       <socially></socially>
    </body>
````
At this point you should see app working and showing "Hello word".

If you have an HTML file in the app root folder with `body` or `head` tag (`index.html` in our case), the package will recognize it as your master HTML file and will skip inserting a default layout. Otherwise, it'll insert bootstrap HTML as follows:
````html
<body>
    <app></app>
</body>
````
Also, if you name your main client component `app.ts`, the package will import `client/app` System.js module for you.

You can name the component whatever you want, but when doing so you'll need to import it manually by adding the next script in the `index.html` or some JS file (say, `main.js`):
````js
Meteor.startup(() => {
    System.import('client/foo');
});
````

### Start using Meteor in your Angular2 app:
This package comes with some modules that makes it easy to use Meteor in Angular2.
To load that modules, you will need to bootsrap your app with the help of the package's bootstraper.

To do that, import `bootstrap` from Meteor-Angular2 package and remove previous one imported from `angular2/angular2`:

````ts
    import {bootstrap} from 'angular2-meteor';

    ....

    bootstrap(Socially);
````

After that, you can use Meteor collections in the same way as you would do in a regular Meteor app with Blaze.

For example, change `client/app.ts` to:
````ts
    ....

    @View({
      templateUrl: 'client/parties.html'
    })
    class Socially {
        constructor() {
          this.parties = Parties.find();
        }
    }

    ....
````

Add Angular2 template file `client/parties.html` with a content as follows:
````html
    <div *ngFor="#party of parties">
      <p>Name: {{party.name}}</p>
    </div>
````

At this moment, you are ready to create awesome apps backed by the power of Angular2 and Meteor!

## Demos

Check out two demos for the quick how-to:

* the Tutorial's [Socially app](https://github.com/Urigo/meteor-angular2.0-socially)
* [Todos](https://github.com/Urigo/Meteor-Angular2/tree/master/examples/todos) demo (deployed http://ng2-todos.meteor.com).

### Server Side
You can import TypeScript and System.js on the server side same way you can on the client side.

Similar to the client's main module `app.ts`, the package checks for the existence of the `main.ts` file in the server folder and, in case of success, will import it for you. Otherwise, you can name main module as you want and import by:
````ts
Meteor.startup(() => {
    System.import('server/foo').await();
))
````

### TypeScript Support
The package uses this TypeScript [compilers](https://github.com/barbatus/ts-compilers) to compile `.ts`-files. Please, read there how you can configure TypeScript, what options are available or how you can speed up just-in-time compilation.

By default, compiler will curse on syntactic errors and will log out all missing modules and other semantic errors to the terminal.
So, if you have code like this:
````ts
    var parties = new Mongo.Collection('parties');
````
It will likely curse that `Mongo` is undefined. Luckily, the package adds Angular2 and Meteor declaration files, which means you'll need only to reference them in your TypeScript files to fix errors.

After the first run of your app, Angular2-Meteor will create declaration (or typings) files (one of them is `typings/angular2-meteor.d.ts`) in the "typings" folder. Add references to `typings/angular2-meteor.d.ts` in every TypeScript file that uses Meteor or Angular2 API as follows:
````ts
/// <reference path="../typings/angular2-meteor.d.ts" />
````
Make sure that paths are relative to the app top folder.

The package installs typings itself but doesn't overrides existing ones in the "typings" folder. So, if you've 
updated package and started getting errors in the console, remove "angular2" folder and "angular2-meteor.d.ts" and re-start the app. New versions of them will be re-installed in the folder.

## Contribution
If you know how to make integration of Angular 2 and Meteor better, you are welcome!

For the coding style guide, we use AirBnB [rules](https://github.com/airbnb/javascript) with TypeScript specifics and max line width set to 100 symbols. Rules are partly enforced by the tslint.json file in the root (if you are not familiar with TSLint, read more [here](https://github.com/palantir/tslint)). Please, check that your code conforms with the rules before PR.


