+++
date = "2014-12-01T20:46:18+01:00"
tags = ["Javascript", "Angular", "AngularJs", "Unit test"]
title = "Jasmine and Karma – unit testing angular directives"

+++

I’m using Jasmine & Karma tests runner to make unit tests for my angular app. Although there are many examples of how to write them for directives, it turns out that I encounter unexpected issues.
Quick glimpse on first of them with solution:

If your directive has “in code” template, then you don’t care – you can go ahead and test it normally. But if you have an external template url – you have a big problem. Trivial example:

```javascript
"use strict";
angular.module('AngularApp').directive('navigationBar', function() {
    return {
        restrict: 'E',
        replace: true,
        templateUrl: 'js/navigation/navigation-bar.html'
    };
});
```
As far as I know, there is no nice “official” way to cope with the above code. Trying to test it, you’ll receive:

> Error: Unexpected request: GET js/navigation/navigation-bar.html 
> No more request expected

(let me mention that `$httpBackend.expectGET(‘/js/navigation/navigation-bar.html’).respond(…)`
doesn’t work)

You simply can’t test it (or maybe you can with some tremendous effort?) Fortunately, somebody made a plugin for Karma which allows you to handle such a case: https://github.com/karma-runner/karma-ng-html2js-preprocessor.
What does it do? Compiles AngularJS templates to JavaScript on the fly.

> *This preprocessor converts HTML files into JS strings and generates Angular modules. These modules, when loaded, put these HTML files into the $templateCache and therefore Angular won’t try to fetch them from the server.*

Of course, I had a problem to make it working ;) so eventually, I ended up with the simplest possible setup. I added to **karma.conf.js:**

```javascript
ngHtml2JsPreprocessor: {
     // setting this option will create only a single module that contains templates
     // from all the files, so you can load them all with module('templates')
     moduleName: 'templates'
 }
```
and in preprocessors section:

```javascript
preprocessors: {
     'js/navigation/*.html': ['ng-html2js']
 }
```

and then eventually in my actual unit test file:

```javascript
beforeEach(module('templates'));
```

and that’s it. I can eventually write a standard unit test part:

```javascript
var element = angular.element("<navigation-bar></navigation-bar>");
$compile(element)($rootScope);
$rootScope.$digest();
expect(....).toContain(...);
```