+++
date = "2015-12-22T22:25:25+01:00"
tags = ["AngularJS", "Javascript", "ng-annotate"]
title = "ng-annotate - it's worth to know it"

+++
Dependency Injection in AngularJS is one of the first thing you learn
in order to effectively use this framework. It's nice and easy -
you pass a service name which you want to inject as an argument to a function,
like for example `$scope`:

```Javascript
angular.module("MyApp").controller("MyCtrl", function($scope) {    
  ///...
});
```

At some point you realize that minification will mess the source and cause an error.
Thus a good practice is to develop a habit to use an array syntax (inline-annotation):

```Javascript
angular.module("MyApp").controller("MyCtrl", ['$scope', function($scope) {    
  ///...
}]);
```

And I have been using it for a long time **but...**

### It's not perfect
This kind of syntax is fine when you are passing one, two or even three services.
But when you are injecting 15 of them? It's quite common in large webapps.
Then the chaos begins: you move some of them (replace, or change order) and forget to track and update this filthy strings.
I can't count how many times I spend some ridiculous amount of time trying to figure out "why the heck it doesn't work?!".
And here comes:

### ng-annotate
This tiny beast allows you to skip array notation which I mentioned above.
Since I'm using gulp in my project I used `gulp-ng-annotate` plugin.
After including it in my build step **before** uglify phase:
```
     //(...)
     .pipe(ngAnnotate())
     .pipe(uglify())
     //(...)
```
 I can now:


- use magic string `"ngInject";` in functions:

```Javascript
angular.module("MyApp").controller("MyCtrl", function($scope) {    
    "ngInject";

  ///...
});
```

- in ES6 classes annotate constructor with `/*ngInject*/``

```Javascript
export default class MyCtrl {
    /*@ngInject*/
     constructor(MyService) {
        this.MyService = MyService;
     }
  }
```

And the magic happens - after minification of the source code it still works.

### Angular 1.x & ES6
Using ng-annotate is extra useful if you are writing Angular app with ES6.
In fact array notation is not available at all for classes thus you have to manually use `$inject` service:

```Javascript
MyController.$inject('MyService');
```

It's not convenient so in my opinion **ng-annotate** is definitely a winner here.
