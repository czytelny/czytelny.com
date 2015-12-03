+++
date = "2014-04-14T20:45:34+01:00"
tags = ["Javascript", "JQuery"]
title = "JQuery data and attr shoudnt be used interchangeably"

+++

It’s just a side note to remember.
JQuery data() function is a convenient way to operate on a data attribute of a DOM object. However if you are going to perform some operations on that attribute it’s important to note that you can’t use data() and attr() interchangeably, although it seems they are doing the same thing.

Consider the following DOM object:

```
<div id="books" data-reference="123"></div>
```

Reading data-reference can be performed both by:

```
var dataR = $("#books").data('reference');
```
and

```
var dataAR=$("#books").attr('data-reference');
```

Both dataR and dataAR have value “123”.

But what if I set data-reference attribute to other value by:

```
$("#books").data('reference', '456');
```

Then mentioned previously dataR and dataAR have different values!
`data()` will return “456” while `attr()` will return “123”.

One more thing to mention is that when setting attributes by data() you won’t see any changes in DOM (for example through Firebug). It’s set in memory.

