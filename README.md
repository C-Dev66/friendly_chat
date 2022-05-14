<img src="https://github.com/C-Dev66/friendly_chat/blob/main/screenshots/Preview.png" alt="HomePage"/>

# friendly_chat
> This multi-platform application(Andoird, iOS, Web) creates a chat instance where the user can send messages into a cache. Updating the page in real time with swift animations provided by flutter depencies.

---

### Table of Contents:

- [Description](#description)
- [Code Snippets](#code-snippets)
- [Summary](#summary)
- [Demo](#demo)




---

## Description

Application is built with Google's Flutter written in dart lang.

We will be doing the following:

- Building the main user interface 
- Adding a UI for composing & displaying messages
- Animating the final application


---

## Code Snippets

> Building the main user interface
```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';

  @override
  void dispose() {
    for (var message in _messages) {
      message.animationController.dispose();
    }
    super.dispose();
  }
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(title: const Text('FriendlyChat')),
        body: Container(
          child: Column(
            children: [
              Flexible(
                  child: ListView.builder(
                padding: const EdgeInsets.all(8.0),
                reverse: true,
                itemBuilder: (_, index) => _messages[index],
                itemCount: _messages.length,
              )),
              const Divider(height: 1.0),
              Container(
                decoration: BoxDecoration(color: Theme.of(context).cardColor),
                child: _buildTextComposer(),
              )
            ],
          ),
          decoration: Theme.of(context).platform == TargetPlatform.iOS
            ? BoxDecoration(
              border: Border(
                top: BorderSide(color: Colors.grey[200]!),
              )
            )
            : null,
        ));
  }
```

> Adding UI for composing & displaying messages
```dart
 Widget _buildTextComposer() {
    return IconTheme(
      data: IconThemeData(color: Theme.of(context).colorScheme.secondary),
      child: Container(
          margin: const EdgeInsets.symmetric(horizontal: 8.0),
          child: Row(children: [
            Flexible(
                child: TextField(
              controller: _textController,
              onChanged: (text) {
                setState(() {
                  _isComposing = text.isNotEmpty;
                });
              }, 
              onSubmitted: _isComposing ? _handleSubmitted: null,
              decoration:
                  const InputDecoration.collapsed(hintText: 'Send a message'),
              focusNode: _focusNode,
            )),
            Container(
              margin: const EdgeInsets.symmetric(horizontal: 4.0),
              child: Theme.of(context).platform == TargetPlatform.iOS ? 
              CupertinoButton(
                child: const Text('Send'),
                onPressed: _isComposing
                  ? () => _handleSubmitted(_textController.text)
                  : null,) :
              IconButton(
                  icon: const Icon(Icons.send),
                  onPressed: _isComposing
                      ? () => _handleSubmitted(_textController.text)
                      : null,
            ))
          ])),
    );
  }

```

> Animating the application & applying theme
```dart
class ChatMessage extends StatelessWidget {
  const ChatMessage(
      {required this.text, required this.animationController, Key? key})
      : super(key: key);

//

  @override
  Widget build(BuildContext context) {
    return SizeTransition(
      sizeFactor:
          CurvedAnimation(parent: animationController, curve: Curves.easeOut),
      axisAlignment: 0.0,
//

final ThemeData kIOSTheme = ThemeData(
  primarySwatch: Colors.red,
  primaryColor: Colors.grey[200],
);

final ThemeData kDefaultTheme = ThemeData(
  colorScheme: ColorScheme.fromSwatch(primarySwatch: Colors.red)
      .copyWith(secondary: Colors.redAccent[400]),
);


```


---

## Summary

Fun project to introduce flutter into my workflow. Building the chat feature will prove useful when implementing into another future project. Looking forward to the next lab.

We covered:

- How to build a Flutter app from the ground up
- How to use some of the shortcuts provided in Android Studio and IntelliJ
- How to run, hot reload, and debug your Flutter app on an emulator, a simulator, and a device
- How to customize your user interface with widgets and animations
- How to customize your user interface for Android and iOS


For more information refer to the official documentation.

- [Firebase Documentation](https://firebase.google.com/docs)
- [Flutterfire](https://firebase.google.com/docs/flutter/setup?platform=ios)
- [Google's awesome Flutter Youtube channel, Lots of great content here](https://www.youtube.com/channel/UCwXdFgeE9KYzlDdR7TG9cMw)

---

## Demo
![HomePage Gif](https://github.com/C-Dev66/friendly_chat/blob/main/screenshots/FriendlyChatDemo.gif)

