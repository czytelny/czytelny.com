+++
date = "2015-04-14T20:47:05+01:00"
tags = ["Javascript"]
title = "Callback function with additional argument. Function currying technique"

+++

This note was created because most of examples about currying technique in javascript ​are sooo long and complex that getting ​an ​answer to​ a​ simple question: “How to create a callback function with additional argument” takes way too long:) Of course there are good reasons why they are so long – it’s very​ a​ powerful and not trivial language feature.
So let’s treat t​he content below as an introduction and a basic usage example.

Let’s say you have a regular function which takes two arguments:

```javascript
function renderArticle(data, articleName) {
  (...)
}
```

and you have another function which is responsible for sending an asynchronous GET request, and trigger​s callback when it’s completed with success:

```javascript
var sendGet = function(url, successCallback) {
        var dataRequest = new XMLHttpRequest();
        dataRequest.onreadystatechange = function() {
            if (dataRequest.readyState == 4 && dataRequest.status == 200) {
                var data = JSON.parse(dataRequest.responseText);
                successCallback(data);
            }
        };
        dataRequest.open("get", url, true);
        dataRequest.send();
    };
```

And eventually, it happens that you have​ a​ third function where you want to connect the two above – send a GET requests in a loop, and
after each success, trigger renderArticle function. Just like that:

```javascript
for (var i = 0; i < someList.length;  i++) {
     var currentArticleName = someList[i];
     sendGet(url, renderArticle); //here:how to use renderArticle with currentArticleName?
}
```

The problem is that on each loop iteration you have ​an ​additional variable currentArticleName which would be nice to send somehow to renderArticle function. Of course​ the​ following: ```sendGet(url, renderArticle(currentArticleName));``` doesn’t work, since it’s sending the result of ​an ​exectution – not a reference to the function.

My first gues was to change sendGet to handle ​an ​additional parameter so it can be called like this: ```sendGet(url, renderArticle, currentArticleName);```
But it’s so shamefully clumsy that I thought that there is must be ​a better solution. Of course there is and it is called currying.

At the beginning I had a problem to wrap my head around ​the ​concept which lies behind this. The idea is to pre-set articleName variable at a time of iterating through​ the​ loop, and reuse it when callback function execution will be triggered in the future.
Let’s take a look ​at an actual code.
We need to create ​an ​additional function which serves as a kind of wrapper for our callback:

```javascript
var curriedRenderArticle = function(articleName) {
                return function(dataToRender) {
                    renderSingleArticle(dataToRender, articleName)
                }
            };
```

**That’s it.**

Now you can use it like this:

```javascript
for (var i = 0; i < someList.length;  i++) {
     var currentArticleName= someList[i];
     sendGet(url, curriedRenderArticle(currentArticleName)); 
} 
```

What’s happening here: when ​the ​above line is processed, curriedRenderArticle function is executed with ​the ​argument articleName and it returns a new anonymous function. This new function is kindly waiting for GET request to be proceed​ed​ (in sendGet) and then is triggered as a callback with dataToRender argument. What’s most important – thanks to closure the value of articleName is remembered so when eventually renderSingleArticle(dataToRender, articleName) is processed – it has all necessary data.