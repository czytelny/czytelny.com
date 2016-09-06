+++
date = "2016-09-06T20:50:24+02:00"
tags = ["react", "socket.io"]
title = "React + socket.io"

+++

After 2 years with Angular I must admit that jumping into React world was truly fun. I'm just starting with it but already appreciate benefits from component-based setup. 

My React project base fully on Websockets so to make it smooth I'm using `socket.io`. After going through some tutorial I found out that asynchronous actions should be taken in `componentDidMount` phase of component life cycle. So my initial component looks like this:

```javascript
var UserListContainer = React.createClass({
  getInitialState() {
    return {
      users: []
    }
  },

  componentDidMount() {
    socketHandler.emit(GET_ALL_USER_REQUEST);
    socketHandler.on(GET_ALL_USER_REQUEST_SUCCESS, (response) => {
    	this.setState({users: response});
    });
  },

  render() {
    return (<UserList users={this.state.users}/>);
  }
});

export default UserListContainer;
``` 

Everything seems to be working fine but I spotted nice (yeah, it's beautiful after staring at Angular's stacktraces) error in console:

> *setState(...): Can only update a mounted or mounting component. This usually means you called setState() on an unmounted component.*

Sounds reasonable. After digging it turned out that I was missing `componentWillUnmount()` component phase function where I should cancel my request. The problem is every example shows simple AJAX request which you can easily abort at any time. I didn't have clue how to handle this with websocket but it turned out to be equally easy, because socket.io provide `removeListener` handler which does exactly what names suggest :) so after tiny tweaks my code was working without any problems:


```javascript
var UserListContainer = React.createClass({
  getInitialState() {
    return {
      users: []
    }
  },

  componentDidMount() {
    socketHandler.emit(GET_ALL_USER_REQUEST);
    socketHandler.on(GET_ALL_USER_REQUEST_SUCCESS, this._getAllUserRequestHandler);
  },

  componentWillUnmount() {
    socketHandler.removeListener(GET_ALL_USER_REQUEST_SUCCESS, this._getAllUserRequestHandler);
  },

  _getAllUserRequestHandler(response) {
    this.setState({users: response});
  },


  render() {
    return (<UserList users={this.state.users}/>);
  }
});

export default UserListContainer;
```


