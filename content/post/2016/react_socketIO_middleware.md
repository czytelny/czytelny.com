+++
date = "2016-10-15T09:44:59+02:00"
tags = ["Redux", "React", "Socket.io", "Middleware"]
title = "React + Redux + Socket.io"
draft = false

+++

In one of my [previous post](http://czytelny.com/post/2016/react_socketIO/) I presented a simple (whatsoever naive) attempt to integrate Socket.io with React. It is based on assumption that handling websocket events is coupled tightly with React component - on `componentDidMount` lifecycle phase. Moreover state of that particular component is intrinsically nested. Although this approach is really fine while learning, or crafting some simple small webapp, it's actually not the best fit for something bigger. There are tons of valuable resources which explains why this approach won't scale, is difficult to test, and so on.

Fortunately, smart guys devise smart things all the time. It turned out that there is a solution for a problem of coupling a state of app with React component. It's architecture (or a pattern) rather than a library (although there is accompanying tiny lib to include). Most recognizable of them are Flux and Redux. In short they allow to consistently manage state of our app in predictable way. 

Let's get back to actual subject of this post **Socket.io**. Initially,  when I started using Redux I didn't realize that handling Socket.io event should be anyhow binded with it. But really quickly it become vivid that having two 'kind' of actions (websocket events and redux actions) flying all over, is a complete nonsense. So I crawl through tutorials and github repos in a search for perfect [middleware](http://redux.js.org/docs/advanced/Middleware.html) for Redux which I can seamless integrate with socket.io... I found number of cool stuff but non of them fulfill my requirement, so I decided to write my own  [https://github.com/czytelny/redux-socket.io-middleware](https://github.com/czytelny/redux-socket.io-middleware).

It's really simple piece of code which purpose can be summarized as follows: 

## Client -> Backend flow
Whenever you want to send an socket.io event from Client to Backend, use store.dispatch with regular action with meta object with remote field, eg.:

*SomeReactComponentContainer.js*
```javascript
const mapDispatchToProps = function (dispatch) {
  return {
    addUserRequest: (user) => dispatch({
    		type: "ADD_USER_REQUEST",
    		meta: {remote: true},
    		user
  		})
    }
}
```

Middleware will send it to a backend (node.js with express in below example), where all you need to do is to listen on `action` event (you can customize this string, see [github readme](https://github.com/czytelny/redux-socket.io-middleware/blob/master/README.md)), and then like in reducer, switch by action type: 

```javascript
const socketIO = require('socket.io');
const http = require('http');
const app = express();

const server = http.createServer(app);
const io = socketIO.listen(server);

io.on('connection', function (socket) {
	socket.on("action", function (action) {
      	switch (action.type) {
        	case "ADD_USER_REQUEST":
        	//do something
    	}
	});
}
```

## Backend -> Client flow
Whenever you want to send an socket.io event from your Backend to Client, use `io.emit` with standard Redux action. 

```javascript
io.on('connection', function (socket) {
	io.emit('action', {
		type: "ADD_USER_REQUEST_SUCCESS"
	})
}
```

As a result of such emit, middleware on client-side will redirect action object to a `store.dispatch()`, so it will call for you:

```javascript
store.dipstach({
	type: "ADD_USER_REQUEST_SUCCESS"
});
```

It should be handled then nicely by your reducers :)

