---
title: Make authenticated requests
description: How to fetch authorized data from a web service.
prev:
  title: Fetch data from the internet
  path: /docs/cookbook/networking/fetch-data
next:
  title: Parse JSON in the background
  path: /docs/cookbook/networking/background-parsing
---

To fetch data from many web services, you need to provide
authorization. There are many ways to do this, but perhaps the most common
uses the `Authorization` HTTP header.

## Add authorization headers

The [`http`][] package provides a
convenient way to add headers to your requests.
Alternatively, use the [`HttpHeaders`][]
class from the `dart:io` library.

<!-- skip -->
```dart
Future<http.Response> fetchAlbum() {
  return http.get(
    'https://jsonplaceholder.typicode.com/albums/1',
    // Send authorization headers to the backend.
    headers: {HttpHeaders.authorizationHeader: "Basic your_api_token_here"},
  );
}
```

## Complete example

This example builds upon the
[Fetching data from the internet][] recipe.

```dart
import 'dart:async';
import 'dart:convert';
import 'dart:io';

import 'package:http/http.dart' as http;

Future<Album> fetchAlbum() async {
  final response = await http.get(
    'https://jsonplaceholder.typicode.com/albums/1',
    headers: {HttpHeaders.authorizationHeader: "Basic your_api_token_here"},
  );
  final responseJson = json.decode(response.body);

  return Album.fromJson(responseJson);
}

class Album {
  final int userId;
  final int id;
  final String title;

  Album({this.userId, this.id, this.title});

  factory Album.fromJson(Map<String, dynamic> json) {
    return Album(
      userId: json['userId'],
      id: json['id'],
      title: json['title'],
    );
  }
}
```


[Fetching data from the internet]: /docs/cookbook/networking/fetch-data
[`http`]: {{site.pub-pkg}}/http
[`HttpHeaders`]: {{site.dart.api}}/stable/dart-io/HttpHeaders-class.html
