+++
date = "2016-09-23T20:21:57+02:00"
tags = ["angular", "ngOptions", "ng-options", "label"]
title = "Complex label for ng-options"

+++

Using `ng-options` is always somewhat cumbersome to me. When I have it written it's perfectly fine, but every time I need to write it from scratch I have to take a look to docs or tutorials, to recall syntax.  
Basic usage example with array, which is most frequently seen is:

```javascript
ng-options="item.name for item in arrayOfItems"
```

How to get somewhat more complex label? Combintation of item fields like for example `item.name` with `item.surname` 

First trials with spaces and brackets failed, but eventually I find out quite obvious solution with a `+` mark:

```javascript
ng-options="item.name+' - '+item.surname for item in arrayOfItems"
```

results in eg. `John - Smith` label. That's how we can define pretty complex labels composed of many item fields if we need.