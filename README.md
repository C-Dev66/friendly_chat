<img src="https://github.com/C-Dev66/friendly_chat/blob/main/screenshots/Preview.png" alt="HomePage"/>

# RSVPGuestBookChatApp
> This multi-platform application(Andoird, iOS, Web) serves the purpose of storing users who will be attending the up coming convention. Once authenticated guests have the option to communicate in a chatroom with all other participants.

---

### Table of Contents:

- [Description](#description)
- [Code Snippets](#code-snippets)
- [Summary](#summary)
- [Demo](#demo)




---

## Description

Application is built with Google's Flutter front-end as well their native back-end service Firebase.

- **Firebase Authentication** to allow your users to sign in to the app.
- **Cloud Firestore** to save structured data on the cloud and get istant notification when data changes.
- **Firebase Security** Rules to secure the database.

Cloud Firestore is a NoSQL database, and data stored in the database is split into collections, documents, fields, and subcollections.

We will store each message of the chat as a document in a top-level collection called guestbook.



<img src="https://github.com/C-Dev66/RSVPGuestbookChatApp/blob/main/screenshots/DataModel.png" alt="HomePage" width="400"/>

---

## Code Snippets

> Setting up the application
```
// Configure dependencies

flutter pub add firebase_core
flutter pub add firebase_auth
flutter pub add cloud_firestore
flutter pub add provider

// Installing flutterfire

dart pub global activiate  flutter_cli

// Configuring your app

flutterfire configure
```

> Adding user sign-in RSVP
```dart
Consumer<ApplicationState>(
	builder: (context, appState, _) => Authentication(
		email: appState.email,
        loginState: appState.loginState,
        startLoginFlow: appState.startLoginFlow,
        verifyEmail: appState.verifyEmail,
        signInWithEmailAndPassword: appState.signInWithEmailAndPassword,
        cancelRegistration: appState.cancelRegistration,
        registerAccount: appState.registerAccount,
        signOut: appState.signOut,
    ),
),

```

> Writing Messages to Cloud Firestore
```dart
import 'dart:async';
import 'package:cloud_firestore/cloud_firestore.dart';

Future<DocumentReference> addMessageToGuestBook(String message) {
	if (_loginState != ApplicationLoginState.loggedIn) {
      throw Exception('Must be logged in');
    }

    return FirebaseFirestore.instance
        .collection('guestbook')
        .add(<String, dynamic>{
      'text': message,
      'timestamp': DateTime.now().millisecondsSinceEpoch,
      'name': FirebaseAuth.instance.currentUser!.displayName,
      'userId': FirebaseAuth.instance.currentUser!.uid,
    });
}

Consumer<ApplicationState>(
     builder: (context, appState, _) => Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
            if (appState.loginState == ApplicationLoginState.loggedIn) ...[
                const Header('Discussion'),
                GuestBook(
                    addMessage: (message) =>
                        appState.addMessageToGuestBook(message),
                  ),
            ],
        ],
    ),
),
```

> Adding security & validation rules cloud.firestore
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /guestbook/{entry} {
      allow read: if request.auth.uid != null;
      allow write:
      if request.auth.uid == request.resource.data.userId
          && "name" in request.resource.data
          && "text" in request.resource.data
          && "timestamp" in request.resource.data;
    }
  }
}
```

---

## Summary

Awesome introduction to Firebase, Cloud Firestore, & Security Rules. Really enjoyed this project and will seek to build more applications with their native database service. All around great experience, the future is bright for Flutter. Glad to be able to partaken in this revolutinary stack. 🤩🫶

For more information refer to the official documentation.

- [Firebase Documentation](https://firebase.google.com/docs)
- [Flutterfire](https://firebase.google.com/docs/flutter/setup?platform=ios)
- [Google's awesome Flutter Youtube channel, Lots of great content here](https://www.youtube.com/channel/UCwXdFgeE9KYzlDdR7TG9cMw)

---

## Demo
![HomePage Gif](https://github.com/C-Dev66/friendly_chat/blob/main/screenshots/FriendlyChatDemo.gif)

