# DART — Complete Notes for Flutter Development

> Goal: everything you need to know in Dart before/while learning Flutter. Written in simple language with code + definitions for every topic.

---

## Table of Contents
1. [Introduction to Dart](#1-introduction-to-dart)
2. [Variables & Data Types](#2-variables--data-types)
3. [Null Safety](#3-null-safety)
4. [Operators](#4-operators)
5. [Control Flow](#5-control-flow)
6. [Functions](#6-functions)
7. [Collections (List, Set, Map)](#7-collections-list-set-map)
8. [Classes & OOP Basics](#8-classes--oop-basics)
9. [Constructors (all types)](#9-constructors-all-types)
10. [Inheritance, Abstract Classes, Interfaces, Mixins](#10-inheritance-abstract-classes-interfaces-mixins)
11. [Enums](#11-enums)
12. [Generics](#12-generics)
13. [Exception Handling](#13-exception-handling)
14. [Asynchronous Programming (Future, async/await, Stream)](#14-asynchronous-programming-future-asyncawait-stream)
15. [Records & Pattern Matching (Dart 3)](#15-records--pattern-matching-dart-3)
16. [Sealed Classes & Class Modifiers](#16-sealed-classes--class-modifiers)
17. [Extension Methods & Extension Types](#17-extension-methods--extension-types)
18. [Libraries, Imports & Packages](#18-libraries-imports--packages)
19. [Syntax Sugar You'll See Everywhere in Flutter](#19-syntax-sugar-youll-see-everywhere-in-flutter)
20. [Dart → Flutter Cheat Sheet](#20-dart--flutter-cheat-sheet)

---

## 1. Introduction to Dart

**Definition:** Dart is a client-optimized, object-oriented, statically-typed programming language made by Google. It's the language Flutter apps are written in.

**Why Dart for Flutter?**
- Compiles to native ARM/x86 code (fast apps) using **AOT** (Ahead Of Time) compilation for release builds.
- Uses **JIT** (Just In Time) compilation during development → gives Flutter's famous **Hot Reload**.
- Everything in Dart is an **object**, even numbers and functions.
- Statically typed but supports **type inference** (`var`), so you don't always have to write the type.

```dart
void main() {
  print('Hello, Dart!'); // entry point of every Dart app
}
```

- `main()` is always the starting point of execution.
- `print()` is the built-in function to output to console.

---

## 2. Variables & Data Types

**Definition:** A variable is a named storage location that holds a value. Dart is statically typed — every variable has a type, either explicit or inferred.

### Declaring variables
```dart
var name = 'Anik';        // type inferred as String
String city = 'Noida';    // explicit type
int age = 21;
double height = 5.9;
bool isStudent = true;
dynamic anything = 'can change type later'; // avoid unless necessary
```

### `var` vs `final` vs `const`
| Keyword | Meaning | Can reassign? | Value known at |
|---|---|---|---|
| `var` | type inferred, mutable | ✅ Yes | runtime |
| `final` | value set once | ❌ No | runtime (can be set later, once) |
| `const` | compile-time constant | ❌ No | compile time |

```dart
final today = DateTime.now(); // ok, decided at runtime, but only once
const pi = 3.14;              // must be known at compile time
```

**Definition — `final` vs `const`:** `final` means "assign once, at runtime." `const` means "this value is fixed even before the app runs" — used a LOT in Flutter for widgets that never change (`const Text('Hello')`) because it improves performance (widget isn't rebuilt).

### Core Data Types
```dart
int wholeNumber = 10;
double decimalNumber = 10.5;
num anyNumber = 10;      // can hold int or double
String text = 'flutter';
bool flag = false;
List<int> numbers = [1, 2, 3];
Set<int> uniqueNumbers = {1, 2, 3};
Map<String, int> ageMap = {'Anik': 21};
```

### Strings
```dart
String name = 'Anik';
String greeting = "Hello, $name!";           // string interpolation
String multiline = '''
This is
a multiline string
''';
String bigExpression = "Sum: ${2 + 3}";      // expression inside interpolation
```

---

## 3. Null Safety

**Definition:** Null safety means Dart's type system knows which variables can be `null` and which cannot, at **compile time**. This prevents the classic "null pointer" runtime crash. Since Dart 3, null safety is mandatory (sound null safety) — every Dart/Flutter project uses it.

```dart
String name = 'Anik';   // cannot be null
String? nickname;       // CAN be null (notice the ?)
```

### Key operators for null safety
```dart
String? city;

// ?. — safe call, returns null instead of crashing if city is null
print(city?.length);

// ?? — if-null, gives a default value
String displayCity = city ?? 'Unknown';

// ??= — assign only if currently null
city ??= 'Noida';

// ! — "trust me, this is not null" (bang/null assertion operator)
// Use carefully — throws error at runtime if actually null
print(city!.length);

// late — declare now, initialize later, but promise it WILL be set before use
late String username;
username = 'anik_dev';
```

**Why this matters for Flutter:** almost every widget property, API response field, and controller uses nullable types (`String?`, `int?`). Getting comfortable with `?`, `??`, `!`, and `late` is non-negotiable.

---

## 4. Operators

**Definition:** Symbols that perform operations on variables and values.

```dart
// Arithmetic
int a = 10, b = 3;
print(a + b); print(a - b); print(a * b);
print(a / b);   // 3.333... (double division)
print(a ~/ b);  // 3 (integer/truncating division)
print(a % b);   // 1 (modulo/remainder)

// Comparison
print(a == b); print(a != b); print(a > b); print(a < b);

// Logical
bool x = true, y = false;
print(x && y); print(x || y); print(!x);

// Assignment shorthand
int c = 5;
c += 2; c -= 1; c *= 3; c ~/= 2;

// Type test
print(a is int);      // true
print(a is! String);  // true

// Cascade notation (.. and ?..) — call multiple things on same object
var list = []..add(1)..add(2)..add(3);

// Conditional / ternary
String result = (a > b) ? 'a is bigger' : 'b is bigger';

// Spread operator
var list1 = [1, 2, 3];
var list2 = [0, ...list1, 4]; // [0, 1, 2, 3, 4]
```

---

## 5. Control Flow

### If-else
```dart
int marks = 85;
if (marks >= 90) {
  print('A grade');
} else if (marks >= 75) {
  print('B grade');
} else {
  print('C grade');
}
```

### Switch statement (traditional)
```dart
String grade = 'B';
switch (grade) {
  case 'A':
    print('Excellent');
    break;
  case 'B':
    print('Good');
    break;
  default:
    print('Needs improvement');
}
```

### Switch expression (Dart 3 — modern & shorter)
```dart
String result = switch (grade) {
  'A' => 'Excellent',
  'B' => 'Good',
  _   => 'Needs improvement', // _ is the default/wildcard
};
```

### Loops
```dart
// for loop
for (int i = 0; i < 5; i++) {
  print(i);
}

// for-in (used a LOT with lists/widgets in Flutter)
List<String> names = ['A', 'B', 'C'];
for (var name in names) {
  print(name);
}

// while
int i = 0;
while (i < 5) {
  print(i);
  i++;
}

// do-while
int j = 0;
do {
  print(j);
  j++;
} while (j < 5);

// break & continue work same as other languages
```

---

## 6. Functions

**Definition:** A function is a reusable block of code that performs a task and optionally returns a value. In Dart, functions are **first-class objects** — they can be stored in variables, passed as arguments, and returned from other functions.

```dart
// Basic function
int add(int a, int b) {
  return a + b;
}

// Arrow function — shorthand for single-expression functions
int addShort(int a, int b) => a + b;

// Optional positional parameters (square brackets)
String greet(String name, [String? title]) {
  return title != null ? '$title $name' : name;
}

// Named parameters (curly braces) — VERY common in Flutter widget constructors
void printInfo({required String name, int age = 18}) {
  print('$name is $age years old');
}
printInfo(name: 'Anik', age: 21);

// Function as a parameter (higher-order function)
void performOperation(int a, int b, int Function(int, int) operation) {
  print(operation(a, b));
}
performOperation(2, 3, (x, y) => x + y);

// Anonymous function / lambda
var square = (int x) => x * x;

// Typedef — naming a function type (used for callbacks)
typedef IntOperation = int Function(int, int);
```

**Definition — `required` vs default value:** In named parameters, `required` forces the caller to pass it; otherwise, give it a default value like `int age = 18` so it's optional.

---

## 7. Collections (List, Set, Map)

### List — ordered, allows duplicates
**Definition:** An indexed collection of objects, like an array.
```dart
List<String> fruits = ['apple', 'banana', 'mango'];
fruits.add('grape');
fruits.remove('banana');
print(fruits[0]);            // apple
print(fruits.length);
fruits.forEach((f) => print(f));
var upper = fruits.map((f) => f.toUpperCase()).toList();
var filtered = fruits.where((f) => f.startsWith('a')).toList();
bool hasMango = fruits.contains('mango');

// collection-if and collection-for (used heavily to build widget lists conditionally)
var list = [
  1, 2,
  if (true) 3,
  for (var i in [4, 5]) i,
]; // [1, 2, 3, 4, 5]
```

### Set — unordered, NO duplicates
```dart
Set<int> numbers = {1, 2, 3, 3}; // {1, 2, 3} — duplicate auto-removed
numbers.add(4);
print(numbers.contains(2));
```

### Map — key-value pairs
**Definition:** A collection of key-value pairs, like a dictionary/JSON object. Extremely important because API responses are usually parsed into `Map<String, dynamic>`.
```dart
Map<String, dynamic> user = {
  'name': 'Anik',
  'age': 21,
  'skills': ['Flutter', 'Dart', 'FastAPI'],
};
print(user['name']);
user['city'] = 'Noida';         // add/update
user.remove('age');
user.forEach((key, value) => print('$key: $value'));
print(user.keys);
print(user.values);
print(user.containsKey('name'));
```

**Important for Flutter/API work:** Most JSON parsing looks like:
```dart
import 'dart:convert';

String jsonString = '{"name": "Anik", "age": 21}';
Map<String, dynamic> data = jsonDecode(jsonString);
String backToJson = jsonEncode(data);
```

---

## 8. Classes & OOP Basics

**Definition:** A class is a blueprint for creating objects. It bundles data (fields/properties) and behavior (methods) together. Dart is a purely object-oriented language.

```dart
class Person {
  String name;      // field
  int age;

  Person(this.name, this.age); // constructor (shorthand)

  void introduce() {           // method
    print('Hi, I am $name and I am $age years old');
  }

  // Getter and setter
  String get greeting => 'Hello, $name';
  set updateName(String newName) => name = newName;
}

void main() {
  var p = Person('Anik', 21);
  p.introduce();
  print(p.greeting);
  p.updateName = 'Dhiman';
}
```

- **Field:** a variable belonging to a class (also called property/attribute).
- **Method:** a function belonging to a class.
- **Getter:** lets you read a computed value like a property (`p.greeting` not `p.greeting()`).
- **Setter:** lets you write/update a value with custom logic while looking like a normal assignment.

---

## 9. Constructors (all types)

**Definition:** A constructor is a special method used to create (instantiate) an object of a class.

```dart
class Car {
  String brand;
  int year;

  // 1. Default/standard constructor (shorthand syntax — most common)
  Car(this.brand, this.year);

  // 2. Named constructor — multiple ways to create the same object
  Car.electric(this.brand) : year = 2024;

  // 3. Factory constructor — doesn't always create a new instance;
  //    useful for returning cached objects or objects of a subtype
  factory Car.fromJson(Map<String, dynamic> json) {
    return Car(json['brand'], json['year']);
  }

  // 4. Const constructor — for compile-time-constant, immutable objects
  //    (Flutter widgets use this heavily for performance)
}

class ImmutablePoint {
  final int x, y;
  const ImmutablePoint(this.x, this.y); // const constructor
}
```

```dart
void main() {
  var c1 = Car('Tesla', 2023);
  var c2 = Car.electric('Tata');
  var c3 = Car.fromJson({'brand': 'BMW', 'year': 2022});
  const p1 = ImmutablePoint(1, 2); // created at compile time
}
```

**Why factory constructors matter in Flutter:** parsing API JSON into model classes almost always uses `factory Model.fromJson(...)`.

---

## 10. Inheritance, Abstract Classes, Interfaces, Mixins

### Inheritance
**Definition:** One class (child/subclass) can inherit fields and methods from another class (parent/superclass) using `extends`.
```dart
class Animal {
  void eat() => print('Eating...');
}

class Dog extends Animal {
  void bark() => print('Barking...');

  @override
  void eat() {
    super.eat();        // call parent's version
    print('Dog is eating');
  }
}
```

### Abstract classes
**Definition:** A class that cannot be instantiated directly and may contain methods without a body (to be implemented by subclasses). Used to define a common template.
```dart
abstract class Shape {
  double area();        // no body — must be implemented by subclass
  void describe() => print('I am a shape'); // can have a normal method too
}

class Circle extends Shape {
  double radius;
  Circle(this.radius);

  @override
  double area() => 3.14 * radius * radius;
}
```

### Interfaces
**Definition:** Dart has no separate `interface` keyword — **every class implicitly defines an interface**. You use `implements` to force a class to provide its own implementation of everything in another class.
```dart
class Flyable {
  void fly() => print('Flying');
}

class Bird implements Flyable {
  @override
  void fly() => print('Bird flying');   // MUST implement, nothing inherited
}
```
**Key difference:** `extends` → inherits actual code. `implements` → only inherits the "contract" (method signatures), you must write all logic yourself.

### Mixins
**Definition:** A mixin lets you reuse a class's code in multiple class hierarchies without using inheritance. Use `with` keyword. Solves Dart's "no multiple inheritance" limitation.
```dart
mixin Swimmer {
  void swim() => print('Swimming');
}

mixin Runner {
  void run() => print('Running');
}

class Athlete with Swimmer, Runner {}

void main() {
  var a = Athlete();
  a.swim();
  a.run();
}
```

---

## 11. Enums

**Definition:** A special class used to represent a fixed set of constant values.

```dart
enum Status { pending, approved, rejected }

void checkStatus(Status s) {
  if (s == Status.approved) print('Approved!');
}

// Enhanced enums (Dart 2.17+) — can have fields, constructors, methods
enum Planet {
  mercury(3.3),
  earth(5.9);

  final double mass;
  const Planet(this.mass);

  bool get isHeavy => mass > 5;
}

void main() {
  print(Planet.earth.mass);
  print(Planet.earth.isHeavy);
}
```

---

## 12. Generics

**Definition:** Generics let you write code that works with any type while still keeping type-safety, using placeholders like `<T>`. You've already been using it: `List<String>`, `Map<String, int>`.

```dart
class Box<T> {
  T value;
  Box(this.value);

  void show() => print('Value: $value');
}

void main() {
  var intBox = Box<int>(10);
  var stringBox = Box<String>('Dart');
  intBox.show();
}

// Generic function
T getFirst<T>(List<T> items) => items[0];

// Bounded generics — restrict T to a specific type or subtype
class NumberBox<T extends num> {
  T value;
  NumberBox(this.value);
}
```

**Why it matters in Flutter:** `FutureBuilder<T>`, `StreamBuilder<T>`, `ValueNotifier<T>`, API response models — all rely on generics.

---

## 13. Exception Handling

**Definition:** A mechanism to catch and handle runtime errors gracefully instead of crashing the app.

```dart
void divide(int a, int b) {
  try {
    if (b == 0) {
      throw Exception('Cannot divide by zero');
    }
    print(a / b);
  } on Exception catch (e) {
    print('Caught: $e');
  } catch (e, stackTrace) {
    print('Something else went wrong: $e');
  } finally {
    print('This always runs');
  }
}

// Custom exception
class InvalidAgeException implements Exception {
  final String message;
  InvalidAgeException(this.message);

  @override
  String toString() => 'InvalidAgeException: $message';
}
```

- `try` → code that might fail.
- `on ExceptionType catch(e)` → catch a **specific** exception type.
- `catch (e, stackTrace)` → catch **any** error, with optional stack trace.
- `finally` → always runs (cleanup code — e.g. closing a controller/connection).

---

## 14. Asynchronous Programming (Future, async/await, Stream)

**Definition:** Asynchronous programming lets your app do things (like network calls or file reads) without freezing the UI while waiting for the result. This is CRITICAL for Flutter since apps constantly call APIs.

### Future
**Definition:** A `Future` represents a value that will be available *at some point in the future* — either a result or an error.
```dart
Future<String> fetchData() {
  return Future.delayed(Duration(seconds: 2), () => 'Data loaded');
}

void main() {
  print('Start');
  fetchData().then((value) => print(value));
  print('End'); // this prints BEFORE 'Data loaded' — non-blocking
}
```

### async / await
**Definition:** `async` marks a function as asynchronous (it returns a `Future`). `await` pauses execution *inside that function* until the `Future` completes — without blocking the whole app.
```dart
Future<void> loadUser() async {
  print('Loading...');
  String data = await fetchData();  // waits here, but UI stays responsive
  print(data);
}
```

### Handling errors in async code
```dart
Future<void> getData() async {
  try {
    String result = await fetchData();
    print(result);
  } catch (e) {
    print('Error: $e');
  }
}
```

### Stream
**Definition:** A `Stream` is like a `Future`, but instead of one value, it can emit **multiple values over time** (e.g., real-time chat messages, sensor data, Firebase live updates).
```dart
Stream<int> countStream() async* {
  for (int i = 1; i <= 5; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i;                     // emits a value into the stream
  }
}

void main() async {
  await for (var value in countStream()) {
    print(value);
  }
}

// Or using listen
countStream().listen((value) {
  print('Received: $value');
});
```

**Why this section is critical for Flutter:** `FutureBuilder` and `StreamBuilder` widgets, API calls with `http`/`dio`, and Firebase real-time data all build directly on these concepts.

---

## 15. Records & Pattern Matching (Dart 3)

### Records
**Definition:** A record is a built-in, anonymous, immutable data structure that lets you group multiple values together *without creating a class*. Introduced in Dart 3.

```dart
// Positional record
(String, int) getUser() {
  return ('Anik', 21);
}

void main() {
  var user = getUser();
  print(user.$1); // Anik
  print(user.$2); // 21

  // Named record fields
  ({String name, int age}) namedUser = (name: 'Anik', age: 21);
  print(namedUser.name);
}
```

### Pattern matching & destructuring
**Definition:** Patterns let you check a value's shape/type and pull data out of it in one step.
```dart
var (name, age) = getUser(); // destructuring
print('$name is $age');

// switch with pattern matching
var point = (3, 4);
switch (point) {
  case (0, 0):
    print('origin');
  case (var x, var y) when x == y:
    print('on the diagonal');
  default:
    print('somewhere else');
}

// if-case pattern
if (getUser() case (var n, var a) when a >= 18) {
  print('$n is an adult');
}
```

**Why it matters:** modern Dart/Flutter code (especially state management like Bloc/Riverpod) increasingly uses records and pattern matching for cleaner code instead of creating tiny classes for everything.

---

## 16. Sealed Classes & Class Modifiers

**Definition:** Dart 3 introduced class modifiers to give more control over how a class can be used/extended. `sealed` is the most important one for Flutter state management.

```dart
sealed class Result {}

class Success extends Result {
  final String data;
  Success(this.data);
}

class Failure extends Result {
  final String error;
  Failure(this.error);
}

String handle(Result result) {
  // switch is EXHAUSTIVE — Dart forces you to handle every possible subtype
  return switch (result) {
    Success(data: var d) => 'Got: $d',
    Failure(error: var e) => 'Error: $e',
  };
}
```

Other class modifiers:
| Modifier | Meaning |
|---|---|
| `abstract` | cannot be instantiated |
| `final` (on class) | cannot be extended/implemented outside its own library |
| `sealed` | all subtypes must be in the same file; enables exhaustive `switch` |
| `interface` | can only be implemented, not extended, outside its library |
| `base` | must be extended/implemented, not used directly outside its library |
| `mixin class` | can be used both as a mixin and a normal class |

**Why it matters:** `sealed class` is the modern, recommended way to model UI states (Loading / Success / Error) in Flutter — replacing older enum-based or boolean-flag approaches.

---

## 17. Extension Methods & Extension Types

### Extension methods
**Definition:** Extensions let you **add new functionality to an existing class** (even ones you don't own, like `String` or `int`) without modifying it or creating a subclass.
```dart
extension StringExtension on String {
  String capitalize() {
    return '${this[0].toUpperCase()}${substring(1)}';
  }
}

void main() {
  print('anik'.capitalize()); // Anik
}
```

### Extension types (Dart 3.3+)
**Definition:** A zero-cost wrapper around an existing type — gives you a new "view"/type-safety on top of a type without runtime overhead. Useful for things like wrapping a raw `int` as a strongly-typed `UserId`.
```dart
extension type UserId(int id) {
  String display() => 'User#$id';
}

void main() {
  var uid = UserId(101);
  print(uid.display());
}
```

---

## 18. Libraries, Imports & Packages

**Definition:** A library is a collection of related Dart code (a file, or set of files). Every Dart file is implicitly its own library.

```dart
// importing Dart's built-in libraries
import 'dart:math';
import 'dart:async';
import 'dart:convert'; // for jsonEncode/jsonDecode

// importing your own file
import 'models/user.dart';

// importing a package (added via pub.dev / pubspec.yaml)
import 'package:http/http.dart' as http;

// show/hide specific parts of a library
import 'package:flutter/material.dart' show Text, Container;
import 'package:some_package/some_package.dart' hide SomeClass;

// deferred loading (lazy-load a library, loads only when needed)
import 'package:heavy_library/heavy_library.dart' deferred as heavy;
```

**pubspec.yaml:** the config file where you declare your project's dependencies (packages), name, version, and assets. This is how you add packages like `http`, `provider`, `firebase_core`, etc. to a Flutter project.

```yaml
dependencies:
  flutter:
    sdk: flutter
  http: ^1.2.0
```

---

## 19. Syntax Sugar You'll See Everywhere in Flutter

```dart
// Cascade notation — chain multiple calls on the same object
var buffer = StringBuffer()
  ..write('Hello')
  ..write(' ')
  ..write('World');

// Spread operator — unpack a list/set/map into another
var combined = [...list1, ...list2];

// Null-aware spread
var safeCombined = [...?nullableList];

// Collection-if / collection-for — build lists conditionally (used constantly in widget trees)
Widget build() {
  return Column(
    children: [
      Text('Always shown'),
      if (isLoggedIn) Text('Welcome back'),
      for (var item in items) Text(item),
    ],
  );
}

// Trailing commas — Dart formatter uses these to auto-format nicely (standard in Flutter code)
Widget build2() {
  return Container(
    color: Colors.blue,
    child: Text('Hi'),
  ); // <- trailing comma
}
```

---

## 20. Dart → Flutter Cheat Sheet

Quick mapping of "why did I just learn that Dart concept?" → where you'll use it in Flutter:

| Dart Concept | Where you'll use it in Flutter |
|---|---|
| Named parameters + `required` | Almost EVERY widget constructor (`Text('hi', style: ...)`) |
| `const` constructors | Performance — avoids unnecessary widget rebuilds |
| Null safety (`?`, `??`, `!`, `late`) | Handling nullable API data, controllers, form fields |
| `Future` / `async`/`await` | API calls (`http`, `dio`), file I/O, database queries |
| `Stream` / `StreamBuilder` | Real-time data — Firebase, WebSockets, sensor data |
| `factory` constructors | `Model.fromJson()` to parse API responses |
| Generics | `FutureBuilder<T>`, `StreamBuilder<T>`, `ValueNotifier<T>`, state management |
| Mixins | `TickerProviderStateMixin` for animations, `AutomaticKeepAliveClientMixin` |
| Abstract classes / interfaces | Repository patterns, dependency injection, testable architecture |
| Sealed classes + pattern matching | Modeling UI/API states (Loading, Success, Error) cleanly |
| Extensions | Adding helper methods to `BuildContext`, `String`, `DateTime`, etc. |
| Records | Returning multiple values from a function without a whole new class |
| Collection-if / collection-for | Conditionally building widget lists inside `children: [...]` |
| Cascade (`..`) | Configuring objects (e.g., `TextEditingController()..text = 'hi'`) |

---

## Final Tips for Practice
1. Don't just read — open [DartPad](https://dartpad.dev) and run every snippet above yourself.
2. Once comfortable with Sections 1–14, you're ready to start Flutter widgets.
3. Sections 15–17 (Records, Sealed classes, Extensions) are "modern Dart" — you'll see them a lot in newer Flutter codebases and in state management packages like Bloc/Riverpod, so don't skip them.
4. Keep this file as your quick-reference — bookmark the sections you forget most (usually: Null Safety, Async, and Constructors).
