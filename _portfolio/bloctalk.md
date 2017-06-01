---
layout: post
title: BlocChat
feature-img: "img/sample_feature_img.png"
thumbnail-path: "https://d13yacurqjgara.cloudfront.net/users/3217/screenshots/2030974/bloctalk_1x.png"
short-description: BlocChat messenger to talk with friends!

---
{:.center}
# BlocChat: Case Study

### Summary

I created blocChat as my second angularJS app, it is a real-time messenger that allows users to talk with friends and piers. The app uses room tabs to navigate to your desired chat room.

### Explanation

This app was created by myself with the help from a bloc font-end starter kit. I used html, css, angularJS and firebase to launch this simplified chat app. It was built to learn the concepts of firebase, in order to help with backend server data and authentication.

### Problem

The initial problem was finding an easy way to collect client side data and save it on the backend. I decided to use angular firebase which is a backend service that provides data storage, file storage, authentication, and static website hosting for your Angular app. This allowed me to use firebase shortcuts in order to log and show data through client side interaction.

### Solution

First I needed to set up the firebase project and link it to my html, as seen below

```
<script src="https://cdn.firebase.com/libs/angularfire/2.3.0/angularfire.min.js"></script>
    <script>
       // Initialize Firebase
       var config = {
         apiKey: "AIzaSyDIzPbacpcv0_q_CX63aceWtmPLHZwf9i0",
         authDomain: "blocmessenger.firebaseapp.com",
         databaseURL: "https://blocmessenger.firebaseio.com",
         projectId: "blocmessenger",
         storageBucket: "blocmessenger.appspot.com",
         messagingSenderId: "467894678082"
       };
       firebase.initializeApp(config);
     </script>
```

Once the firebase was linked, I was able to create services and add data, that stored my chat rooms, messages, and user accounts.

Example room controller:
```
(function() {
  function Room($firebaseArray) {
    var Room = {};
    var ref = firebase.database().ref().child("rooms");
    var rooms = $firebaseArray(ref);


    Room.all = rooms;

    Room.add = function (room) {
      rooms.$add(room);
    };
    console.log(rooms);

    return Room;
  }

  angular
    .module('blocMessenger')
    .factory('Room', ['$firebaseArray', Room]);
})();
```
 The rooms were then saved in the firebase data directory

 ![]({{ site.baseurl }}/img/firebasedata.png)

 After the rooms and messages were created, the project started to take form with some minor css help for organization and display

 ![]({{ site.baseurl }}/img/blocchat.png)

 The final step required me to use firebase authentication to sign up and login using the email/password auth controller. First I made a sign in template and auth controller in which I had to inject the firebaseAuth dependency and connected the front-end data inputs with firebaseAuth built in functions. The below code allowed me to create a user, login as a user, and sign out.

 ```
 (function() {
    function Auth($firebaseAuth) {

      var Auth = {};
      var user = firebase.auth().currentUser;

      Auth.authObj = $firebaseAuth();
      console.log(Auth.authObj);


      Auth.createUser = function(email, password) {
        firebase.auth().createUserWithEmailAndPassword(email, password).catch(function(error) {
        console.log(email, password);
        // Handle Errors here.
        var errorCode = error.code;
        var errorMessage = error.message;
        // ...
      }).then(function(){
        firebase.auth().signInWithEmailAndPassword(email, password);
        // console.log(promiseObj.password,'promise password')
        // console.log(promiseObj.email,'email');
        // Auth.signIn(modal.email
        // console.log(promiseObj.email,'email');, modal.password);
      });
    }

      Auth.signIn = function(email, password) {
        firebase.auth().signInWithEmailAndPassword(email, password).catch(function(error) {
        // Handle Errors here.
        var errorCode = error.code;
        var errorMessage = error.message;
        // ...
        console.log(email, password);
        console.log("You are logged in:", firebase.auth.uid);
      });
    }

      Auth.signOut = function() {
        firebase.auth().signOut().then(function() {
        // Sign-out successful.
        alert("You are signed out")

      }).catch(function(error) {
        // An error happened.
      });
    }

    return Auth;
  }

    angular
        .module('blocMessenger')
        .factory('Auth', ['$firebaseAuth', Auth]);
})();
```

Sign in template:

![]({{ site.baseurl }}/img/signintemplate.png)



### Results

In the end, I was able to use firebase as my backend service in order to use and store data in real time.

### Conclusion

In conclusion, I learned how to implement angularJS and firebase to create a fully functional chat app that can authenticate a user and talk with friends from anywhere.
