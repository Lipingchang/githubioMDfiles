---
title: flutter 应用
typora-root-url: flutter-应用
typora-copy-images-to: flutter-应用
date: 2019-04-05 10:59:23
tags:
- flutter
---

> <https://flutter.dev/docs/get-started/codelab>

```dart
import 'package:flutter/material.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Welcome to Flutter',
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text('Welcome to Flutter'),
        ),
        body: new Center(
          child: new Text('Hello World'),
        ),
      ),
    );
  }
}
```





- 文件 pubspec.yaml：![1554435162039](/1554435162039.png)

  和 package.json 文件差不多，用来导入外部的包，在用 `flutter packages get`后，会生成 pubspec.lock 来说明项目中用到的各个packages的版本号。



- 每次 点击![1554436506259](/1554436506259.png)中的闪电后，会hot reload应用，会使得 `Widget build(BuildContext context)`这个函数被再次调用。
- 