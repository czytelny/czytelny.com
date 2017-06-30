+++
title = "Vue.js and Firebase - first steps"
draft = false
date = "2017-06-30T20:43:02+01:00"
tags = ["vuejs", "vue", "vue.js", "firebase"]
+++

Few days ago I came back from awesome conference [vueconf](https://conf.vuejs.org) - 
the first Official Vue.js Conference in the world. Regarding whole Vue.js framework I'm going to 
write separate post because it's awesome, however right now something different drew my attention - firebase.
 
Firebase is a Google's product *"that helps you quickly develop high-quality apps and grow your business"*.
In fact it's a whole platform which offers realtime databse, analytics, cloud messaging, authentication and 
a other goodies. For me, pretty interesting concept is realtime database (accomplished with websockets) 
which allows to write fully functional CRUD application without any backend burden O_O 
What it means is that actually you don't need any backend at all. 
As an enthusiast of front-end it sounds appealing :)

Getting back to Vue - it offers extremely simple and intuitive integration with firebase. 
I'm going to quickly show how I build my simple Firebase-based application [available on github](https://github.com/czytelny/vuelendar). 
I'll skip boring things like initializing vue app with vue-cli, or setup firebase account (all you need is Google account).
  
### Setup dependencies

First step is, of course, to add `firebase` and `vuefire` dependency to your project, so:

```
npm install firebase --save
npm install vuefire --save
```
or with yarn:
```
yarn add firebase
yarn add vuefire
```

so that you `package.json` looks like (versions may vary):

```
    "dependencies": {
      "firebase": "^4.1.3",
      "vue": "^2.3.3",
      "vuefire": "^1.4.3"
    },
```

### Setup VueFire
It's really trivial. In main vue file (it's named `main.js` in my project) import and attach vuefire to Vue instance. 
It gives a possibility to use data from Firebase as local bindings - very useful thing.
 
 ```javascript
 import Vue from 'vue'
 import VueFire from 'vuefire'
 import App from './App'

Vue.use(VueFire)

new Vue({
  el: '#app',
  template: '<App/>',
  components: { App }
})
```

### Setup Firebase
Actually it's also very simple. I divided configuration to two modules: 


`firebaseConfig.js` 
```javascript
// You can get this data from Firebase console after creating a project:
// Go to: Overview->Add Firebase to your webapp.
export default {
  apiKey: '--- copied from firebase console ---',
  authDomain: '--- copied from firebase console ---',
  databaseURL: '--- copied from firebase console ---',
  projectId: '--- copied from firebase console ---',
  storageBucket: '--- copied from firebase console ---',
  messagingSenderId: '--- copied from firebase console ---'
}
```
Some part of that configuration is not required right now, 
but it's better to keep for the future ;) 


`firebaseInit.js`
```javascript
import Firebase from 'firebase/app'
import firebaseConfig from './firebaseConfig'
/* eslint-disable no-unused-vars */
import firebaseDatabase from 'firebase/database'

const firebaseApp = Firebase.initializeApp(firebaseConfig)

export default firebaseApp.database()
```

What's worth to notice in `firebaseInit.js` is that I'm importing only `app` from firebase module
and then separately `firebase/database`. Why ? I'm using only realtime database, so I don't need other modules 
(like `firebase-auth`, `firebase-messaging`, `firebase-storage`). 
It allows me to limit final bundle size by ~200kb.

### Your vue.js app

Now, the most important part. In one of your stateful component: 

```javascript
import db from './firebaseInit'

const eventsRef = db.ref('events')

export default {
    name: 'statefulComponentName',
    data () {
      return {
        inputVal: null        
      }
    },
    firebase: {
      events: eventsRef 
    },
    methods: {
      saveEvent (eventName) {
        db.ref('events/').push({
          name: eventName
        })
      },
      removeEvent (eventId) {
        db.ref(`events/${eventId}`).remove()
      },
      eventSubmit () {
         this.saveEvent(this.inputVal);
         this.inputVal = null;
      }
    }
  }
```

and in a template

```html
<!-- listing items -->
 <div v-for="event in events" :key="event['.key']">
  {{event.name}}
 </div>
 
 <!-- adding item -->
 <form v-on:submit.prevent="eventSubmit">
     <input placeholder="Event title" v-model="inputVal">
     <button type="submit" @click="eventSubmit">+</el-button>
   </form>
```


### Wrap up
And that's basically it. Every time user add new item through input, it's automatically synchronized with all clients.
Pretty amazing how trivial it was. One more thing to note is that by default fireabse project require authentication. 
To keep it as simple as possible I setup rules for my project to allow anybody write and read from it:
In your Firebase web console click to: **Database** and then in **Rules** tab: 

```
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

In next step I'm going to tackle integration authentication with vue, 
but I'm positive about it after simplicity of this initial setup :) 





   
   