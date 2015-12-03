+++
date = "2015-10-29T20:47:32+01:00"
tags = ["Javascript", "Angular", "AngularJs"]
title = "Dynamic adding controllers, services to an Angular app"

+++

Angular enforces to declare all controllers, services, directives etc. before application is bootstrapped.
So typically you:

- include script in the head section
- bootstrap your app via: ng-app="myApp"

But there are situations when you may need more flexibility.
For example big webapps with number of modules which visibility need to be limited due to security reasons. Or because of big codebase you may need to avoid unnecessary network traffic.

There is no official way to load files with angular source code and then register it eg. as a controller. BUT thanks to the excellent article written by Ben Nadel ([link](http://www.bennadel.com/blog/2553-loading-angularjs-components-after-your-application-has-been-bootstrapped.htm)) I managed to achieve this very easily.

First step is to overwrite functions responsible for creating new components, what we want to do in a config phase, when you have access to providers.

```javascript
var app = angular.module('myApp', []);
app.config(['$provide', '$controllerProvider', function($provide, $controllerProvider) {
    app.controller = function(name, constructor) {
        $controllerProvider.register(name, constructor);
        return this;
    };
 
    app.factory = function(name, factory) {
        $provide.factory(name, factory);
        return this;
    };
}]);
```

Second step is to load JS source file. For that purpose I use an excellent library [angular-load](https://github.com/urish/angular-load).

```javascript
angularLoad.loadScript('/js/additional/extra.js').then(function() {
    // Script loaded succesfully.
})
```

And now if your recently loaded script contains regular angular init line, like

```javascript
angular.module('myApp').controller(function() {
   //anything
});
```

It will be available all over the myApp through regular dependency injection mechanism.

Bare in mind that loading script is asynchronous, so before you can actually use your dynamic component you have to make sure that its successfully loaded.
I handled it by using ng-if directive on a view and a flag for controllers. For services simple boolean is enough.