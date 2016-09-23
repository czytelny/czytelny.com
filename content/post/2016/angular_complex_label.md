+++
date = "2016-09-23T20:21:57+02:00"
tags = ["angular", "ngOptions", "ng-options"]
title = "Angular complex label for ng-options"

+++

Using `ng-options` is always somewhat cumbersome for me. When I have it written it's perfectly fine, but every time I need to write it from scratch I have to take a look to docs, to recall syntax.  
Basic usage with array which you can see everywhere:

```javascript
ng-options="item.name for item in arrayOfItems"
```

I was wondering : how to get somewhat more complex label? Combintation of item fields like for example `item.name` with `item.surname` 

Assume item is defined as:

```
{
	name: "Michal",
	surname: "Kowalsky"
}
```

First trials with spaces and brackets failed, but eventually I find out quite obvious solution with a `+` mark:

```javascript
ng-options="item.name+' - '+item.surname for item in arrayOfItems"
```

results in `Michal - Kowalsky` label. That's how we can define pretty complex labels composed of many item fields if we need.