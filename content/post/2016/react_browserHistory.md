+++
date = "2016-09-14T10:15:51+02:00"
tags = ["React", "hashHistory", "browserHistory", "javascript"]
title = "React: switching from hashHistory to browserHistory"

+++

Since I have been working a lot with Angular, hash history is a kind of obvious way of handling routes in a single page application. It's quite common to see tutoarials/examples where URL looks like this:

`http://example.com/#/about` 

Sure, you can set `html5Mode` in `$lcoationProvider`, but it's not set by default, moreover HTML5 History API which is then activated is not supported by all browsers (although all modern does).


I was a little bit surprised at first, when I read that [among other things](https://ponyfoo.com/articles/stop-breaking-the-web) it's actually not the best idea to use hashes at all and since it was designed for a different purpose.

React by default encourage to use `browserHistory` although there are still many tutorials/examples which use hashHistory. Why ? It's easier in terms of configuration, because you don't need to care about any server settings - it just works on any modern browser.

### OK, how to switch to browserHistory

All you need to do when switching to browserHistory is to tell your server (node.js in my case) to handle every route with index.html (entry point html for app), to avoid situation when accessing eg. `/users/123` server redirects you to that location (and most often throw 404).

```javascript
app.get('*', function (request, response) {
  response.sendFile(path.resolve(__dirname, 'public', 'index.html'))
});
```

Second, very important thing is to make all your paths in `index.html` to be absolute, so instead:
```
<script src="dist/bundle.js"></script>
```
prefix your path with "`/`" mark:
```
<script src="/dist/bundle.js"></script>
```
This allows you to avoid strange error like `Unexpected token < error`.

And now we're ready to use it in React just like that:

```javascript
import {render} from 'react-dom';
import { Router, Route, browserHistory } from 'react-router'
import MainView from './app/mainView';
import SettingsView from './settings/settingsView';
import UserView from './settings/user/userView';
import UserListView from './settings/userList/userListView';

render(<Router history={browserHistory}>
      <Route path="/" component={MainView}/>
      <Route path="/settings" component={SettingsView}/>
      <Route path="/settings/user" component={UserListView}/>
      <Route path="/settings/user/new" component={UserView}/>
    </Router>),

    document.getElementById('app')
```


What differentiate React from Angular is that former one doesn't fall back to hash history if your browser doesn't support `windows.history` API. 
