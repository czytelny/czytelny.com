+++
date = "2016-05-28T19:42:25+02:00"
tags = []
title = "_.propertyOf - underscore/loadash"

+++

I hadn't known `underscore` nor `loadash` for quite a long time in my developer career. Definitely too long. Simply nobody told me about its existence ;) Nevertheless, I don't want to babble too much about all functions which I use almost every day, but rather focus on one of them: `_.propertyOf`.
 
When I first saw it in API my first thought was: why the heck i should use something like:

```
_.propertyOf(obj)('property');
```

instead of simple `obj.property` or `obj["property"]` ? 


The answer hit me when I was working on function which converts DTO to a business model object. I encountered a problem with property of a object which doesn't exist... Let's take a look in the source code of `underscore` (I guess `loadash` implementation is exactly the same)

```
  _.propertyOf = function(obj) {
    return obj == null ? function(){} : function(key) {
      return obj[key];
    };
  };
```

Let's see what's going on here... It returns empty function if passed `object` is null, or returns function which returns value of a given property named `key`. What's the difference ? 

Consider you have nested objects eg.
```
var complexObj: {
	obj1: {
	   code: 'm'
	},
	obj2: {
	   code: 'c'
	}
}
```

- if you call `complexObj.obj1.code` you obviously receive `m`
- if you call `complexObj.obj1.name` you receive `undefined`
- if you try to call `complexObj.obj3.code` ? 

`
Uncaught TypeError: Cannot read property 'name' of undefined
`

Ooops, `TypeError` and as a result stopped script execution. Why would you try to call property on a not existing property? Sometimes it's just inevitable while handling backend responses. And here jumps in `_.propertyOf`.

Calling 
```
_.propertyOf(complexObj.obj3)('code')
```
results only `undefined`. 

_propertyOf is really tiny, useful function, because you can easily cope with it's result without breaking script execution.
