+++
date = "2016-10-15T09:44:59+02:00"
tags = ["Redux", "React", "Socket.io", "Middleware"]
title = "React + Redux + Socket.io"
draft = true

+++

In one of my [previous post](http://czytelny.com/post/2016/react_socketIO/) I presented a simple (or rather whatsoever naive) attempt to integrate Socket.io with React. It assumes that handling websocket events is tied directly with React component on `componentDidMount` phase. Moreover state of that particular component is intrinsically nested. Although this approach is really fine while learning, or doing some simple example app, it's actually not the best fit for something bigger. Doing so you would quickly fall into muddy issues with scaling, testing and so on.

Fortunately, smart guys devise smart things all the time, and it turned out that there is a de-facto standard "pattern" for writing bigger web apps with React. It's name is Redux and it allow us to manage state of our app. To me, it's much more *architecture* rather than a library. I don't want to focus here on explaining Redux, because there is a lot awesome stuff across whole web (including original [documentation](http://redux.js.org))

Let's go back to actual subject of this post **Socket.io**. When I first started using Redux I didn't realize that handling socket.io event should be anyhow binded with it. But really quickly it become vivid that having two 'kind' of actions flying all over, is a complete nonsense. Firstly we have `redux action` which is a plain javascript object (or a function if we use `thunk` middleware) and latter - websocket action which has a string passed as a identifier and object as a payload.

So I crawl through tutorials and github repos in a search for perfect [middleware](http://redux.js.org/docs/advanced/Middleware.html) for Redux which I can seamless integrate with socket.io... as a result I write it on my own ;) [https://github.com/czytelny/redux-socket.io-middleware](https://github.com/czytelny/redux-socket.io-middleware).

It's really simple piece of code which purpose can be summarized as follows: 

## Client -> Backend flow
- Whenever you want to send an socket.io event from Client to Backend, use store.dispatch with regular action with meta object, eg.:

*SomeReactComponentContainer.js*
```
const mapDispatchToProps = function (dispatch) {
  return {
    addUserRequest: (user) => dispatch({
    		type: "ADD_USER_REQUEST",
    		meta: {remote: true},
    		user
  		}),
    }
}
```

Middleware will send it to a backend (node.js with express in below example), where all you need to do is to listen on `action` event (you can customize this string, see [github readme](https://github.com/czytelny/redux-socket.io-middleware/blob/master/README.md)): 

```
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
- Whenever you want to send an socket.io event from your Backend to Client, use `io.emit` with standard Redux action. 

```
io.on('connection', function (socket) {
	io.emit('action', {
		type: "ADD_USER_REQUEST_SUCCESS"
	})
}
```

As a result of such emit, middleware on client-side will redirect action object to a `store.dispatch()`, so it will call for you:

```
store.dipstach({
	type: "ADD_USER_REQUEST_SUCCESS"
});
```

What you should handle nicely with reducers :)

