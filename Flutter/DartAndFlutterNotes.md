# The Complete Dart & Flutter Developer Course
**By Rivaan Ranawat | Beginner to Advanced**

---

## Table of Contents
1. [PART 1: DART FUNDAMENTALS](#part-1-dart-fundamentals)
2. [PART 2: OBJECT ORIENTED PROGRAMMING](#part-2-oop-in-dart)
3. [PART 3: COLLECTIONS & DATA STRUCTURES](#part-3-collections--data-structures)
4. [PART 4: ASYNC DART](#part-4-async-dart)
5. [PART 5: ADVANCED DART FEATURES](#part-5-advanced-dart-features)
6. [PART 6: FLUTTER SETUP & BASICS](#part-6-flutter-setup--basics)
7. [PART 7: FLUTTER CORE WIDGETS](#part-7-flutter-core-widgets)
8. [PART 8: CURRENCY CONVERTER APP](#part-8-currency-converter-app)
9. [PART 9: WEATHER APP](#part-9-weather-app)
10. [PART 10: FLUTTER INTERNALS](#part-10-flutter-internals)
11. [PART 11: SHOP APP](#part-11-shop-app)
12. [PART 12: RESPONSIVE UI](#part-12-responsive-ui)

---

# PART 1: DART FUNDAMENTALS

## 00:00:00 — Course Overview
- Full-stack mobile development course: Dart language first, then Flutter framework.
- Structure: Language fundamentals → OOP → Async → Flutter widgets → Real projects (Currency Converter, Weather App, Shop App).
- Prerequisites: Basic programming familiarity helpful but not required.

---

## 00:02:16 — What is Dart?
- **Dart** is a strongly-typed, object-oriented language developed by Google.
- Compiles to native ARM code (mobile/desktop) or JavaScript (web).
- Primary language for Flutter; also used independently for CLI/server-side apps.
- Key design goals: Fast execution, AOT (Ahead-of-Time) + JIT (Just-in-Time) compilation, sound null safety.
- **Sound null safety**: Variables can't be null unless explicitly declared nullable (e.g., `String?`).

---

## 00:03:52 — Dart SDK
- **SDK** (Software Development Kit): Includes the Dart compiler, runtime, and core libraries.
- Installation: Download from `dart.dev` or install via Flutter SDK (Flutter bundles Dart).
- Verify install: `dart --version`
- Run a Dart file: `dart run filename.dart`
- DartPad (`dartpad.dev`): In-browser playground, great for quick experimentation.

---

## 00:06:57 — Print Statement
```dart
void main() {
  print('Hello, World!');         // prints to console
  print(42);                       // works with any type
  print('Value: ${2 + 2}');       // string interpolation with expressions
  print('Name: $name');           // simple variable interpolation
}
```
- `main()` is the **entry point** of every Dart program.
- `void` means the function returns nothing.
- String interpolation: `$variable` or `${expression}`.

---

## 00:09:59 — Operators
**Arithmetic:**
```dart
+  -  *  /      // standard (/ always returns double)
~/             // integer division
%              // modulus (remainder)
```

**Comparison:** `==  !=  >  <  >=  <=`

**Logical:** `&&  ||  !`

**Assignment:**
```dart
=   +=   -=   *=   /=   ~/=   %=
```

**Null-aware:**
```dart
??    // if-null: a ?? b  → returns b if a is null
??=   // assign only if null: a ??= 'default'
?.    // null-safe access: obj?.method()
```

**Type test:**
```dart
is     // a is String
is!    // a is! int
```

---

## 00:14:39 — Comments
```dart
// Single-line comment

/* Multi-line
   comment */

/// Documentation comment (shown in IDE tooltips)
/// Supports markdown formatting
```
- Prefer `///` for public APIs and class/method docs.

---

## 00:17:31 — Variables
### Type System
```dart
// Explicit typing
int age = 25;
double price = 9.99;
String name = 'Alice';
bool isActive = true;

// Type inference (var)
var city = 'Delhi';    // inferred as String
var count = 0;         // inferred as int

// Dynamic (avoid unless necessary)
dynamic anything = 42;
anything = 'now a string'; // valid but unsafe
```

### `final` vs `const`
```dart
final String username = 'alice';   // set once at runtime
const double pi = 3.14159;         // compile-time constant

// Key difference:
final DateTime now = DateTime.now();  // ✅ evaluated at runtime
const DateTime now = DateTime.now(); // ❌ error — not compile-time constant
```

### Null Safety
```dart
String name = 'Bob';      // non-nullable: CANNOT be null
String? nickname;         // nullable: CAN be null, defaults to null

// Null-aware operators
print(nickname?.length);        // safe call → null instead of crash
print(nickname ?? 'No name');   // fallback value
nickname!.length;               // force unwrap (throws if null — use carefully)
```

### Strings Deep Dive
```dart
String s1 = 'single quotes';
String s2 = "double quotes";
String s3 = '''
  multi-line
  string
''';

// Concatenation
String full = 'Hello' + ' ' + 'World';
String full2 = 'Hello $name';

// Common methods
s1.length;
s1.toUpperCase();
s1.toLowerCase();
s1.contains('sub');
s1.replaceAll('old', 'new');
s1.split(',');
s1.trim();
s1.isEmpty;
s1.startsWith('H');
```

### Numbers
```dart
int x = 10;
double y = 3.14;
num z = 5;     // supertype of int & double

int.parse('42');       // String → int
double.parse('3.14');  // String → double
42.toString();         // int → String
y.toStringAsFixed(2);  // "3.14"
y.round();   .ceil();   .floor();
```

---

# PART 2: OOP IN DART

## 01:11:35 — Control Flow
### if / else
```dart
if (score >= 90) {
  print('A');
} else if (score >= 80) {
  print('B');
} else {
  print('C');
}
```

### Ternary Operator
```dart
String result = score >= 50 ? 'Pass' : 'Fail';
```

### Switch Statement
```dart
switch (day) {
  case 'Monday':
    print('Start of week');
    break;
  case 'Friday':
    print('End of week');
    break;
  default:
    print('Midweek');
}
```

### Switch Expression (Dart 3+)
```dart
String label = switch (day) {
  'Monday' => 'Start',
  'Friday' => 'End',
  _ => 'Middle',
};
```

---

## 01:37:52 — Exercise 1
- Practical reinforcement: variables, operators, control flow combined.
- Common task: FizzBuzz, basic calculator, grade evaluator.
- Focus: solidify understanding before moving to loops.

---

## 01:46:06 — Loops
### for loop
```dart
for (int i = 0; i < 5; i++) {
  print(i);
}
```

### for-in loop (iterating collections)
```dart
List<String> fruits = ['apple', 'banana', 'cherry'];
for (String fruit in fruits) {
  print(fruit);
}
```

### while loop
```dart
int i = 0;
while (i < 5) {
  print(i);
  i++;
}
```

### do-while loop
```dart
int i = 0;
do {
  print(i);
  i++;
} while (i < 5);
```

### Loop control
```dart
break;     // exit loop entirely
continue;  // skip current iteration
```

---

## 02:10:49 — Functions
```dart
// Basic function
int add(int a, int b) {
  return a + b;
}

// Arrow function (single expression)
int multiply(int a, int b) => a * b;

// Named parameters (with defaults)
void greet({required String name, int age = 0}) {
  print('Hello $name, age $age');
}
greet(name: 'Alice', age: 25);

// Optional positional parameters
String fullName(String first, [String? last]) {
  return last != null ? '$first $last' : first;
}

// Functions as first-class objects
void runOp(int a, int b, int Function(int, int) op) {
  print(op(a, b));
}

// Anonymous functions / lambdas
var square = (int x) => x * x;

// Higher-order functions (on collections)
List<int> nums = [1, 2, 3, 4, 5];
nums.map((n) => n * 2).toList();
nums.where((n) => n.isEven).toList();
nums.reduce((a, b) => a + b);
nums.forEach((n) => print(n));
```

**Important:** Dart passes objects by reference for complex types, by value for primitives (int, double, bool, String — strings are immutable).

---

## 02:46:53 — Classes
```dart
class Person {
  // Instance variables
  String name;
  int age;

  // Constructor
  Person(this.name, this.age);

  // Named constructor
  Person.anonymous() : name = 'Unknown', age = 0;

  // Method
  void introduce() {
    print('I am $name, age $age');
  }

  // Getter
  String get info => '$name ($age)';

  // Setter
  set setAge(int value) {
    if (value >= 0) age = value;
  }
}

// Usage
var p = Person('Alice', 30);
p.introduce();
print(p.info);
```

### Constructors
```dart
class Point {
  final double x, y;

  // Default constructor with initializer list
  Point(this.x, this.y);

  // Named constructor
  Point.origin() : x = 0, y = 0;

  // Factory constructor (returns existing/computed instance)
  factory Point.fromMap(Map<String, double> map) {
    return Point(map['x']!, map['y']!);
  }
}
```

---

## 03:41:10 — Inheritance
```dart
class Animal {
  String name;
  Animal(this.name);

  void speak() => print('...');
}

class Dog extends Animal {
  Dog(String name) : super(name);   // call parent constructor

  @override
  void speak() => print('Woof!');   // override parent method

  void fetch() => print('$name fetches!');
}
```
- `extends` = single inheritance.
- `super` = access parent class members/constructor.
- `@override` annotation: good practice for clarity and compiler checks.

---

## 03:59:58 — implements Keyword
```dart
abstract class Flyable {
  void fly();          // no implementation — contract only
}

abstract class Swimmable {
  void swim();
}

// A class can implement MULTIPLE interfaces
class Duck implements Flyable, Swimmable {
  @override
  void fly() => print('Duck flying');

  @override
  void swim() => print('Duck swimming');
}
```
- `implements`: Must provide ALL methods — no inherited implementation.
- Dart has no `interface` keyword; any class can be used as an interface.

---

## 04:10:13 — Abstract Classes
```dart
abstract class Shape {
  // Abstract method — subclass MUST implement
  double area();

  // Concrete method — subclass INHERITS this
  void describe() => print('Area: ${area()}');
}

class Circle extends Shape {
  double radius;
  Circle(this.radius);

  @override
  double area() => 3.14159 * radius * radius;
}
```
- `abstract class`: Cannot be instantiated directly.
- Difference from interface (`implements`): `extends abstract class` inherits concrete methods; `implements` requires re-implementing everything.

---

## 04:15:03 — OOP in Dart (Overview)
The 4 pillars, Dart-specific implementation:

---

## 04:17:09 — Polymorphism
- Same interface, different behavior depending on type at runtime.
```dart
List<Animal> zoo = [Dog('Rex'), Cat('Whiskers')];
for (var animal in zoo) {
  animal.speak();  // calls Dog.speak() or Cat.speak() — determined at runtime
}
```

---

## 04:20:52 — Abstraction
- Hide complexity; expose only what's necessary.
- Achieved via abstract classes, interfaces, and access modifiers.
- Example: You call `car.start()` — you don't care about the engine internals.

---

## 04:23:12 — Encapsulation
```dart
class BankAccount {
  double _balance = 0;   // private (underscore prefix in Dart)

  double get balance => _balance;

  void deposit(double amount) {
    if (amount > 0) _balance += amount;
  }
}
```
- Dart uses `_` prefix for private (private to the **library/file**, not class).
- No `private`/`public` keywords — underscore is the convention.

---

## 04:26:14 — Mixins
```dart
mixin Logger {
  void log(String message) => print('[LOG] $message');
}

mixin Validator {
  bool isValid(String input) => input.isNotEmpty;
}

class UserService with Logger, Validator {
  void createUser(String name) {
    if (isValid(name)) {
      log('Creating user: $name');
    }
  }
}
```
- Mixins add behavior to a class without inheritance.
- Use `with` keyword.
- Cannot have constructors.
- Solves multiple inheritance limitations cleanly.

---

## 04:33:40 — Class Modifiers (Dart 3+)
| Modifier | Effect |
|----------|--------|
| `final class` | Cannot be extended or implemented outside library |
| `base class` | Can extend but not implement |
| `interface class` | Can implement but not extend |
| `sealed class` | All subclasses must be in the same library (exhaustive switch) |
| `abstract` | Cannot be instantiated |

```dart
sealed class Result {}
class Success extends Result { final String data; Success(this.data); }
class Failure extends Result { final String error; Failure(this.error); }

// Switch is exhaustive — compiler knows all subclasses
String handle(Result r) => switch (r) {
  Success s => 'OK: ${s.data}',
  Failure f => 'Error: ${f.error}',
};
```

---

# PART 3: COLLECTIONS & DATA STRUCTURES

## 04:40:48 — Lists
```dart
// Creation
List<int> nums = [1, 2, 3];
var empty = <String>[];
var filled = List.filled(5, 0);    // [0, 0, 0, 0, 0]
var generated = List.generate(5, (i) => i * 2); // [0, 2, 4, 6, 8]

// Access
nums[0];       // first element
nums.last;     // last element
nums.length;

// Modification
nums.add(4);
nums.addAll([5, 6]);
nums.insert(0, 99);       // insert at index
nums.remove(3);           // remove by value
nums.removeAt(0);         // remove by index
nums.removeLast();
nums.clear();

// Querying
nums.contains(2);
nums.indexOf(3);
nums.isEmpty;
nums.isNotEmpty;

// Iteration & transformation
nums.forEach((n) => print(n));
var doubled = nums.map((n) => n * 2).toList();
var evens = nums.where((n) => n.isEven).toList();
var sum = nums.reduce((a, b) => a + b);
nums.sort();
nums.sort((a, b) => b.compareTo(a)); // descending

// Spread operator
var combined = [...nums, ...doubled];

// List spread in constructors (common in Flutter)
var items = [
  if (isAdmin) 'Admin Panel',
  ...regularItems,
];
```

---

## 05:23:04 — Sets
```dart
// Set: unordered, unique elements
Set<String> colors = {'red', 'green', 'blue'};
var s = <int>{};       // empty set

colors.add('yellow');
colors.add('red');     // duplicate — ignored
colors.remove('green');
colors.contains('blue');  // fast O(1) lookup
colors.length;

// Set operations
var a = {1, 2, 3};
var b = {2, 3, 4};
a.union(b);        // {1, 2, 3, 4}
a.intersection(b); // {2, 3}
a.difference(b);   // {1}
```
- Use Set when you need **uniqueness** and **fast membership checks**.
- Use List when you need **ordering** or **duplicates**.

---

## 05:25:39 — Maps
```dart
// Map: key-value pairs
Map<String, int> scores = {'Alice': 95, 'Bob': 87};
var m = <String, dynamic>{};

// Access
scores['Alice'];          // 95
scores['Unknown'];        // null (no error)
scores.containsKey('Bob');
scores.containsValue(95);

// Modification
scores['Charlie'] = 91;          // add/update
scores.putIfAbsent('Dan', () => 80); // only adds if key absent
scores.remove('Bob');
scores.update('Alice', (v) => v + 5); // update existing value

// Iteration
scores.forEach((key, value) => print('$key: $value'));
scores.keys;
scores.values;
scores.entries;    // Iterable<MapEntry<K,V>>

for (var entry in scores.entries) {
  print('${entry.key}: ${entry.value}');
}

// Transformations
var upperCased = scores.map((k, v) => MapEntry(k.toUpperCase(), v));
```

---

## 05:50:32 — Enums
```dart
// Basic enum
enum Direction { north, south, east, west }

// Enhanced enum (Dart 2.17+)
enum Color {
  red(0xFF0000),
  green(0x00FF00),
  blue(0x0000FF);

  final int hexValue;
  const Color(this.hexValue);

  String get hex => '#${hexValue.toRadixString(16).padLeft(6, '0')}';
}

// Usage
Direction d = Direction.north;
print(d.name);    // 'north'
print(d.index);   // 0

// Enums work great with switch
switch (d) {
  case Direction.north: print('Go up');
  case Direction.south: print('Go down');
  // ...
}
```

---

# PART 4: ASYNC DART

## 06:03:03 — Exception Handling
```dart
try {
  int result = 10 ~/ 0;       // throws IntegerDivisionByZeroException
  throw Exception('custom error');
} on IntegerDivisionByZeroException {
  print('Divide by zero');
} on FormatException catch (e) {
  print('Format error: $e');
} catch (e, stackTrace) {
  print('Unknown error: $e');
  print(stackTrace);
} finally {
  print('Always runs');
}

// Custom exceptions
class InsufficientFundsException implements Exception {
  final double amount;
  InsufficientFundsException(this.amount);
  @override
  String toString() => 'Insufficient funds: need \$$amount more';
}
```

---

## 06:11:45 — Futures
- **Future**: Represents a value that will be available at some point in the future (async operation).

```dart
// Returning a Future
Future<String> fetchUser() async {
  await Future.delayed(Duration(seconds: 2));
  return 'Alice';
}

// Consuming with async/await
void main() async {
  print('Fetching...');
  String user = await fetchUser();
  print('Got: $user');
}

// .then() / .catchError() — alternative chaining style
fetchUser()
  .then((user) => print(user))
  .catchError((e) => print('Error: $e'))
  .whenComplete(() => print('Done'));

// Future.wait — run multiple futures in parallel
Future<void> fetchAll() async {
  var results = await Future.wait([
    fetchUser(),
    fetchData(),
    fetchConfig(),
  ]);
}

// Future.delayed — useful for testing/mocking
await Future.delayed(Duration(milliseconds: 500));
```

**Key concepts:**
- `async` marks a function as asynchronous.
- `await` suspends execution until the Future completes.
- Functions marked `async` always return a `Future`.
- Never block the main thread — always await or handle async.

---

## 06:56:08 — Streams
- **Stream**: A sequence of asynchronous events over time (like a pipe of data).

```dart
// Creating a Stream
Stream<int> countDown(int from) async* {
  for (int i = from; i >= 0; i--) {
    await Future.delayed(Duration(seconds: 1));
    yield i;        // yield emits a value into the stream
  }
}

// Listening to a Stream
StreamSubscription sub = countDown(5).listen(
  (value) => print(value),
  onError: (e) => print('Error: $e'),
  onDone: () => print('Completed'),
);

// Cancel a subscription
sub.cancel();

// await for — cleaner iteration
await for (int n in countDown(5)) {
  print(n);
}

// Stream types
// Single subscription: one listener only (most streams)
// Broadcast: multiple listeners (e.g., UI events)

StreamController<String> controller = StreamController<String>.broadcast();
controller.sink.add('event 1');
controller.stream.listen((e) => print(e));
controller.close();

// Stream transformations
countDown(10)
  .where((n) => n.isEven)
  .map((n) => 'Number: $n')
  .listen(print);
```

**Future vs Stream:**
| | Future | Stream |
|---|---|---|
| Values | One | Many |
| When complete | Once | Multiple times (until done) |
| Use case | HTTP response | WebSocket, sensor data, UI events |

---

# PART 5: ADVANCED DART FEATURES

## 07:19:46 — (Bonus) Creating Records
- **Records** (Dart 3+): Lightweight, anonymous, immutable data structures.
```dart
// Record literal
var point = (10.0, 20.0);           // positional
var person = (name: 'Alice', age: 25); // named

// Access
print(point.$1);   // 10.0
print(point.$2);   // 20.0
print(person.name);
print(person.age);

// Records in functions
(String, int) getUser() => ('Alice', 30);
var (name, age) = getUser();  // destructuring
```

---

## 07:23:57 — (Bonus) Patterns & Pattern Matching
```dart
// Switch patterns (Dart 3+)
switch (shape) {
  case Circle(radius: var r): print('Circle r=$r');
  case Rectangle(width: var w, height: var h): print('Rect ${w}x$h');
}

// List patterns
var [first, second, ...rest] = [1, 2, 3, 4, 5];

// Map patterns
var {'name': String name, 'age': int age} = userData;

// Guard clauses in patterns
switch (n) {
  case int x when x > 0: print('positive');
  case int x when x < 0: print('negative');
  default: print('zero');
}
```

---

## 07:36:11 — Extensions
```dart
// Add methods to existing classes without modifying them
extension StringUtils on String {
  bool get isEmail => contains('@') && contains('.');
  String capitalize() => isEmpty ? '' : '${this[0].toUpperCase()}${substring(1)}';
  String truncate(int maxLength) =>
      length <= maxLength ? this : '${substring(0, maxLength)}...';
}

// Usage
'hello@test.com'.isEmail;   // true
'hello world'.capitalize(); // 'Hello world'

// Extensions on nullable types
extension NullableStringExt on String? {
  bool get isNullOrEmpty => this == null || this!.isEmpty;
}
```

---

# PART 6: FLUTTER SETUP & BASICS

## 07:42:25 — Introduction to Flutter
- **Flutter**: Google's UI toolkit for building natively compiled apps from a single codebase.
- Targets: iOS, Android, Web, Windows, macOS, Linux.
- Uses Dart language, renders its own widgets via **Skia/Impeller** graphics engine (no native components).
- Key advantage: Pixel-perfect UI across platforms, fast development with hot reload.

---

## 07:42:35 — Installing Flutter
```bash
# macOS (via Homebrew)
brew install flutter

# Or download from flutter.dev and add to PATH
export PATH="$PATH:`pwd`/flutter/bin"

# Verify
flutter doctor    # shows what's installed / what's missing
flutter doctor -v # verbose output
```

---

## 07:51:59 — Installing Android Studio & Configuring for Android
- Download Android Studio from developer.android.com.
- Install Flutter & Dart plugins via Plugins settings.
- Android SDK: Accept licenses → `flutter doctor --android-licenses`.
- Create Android Virtual Device (AVD) in Device Manager.
- Verify: `flutter devices` should list the emulator.

---

## 07:56:37 — Installing Xcode & Configuring for iOS
- Available on Mac only via App Store.
- After install: `sudo xcode-select --switch /Applications/Xcode.app`
- `sudo xcodebuild -runFirstLaunch`
- Install CocoaPods: `sudo gem install cocoapods`
- iOS Simulator: `open -a Simulator`

---

## 07:58:47 — Installing VS Code
- Download from code.visualstudio.com.
- Essential extensions: **Flutter**, **Dart** (install Flutter extension — Dart comes automatically).

---

## 08:00:24 — Exploring VS Code
- Command Palette: `Cmd+Shift+P` (Mac) / `Ctrl+Shift+P` (Windows).
- Flutter: New Project via Command Palette.
- Hot Reload: `r` in terminal, or save file (with auto hot reload setting).
- Hot Restart: `R` in terminal (resets state).
- Debug Console: View → Debug Console.

---

## 08:04:41 — Creating & Exploring The Flutter Project
Structure:
```
my_app/
├── lib/           ← Your Dart code lives here
│   └── main.dart  ← Entry point
├── android/       ← Android-specific config
├── ios/           ← iOS-specific config
├── web/           ← Web-specific config
├── test/          ← Unit & widget tests
└── pubspec.yaml   ← Dependencies, assets, fonts
```
- `pubspec.yaml`: Flutter's package manifest. Add dependencies, declare assets/fonts here.
- `lib/` is where almost all your work happens.

---

## 08:18:27 — Running Flutter App
```bash
flutter run                   # run on connected device
flutter run -d chrome         # run in browser
flutter run -d ios            # run on iOS simulator
flutter run --release         # production build
flutter build apk             # Android APK
flutter build ios             # iOS build
```

---

## 08:31:11 — Writing First Flutter Code!

## 08:32:34 — Importing Packages and material.dart
```dart
import 'package:flutter/material.dart';       // Material Design widgets
import 'package:flutter/cupertino.dart';      // iOS-style widgets
import 'package:flutter/widgets.dart';        // base Flutter widgets only
```

---

## 08:35:20 — runApp Function
```dart
void main() {
  runApp(const MyApp());   // bootstraps the app, takes a Widget
}
```
- `runApp()` inflates the given widget and attaches it to the screen.
- The widget passed becomes the root of the widget tree.

---

## 08:37:24 — What are Widgets?
- **Everything in Flutter is a Widget**: text, buttons, layout, styling, animation.
- Widgets are **immutable descriptions** of UI — they don't draw themselves, they describe what to draw.
- Flutter rebuilds widgets when state changes (cheap — they're just Dart objects).
- Analogy: Widgets are like blueprints; the Flutter engine constructs the actual UI from them.

---

## 08:38:10 — Text Widget
```dart
Text('Hello, Flutter!')

Text(
  'Styled Text',
  style: TextStyle(
    fontSize: 24,
    fontWeight: FontWeight.bold,
    color: Colors.blue,
    letterSpacing: 1.5,
    fontStyle: FontStyle.italic,
    decoration: TextDecoration.underline,
  ),
  textAlign: TextAlign.center,
  maxLines: 2,
  overflow: TextOverflow.ellipsis,
)

// Rich text with multiple styles
RichText(
  text: TextSpan(
    children: [
      TextSpan(text: 'Hello ', style: TextStyle(color: Colors.black)),
      TextSpan(text: 'Flutter', style: TextStyle(color: Colors.blue, fontWeight: FontWeight.bold)),
    ],
  ),
)
```

---

## 08:55:24 — Types of Widgets
| Type | Description | Example |
|------|-------------|---------|
| **Structural** | Layout & positioning | `Column`, `Row`, `Stack` |
| **Stylistic** | Appearance | `Container`, `DecoratedBox` |
| **Input** | User interaction | `TextField`, `GestureDetector` |
| **Display** | Show content | `Text`, `Image`, `Icon` |
| **State-based** | `StatelessWidget` or `StatefulWidget` | — |

---

## 08:57:22 — What is State?
- **State**: Data that can change over time and cause the UI to rebuild.
- Examples: user input, fetched data, toggle values, animation position.
- Widgets rebuild when their state changes — Flutter re-calls the `build()` method.

---

## 08:58:48 — StatelessWidget
```dart
class MyButton extends StatelessWidget {
  final String label;
  final VoidCallback onTap;

  const MyButton({super.key, required this.label, required this.onTap});

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: onTap,
      child: Container(
        padding: const EdgeInsets.all(12),
        child: Text(label),
      ),
    );
  }
}
```
- No mutable state — once built, it doesn't change unless parent rebuilds it.
- Always `const` if all properties are constant.

---

## 09:11:43 — Material & Cupertino Design
- **Material**: Google's design language (Android-style).
- **Cupertino**: Apple's design language (iOS-style).
- Flutter lets you use either or mix them.
- Start with `MaterialApp` for most projects (better component coverage).

---

## 09:13:51 — MaterialApp
```dart
MaterialApp(
  title: 'My App',
  theme: ThemeData(
    colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
    useMaterial3: true,
  ),
  home: const HomePage(),
  // Routes
  routes: {
    '/': (context) => const HomePage(),
    '/details': (context) => const DetailsPage(),
  },
  debugShowCheckedModeBanner: false,
)
```

---

## 09:17:45 — Scaffold Widget
```dart
Scaffold(
  appBar: AppBar(title: const Text('Home')),
  body: const Center(child: Text('Content')),
  floatingActionButton: FloatingActionButton(
    onPressed: () {},
    child: const Icon(Icons.add),
  ),
  drawer: Drawer(...),
  bottomNavigationBar: BottomNavigationBar(...),
  backgroundColor: Colors.white,
)
```
- `Scaffold` provides the basic visual structure: app bar, body, FAB, drawer, snack bar support.

---

## 09:21:37 — Center Widget
```dart
Center(child: Text('Centered text'))
// Centers child within available space
// Equivalent to Align(alignment: Alignment.center, ...)
```

---

## 09:26:28 — Widget Tree
```
MaterialApp
  └── Scaffold
        ├── AppBar
        │     └── Text
        └── Center
              └── Column
                    ├── Text
                    └── ElevatedButton
```
- Flutter builds a tree of widgets.
- Parent widgets pass constraints DOWN; children report sizes UP.
- The rendering engine walks the tree to paint the UI.

---

## 09:29:09 — Splitting & Extracting Widgets
- Extract widgets into separate classes or files for readability and reuse.
- Use VS Code shortcut: right-click on widget → "Extract Widget".
- Rule of thumb: if a widget subtree has 20+ lines, consider extracting.

---

## 09:34:49 — What is BuildContext?
- `BuildContext` is a reference to **where a widget lives in the tree**.
- Used to look up inherited data (`Theme.of(context)`, `MediaQuery.of(context)`).
- Never store BuildContext across async gaps without checking `mounted`.
```dart
if (context.mounted) {
  Navigator.of(context).pop();
}
```

---

## 09:37:38 — Importing Files & Magic of Flutter Extension
```dart
// Absolute (package import)
import 'package:my_app/screens/home_screen.dart';

// Relative
import '../widgets/my_button.dart';
```
- Flutter VS Code extension auto-imports on paste/type.
- Organize by feature or type: `screens/`, `widgets/`, `models/`, `services/`.

---

# PART 7: FLUTTER CORE WIDGETS

## 09:43:31 — Column Widget
```dart
Column(
  mainAxisAlignment: MainAxisAlignment.center,    // vertical alignment
  crossAxisAlignment: CrossAxisAlignment.start,   // horizontal alignment
  mainAxisSize: MainAxisSize.min,                 // shrink-wrap or expand
  children: [
    Text('Item 1'),
    SizedBox(height: 8),    // spacing
    Text('Item 2'),
  ],
)
```
- Column: vertical axis = main axis.
- Row: horizontal axis = main axis.
- `SizedBox` is the idiomatic spacer.

---

## 09:52:10 — ColoredBox Widget
```dart
ColoredBox(
  color: Colors.blue,
  child: Text('Hello'),
)
// Lightweight — only fills color, no padding/margins
// Use Container when you need more: padding, margin, border, etc.
```

---

## 09:53:01 — Color Class
```dart
Colors.blue            // Material color
Colors.blue[300]       // shade (50 to 900)
Colors.blue.shade200

Color(0xFF2196F3)      // ARGB hex
Color.fromARGB(255, 33, 150, 243)
Color.fromRGBO(33, 150, 243, 1.0)

// Opacity
Colors.black.withOpacity(0.5)
Colors.blue.withAlpha(128)
```

---

## 09:56:53 — TextStyle
```dart
TextStyle(
  fontSize: 18,
  fontWeight: FontWeight.w600,    // w100 to w900, bold = w700
  color: Colors.black87,
  fontFamily: 'Roboto',
  letterSpacing: 0.5,
  wordSpacing: 2.0,
  height: 1.5,                    // line height multiplier
  fontStyle: FontStyle.italic,
  decoration: TextDecoration.underline,
  decorationColor: Colors.blue,
  shadows: [Shadow(blurRadius: 4, color: Colors.grey)],
)

// Inherit and override from theme
Theme.of(context).textTheme.headlineMedium!.copyWith(color: Colors.red)
```

---

## 10:04:22 — Colors (Theme Integration)
```dart
// Prefer theme colors over hardcoded ones
Theme.of(context).colorScheme.primary
Theme.of(context).colorScheme.surface
Theme.of(context).colorScheme.onPrimary

// ColorScheme generation
ColorScheme.fromSeed(seedColor: Colors.green)
```

---

## 10:06:49 — TextField Widget
```dart
final TextEditingController _controller = TextEditingController();

TextField(
  controller: _controller,
  keyboardType: TextInputType.number,
  decoration: InputDecoration(
    labelText: 'Amount',
    hintText: 'Enter amount',
    prefixIcon: Icon(Icons.attach_money),
    suffixText: 'USD',
    border: OutlineInputBorder(
      borderRadius: BorderRadius.circular(12),
    ),
    filled: true,
    fillColor: Colors.grey[100],
  ),
  onChanged: (value) => print(value),     // fires on every keystroke
  onSubmitted: (value) => print(value),   // fires on submit/enter
  maxLength: 10,
  obscureText: true,    // for passwords
)

// Accessing value
String text = _controller.text;

// Clearing
_controller.clear();

// Dispose controller to prevent memory leaks
@override
void dispose() {
  _controller.dispose();
  super.dispose();
}
```

---

## 10:48:00 — Why Build Function Should Contain NO Complex Tasks
- `build()` can be called **many times** — on every frame if needed.
- Never do: HTTP calls, heavy computations, database queries in `build()`.
- Do in `build()`: Read state, construct widget tree.
- Do in `initState()` / event handlers / providers: fetch data, run logic.

---

## 10:53:12 — Padding & Container Widget
```dart
// Padding — only adds space around child
Padding(
  padding: const EdgeInsets.all(16),
  child: Text('Padded'),
)

EdgeInsets.all(16)
EdgeInsets.symmetric(horizontal: 24, vertical: 12)
EdgeInsets.only(left: 8, top: 4)
EdgeInsets.fromLTRB(8, 4, 8, 4)

// Container — the "Swiss Army knife" widget
Container(
  width: 200,
  height: 100,
  padding: const EdgeInsets.all(16),
  margin: const EdgeInsets.only(bottom: 8),
  decoration: BoxDecoration(
    color: Colors.white,
    borderRadius: BorderRadius.circular(12),
    border: Border.all(color: Colors.grey.shade300),
    boxShadow: [
      BoxShadow(color: Colors.black12, blurRadius: 8, offset: Offset(0, 2))
    ],
    gradient: LinearGradient(colors: [Colors.blue, Colors.purple]),
  ),
  child: Text('Hello'),
)
```

---

## 11:02:01 — Padding vs Margin
- `padding`: Space INSIDE the container (between border and content).
- `margin` (via Container): Space OUTSIDE the container (between container and surroundings).
- `Padding` widget = only padding, no background/decoration.
- `Container` = padding + margin + decoration + sizing all in one.

---

## 11:07:56 — TextButton Widget
```dart
TextButton(
  onPressed: () {},
  style: TextButton.styleFrom(
    foregroundColor: Colors.blue,
    padding: const EdgeInsets.symmetric(horizontal: 24, vertical: 12),
    textStyle: const TextStyle(fontSize: 16),
  ),
  child: const Text('Click Me'),
)
```

---

## 11:13:35 — Flutter Lints
- `flutter_lints` package enforces best practices.
- `analysis_options.yaml` configures lint rules.
- Common lint: use `const` where possible, prefer `final`, avoid print in production.

---

## 11:34:29 — ElevatedButton Widget
```dart
ElevatedButton(
  onPressed: () {},
  style: ElevatedButton.styleFrom(
    backgroundColor: Colors.blue,
    foregroundColor: Colors.white,
    padding: const EdgeInsets.symmetric(horizontal: 32, vertical: 16),
    shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
    elevation: 4,
  ),
  child: const Text('Submit'),
)

// With icon
ElevatedButton.icon(
  onPressed: () {},
  icon: const Icon(Icons.send),
  label: const Text('Send'),
)
```

---

## 11:44:26 — AppBar Widget
```dart
AppBar(
  title: const Text('My App'),
  centerTitle: true,
  leading: IconButton(icon: const Icon(Icons.menu), onPressed: () {}),
  actions: [
    IconButton(icon: const Icon(Icons.search), onPressed: () {}),
    IconButton(icon: const Icon(Icons.more_vert), onPressed: () {}),
  ],
  backgroundColor: Theme.of(context).colorScheme.inversePrimary,
  elevation: 0,
  bottom: TabBar(...),    // for tabbed interface
)
```

---

## 11:51:47 — StatefulWidget
```dart
class CounterPage extends StatefulWidget {
  const CounterPage({super.key});

  @override
  State<CounterPage> createState() => _CounterPageState();
}

class _CounterPageState extends State<CounterPage> {
  int _count = 0;    // mutable state lives here

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Text('$_count', style: const TextStyle(fontSize: 48)),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () => setState(() => _count++),
        child: const Icon(Icons.add),
      ),
    );
  }
}
```
- `StatefulWidget`: Immutable configuration.
- `State<T>`: Mutable state that persists across rebuilds.
- Always access widget properties via `widget.propertyName` inside State.

---

## 12:24:38 — Build Function Can Be Called How Many Times?
- `build()` can be called on every `setState()`, parent rebuild, or theme change.
- Flutter is optimized for this — cheap widget creation is by design.
- Avoid expensive work in `build()`.

---

## 12:27:11 — setState
```dart
setState(() {
  _counter++;           // wrap all state mutations in setState()
  _items.add('new');
});
// setState: marks widget as dirty → schedules rebuild → build() is called
```
- Only call `setState()` when something changed AND you want the UI to reflect it.
- Don't call `setState()` in `build()`, `dispose()`, or after widget is unmounted.

---

## 12:41:19 — CupertinoApp & iOS Styled Widgets
```dart
// Use CupertinoApp for full iOS experience
CupertinoApp(
  theme: CupertinoThemeData(primaryColor: CupertinoColors.activeBlue),
  home: CupertinoPageScaffold(
    navigationBar: CupertinoNavigationBar(middle: Text('iOS App')),
    child: CupertinoButton(onPressed: () {}, child: Text('Press')),
  ),
)

// iOS-only widgets
CupertinoSlider, CupertinoSwitch, CupertinoDatePicker
CupertinoAlertDialog, CupertinoActionSheet
```

---

## 12:59:14 — initState and dispose
```dart
@override
void initState() {
  super.initState();
  // Called once when widget is inserted into the tree
  // Use for: initial data fetch, subscriptions, animation controllers
  _fetchData();
  _controller = AnimationController(vsync: this, duration: Duration(seconds: 1));
}

@override
void dispose() {
  // Called when widget is permanently removed from the tree
  // Use for: cancel subscriptions, dispose controllers, close streams
  _controller.dispose();
  _textController.dispose();
  _subscription.cancel();
  super.dispose();    // always call super last
}
```

---

## 13:02:05 — Recap & Widget Lifecycle
```
Constructor → createState() → initState() → build() → [setState → build()]* → deactivate() → dispose()
```
- `didUpdateWidget()`: Called when parent passes new config to this widget.
- `didChangeDependencies()`: Called when `InheritedWidget` dependency changes.

---

# PART 8: WEATHER APP

## 13:09:53 — Weather App Demo
Full app using: REST API, JSON parsing, async/await, dynamic UI.

---

## 13:26:48 — GestureDetector & InkWell Widget
```dart
// GestureDetector — detects any gesture, no visual feedback
GestureDetector(
  onTap: () {},
  onLongPress: () {},
  onDoubleTap: () {},
  onPanUpdate: (details) {},
  child: Container(...),
)

// InkWell — Material ripple effect on tap
InkWell(
  onTap: () {},
  borderRadius: BorderRadius.circular(8),
  child: Container(...),
)
```

---

## 13:29:20 — IconButton Widget
```dart
IconButton(
  icon: const Icon(Icons.refresh),
  onPressed: () {},
  tooltip: 'Refresh',
  iconSize: 28,
  color: Colors.white,
)
```

---

## 13:30:17 — PlaceHolder Widget
```dart
// Shows a box with an X — useful during development
Placeholder(
  color: Colors.red,
  fallbackHeight: 200,
)
```

---

## 13:34:22 — Card Widget
```dart
Card(
  elevation: 4,
  margin: const EdgeInsets.all(8),
  shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(16)),
  color: Colors.white,
  child: Padding(
    padding: const EdgeInsets.all(16),
    child: Text('Card content'),
  ),
)
```

---

## 13:45:35 — ClipRRect Widget
```dart
// Clips child to rounded rectangle
ClipRRect(
  borderRadius: BorderRadius.circular(12),
  child: Image.network('https://...'),
)
```

---

## 13:47:01 — Backdrop and ImageFilter Widget
```dart
// Blur/frosted glass effect
BackdropFilter(
  filter: ImageFilter.blur(sigmaX: 10, sigmaY: 10),
  child: Container(
    color: Colors.white.withOpacity(0.1),
    child: Text('Frosted Glass'),
  ),
)
// Must be placed inside a Stack with background behind it
```

---

## 13:58:14 — Row Widget
```dart
Row(
  mainAxisAlignment: MainAxisAlignment.spaceBetween,
  crossAxisAlignment: CrossAxisAlignment.center,
  children: [
    Text('Left'),
    Expanded(child: Text('Center — takes remaining space')),
    Text('Right'),
  ],
)
```

---

## 14:07:49 — SingleChildScrollView Widget
```dart
SingleChildScrollView(
  scrollDirection: Axis.vertical,    // or Axis.horizontal
  child: Column(
    children: [...many items...],
  ),
)
```

---

## 14:25:07 — Passing Arguments
```dart
// Between screens via constructor
Navigator.push(context, MaterialPageRoute(
  builder: (_) => DetailsPage(itemId: 42, title: 'My Item'),
));

// In DetailsPage
class DetailsPage extends StatelessWidget {
  final int itemId;
  final String title;
  const DetailsPage({super.key, required this.itemId, required this.title});
  // ...
}
```

---

## 14:35:02 — http Plugin in Flutter
```yaml
# pubspec.yaml
dependencies:
  http: ^1.2.0
```
```dart
import 'package:http/http.dart' as http;
import 'dart:convert';

Future<Map<String, dynamic>> fetchData(String url) async {
  final response = await http.get(Uri.parse(url));

  if (response.statusCode == 200) {
    return jsonDecode(response.body) as Map<String, dynamic>;
  } else {
    throw Exception('Failed to load data: ${response.statusCode}');
  }
}
```

---

## 14:38:12 — OpenMapWeather API
```dart
// Base URL: api.openweathermap.org/data/2.5/forecast
const String apiKey = 'YOUR_API_KEY';
final url = Uri.parse(
  'https://api.openweathermap.org/data/2.5/forecast?q=London&APPID=$apiKey'
);
```

---

## 14:44:57 — Handling Future in initState
```dart
@override
void initState() {
  super.initState();
  _weatherFuture = _fetchWeather();   // store the future, don't await here
}

Future<WeatherData> _fetchWeather() async {
  final data = await weatherService.getWeather('London');
  return data;
}
```

---

## 14:48:05 — Extracting Data from API
```dart
final jsonData = jsonDecode(response.body);
final temperature = jsonData['main']['temp'];
final description = jsonData['weather'][0]['description'];
final icon = jsonData['weather'][0]['icon'];
final forecast = (jsonData['list'] as List)
    .map((item) => ForecastItem.fromJson(item))
    .toList();
```

---

## 15:01:22 — Loading Indicator
```dart
// Circular progress
const CircularProgressIndicator()
CircularProgressIndicator(color: Colors.white)

// Linear progress
const LinearProgressIndicator()
```

---

## 15:06:55 — FutureBuilder Widget
```dart
FutureBuilder<WeatherData>(
  future: _weatherFuture,
  builder: (context, snapshot) {
    if (snapshot.connectionState == ConnectionState.waiting) {
      return const CircularProgressIndicator();
    }
    if (snapshot.hasError) {
      return Text('Error: ${snapshot.error}');
    }
    if (snapshot.hasData) {
      return WeatherDisplay(data: snapshot.data!);
    }
    return const Text('No data');
  },
)
```

---

## 15:19:28 — AsyncSnapshot
| Property | Meaning |
|----------|---------|
| `snapshot.connectionState` | `none`, `waiting`, `active`, `done` |
| `snapshot.hasData` | true if data is available |
| `snapshot.hasError` | true if an error occurred |
| `snapshot.data` | the resolved value (nullable) |
| `snapshot.error` | the error object |

---

## 15:39:42 — ListView.builder Widget
```dart
ListView.builder(
  itemCount: items.length,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text(items[index].name),
      subtitle: Text(items[index].description),
      leading: Icon(Icons.cloud),
      trailing: Text('${items[index].temp}°'),
      onTap: () {},
    );
  },
)
// .builder is lazy — only builds visible items (efficient for long lists)
```

---

## 15:50:23 — Date Formatting using intl
```yaml
dependencies:
  intl: ^0.19.0
```
```dart
import 'package:intl/intl.dart';

DateTime now = DateTime.now();
DateFormat('MMM dd, yyyy').format(now);        // "Feb 23, 2026"
DateFormat('EEEE').format(now);                // "Monday"
DateFormat('hh:mm a').format(now);             // "02:30 PM"
DateFormat('dd/MM/yyyy HH:mm').format(now);

// Parse
DateTime.parse('2026-02-23T14:30:00');
```

---

# PART 9: FLUTTER INTERNALS

## 16:05:35 — Layout Principle in Flutter Explained
- **Constraints go DOWN, sizes go UP, parent sets position.**
- Parent passes `BoxConstraints(minWidth, maxWidth, minHeight, maxHeight)` to child.
- Child decides its own size within those constraints.
- Parent positions the child.
- This is why some widgets expand, some shrink, and some cause errors when unconstrained.

---

## 16:10:57 — Flutter Behind the Scenes: 3 Trees & BuildContext
Flutter maintains 3 trees simultaneously:

| Tree | Role |
|------|------|
| **Widget Tree** | Immutable descriptions (what you write in Dart) |
| **Element Tree** | Live instances linking widget config to render objects; persists across rebuilds |
| **RenderObject Tree** | Handles layout, painting, hit testing |

- `BuildContext` is actually an `Element` — it knows where in the tree a widget lives.
- When you call `setState()`, Flutter diffs the widget tree, reuses elements where possible, and only updates what changed.
- This is why Flutter is fast even with many rebuilds.

---

# PART 10: SHOP APP

## 16:32:15 — Shop App Demo
A multi-screen e-commerce app. Covers: theming, navigation, state management (Provider), responsive UI.

---

## 16:33:32 — Project Setup (Fonts, Theme, ColorScheme)
```dart
// pubspec.yaml — fonts
flutter:
  fonts:
    - family: Lato
      fonts:
        - asset: assets/fonts/Lato-Regular.ttf
        - asset: assets/fonts/Lato-Bold.ttf
          weight: 700

// main.dart — theme
ThemeData(
  colorScheme: ColorScheme.fromSeed(seedColor: Colors.orange),
  useMaterial3: true,
  fontFamily: 'Lato',
  textTheme: const TextTheme(
    headlineLarge: TextStyle(fontSize: 28, fontWeight: FontWeight.bold),
  ),
  cardTheme: CardTheme(elevation: 4, shape: RoundedRectangleBorder(...)),
)
```

---

## 16:52:23 — SafeArea Widget
```dart
SafeArea(
  child: Column(...),
)
// Adds padding to avoid notches, status bar, home indicator
```

---

## 16:59:26 — Expanded Widget
```dart
Row(
  children: [
    Expanded(
      flex: 2,   // takes 2/3 of space
      child: Text('Left'),
    ),
    Expanded(
      flex: 1,   // takes 1/3 of space
      child: Text('Right'),
    ),
  ],
)
// Expanded forces child to fill available space along main axis
// Flexible is like Expanded but allows child to be smaller
```

---

## 17:14:16 — Chip Widget
```dart
FilterChip(
  label: Text('Electronics'),
  selected: _selectedCategory == 'Electronics',
  onSelected: (bool selected) {
    setState(() => _selectedCategory = selected ? 'Electronics' : null);
  },
)

Chip(label: Text('New'))
ActionChip(label: Text('View All'), onPressed: () {})
InputChip(label: Text('Tag'), onDeleted: () {}, avatar: Icon(Icons.tag))
```

---

## 17:30:17 — How Theming Works Behind the Scenes (InheritedWidget)
- `Theme`, `MediaQuery`, `Navigator` all use `InheritedWidget` under the hood.
- `InheritedWidget` propagates data down the tree efficiently.
- Widgets that read from an `InheritedWidget` rebuild only when that data changes.
- `Theme.of(context)` walks up the tree to find the nearest `Theme` widget.

---

## 17:38:40 — Images and Dummy Data
```dart
// Network image
Image.network(
  'https://example.com/image.jpg',
  width: 200,
  height: 200,
  fit: BoxFit.cover,
  loadingBuilder: (ctx, child, progress) =>
      progress == null ? child : CircularProgressIndicator(),
  errorBuilder: (ctx, err, stack) => Icon(Icons.broken_image),
)

// Asset image
Image.asset('assets/images/product.png', fit: BoxFit.contain)

// Declare in pubspec.yaml:
// flutter:
//   assets:
//     - assets/images/

// BoxFit values
BoxFit.cover    // fill, may crop
BoxFit.contain  // fit inside, may letterbox
BoxFit.fill     // stretch to fill
BoxFit.fitWidth / .fitHeight
```

---

## 18:37:33 — Navigation & Routing
```dart
// Push (go to new screen)
Navigator.of(context).push(
  MaterialPageRoute(builder: (_) => const DetailsPage()),
);

// Pop (go back)
Navigator.of(context).pop();
Navigator.of(context).pop(result);   // return data to previous screen

// Push and remove previous
Navigator.of(context).pushReplacement(
  MaterialPageRoute(builder: (_) => const HomePage()),
);

// Named routes
Navigator.of(context).pushNamed('/details', arguments: {'id': 42});

// Receive result
final result = await Navigator.of(context).push(...);
```

---

## 18:48:20 — How Navigator Works Behind the Scenes
- Navigator maintains a **stack** of Routes (pages).
- Each Route can push/pop from the stack.
- `BuildContext` is key — Navigator.of(context) finds the nearest Navigator in the tree.
- State management: each Route is a widget with its own state; popping removes it and its state.
- This is why you need global state management (Provider, Riverpod, Bloc) for data shared across screens.

---

## 18:59:59 — BottomNavigationBar Widget
```dart
BottomNavigationBar(
  currentIndex: _currentIndex,
  onTap: (index) => setState(() => _currentIndex = index),
  items: const [
    BottomNavigationBarItem(icon: Icon(Icons.home), label: 'Home'),
    BottomNavigationBarItem(icon: Icon(Icons.shop), label: 'Shop'),
    BottomNavigationBarItem(icon: Icon(Icons.shopping_cart), label: 'Cart'),
  ],
  selectedItemColor: Colors.orange,
  type: BottomNavigationBarType.fixed,
)
```

---

## 19:09:10 — IndexedStack Widget
```dart
IndexedStack(
  index: _currentIndex,  // only shows the child at this index
  children: const [
    HomePage(),
    ShopPage(),
    CartPage(),
  ],
)
// Unlike PageView, IndexedStack preserves state of all pages
// All children are built at once, but only one is visible
```

---

## 19:11:59 — Designing Cart Page (ListTile Widget)
```dart
ListTile(
  leading: CircleAvatar(backgroundImage: NetworkImage(product.imageUrl)),
  title: Text(product.name),
  subtitle: Text('\$${product.price}'),
  trailing: IconButton(icon: Icon(Icons.delete), onPressed: () {}),
  onTap: () {},
  contentPadding: EdgeInsets.symmetric(horizontal: 16),
)
```

---

## 19:22:38 — State Management with Provider & SnackBar
```yaml
dependencies:
  provider: ^6.1.2
```
```dart
// Model (ChangeNotifier)
class CartProvider extends ChangeNotifier {
  final List<Product> _items = [];

  List<Product> get items => List.unmodifiable(_items);
  int get count => _items.length;
  double get total => _items.fold(0, (sum, item) => sum + item.price);

  void addItem(Product product) {
    _items.add(product);
    notifyListeners();   // triggers rebuild in all listeners
  }

  void removeItem(Product product) {
    _items.remove(product);
    notifyListeners();
  }
}

// Provide at top of tree
MultiProvider(
  providers: [
    ChangeNotifierProvider(create: (_) => CartProvider()),
    ChangeNotifierProvider(create: (_) => ProductsProvider()),
  ],
  child: const MyApp(),
)

// Read (no rebuild)
context.read<CartProvider>().addItem(product);

// Watch (rebuilds on change)
final cart = context.watch<CartProvider>();

// Select (rebuild only when specific field changes)
final count = context.select<CartProvider, int>((c) => c.count);
```

**SnackBar:**
```dart
ScaffoldMessenger.of(context).showSnackBar(
  SnackBar(
    content: const Text('Item added to cart'),
    action: SnackBarAction(label: 'UNDO', onPressed: () {}),
    duration: const Duration(seconds: 3),
  ),
);
```

---

## 19:51:20 — Dialogs in Flutter
```dart
// Alert Dialog
showDialog(
  context: context,
  builder: (ctx) => AlertDialog(
    title: const Text('Confirm Delete'),
    content: const Text('Are you sure you want to remove this item?'),
    actions: [
      TextButton(
        onPressed: () => Navigator.of(ctx).pop(false),
        child: const Text('Cancel'),
      ),
      ElevatedButton(
        onPressed: () => Navigator.of(ctx).pop(true),
        child: const Text('Delete'),
      ),
    ],
  ),
);

// Bottom Sheet
showModalBottomSheet(
  context: context,
  shape: const RoundedRectangleBorder(
    borderRadius: BorderRadius.vertical(top: Radius.circular(20)),
  ),
  builder: (ctx) => SizedBox(
    height: 300,
    child: Column(children: [...]),
  ),
);
```

---

# PART 11: RESPONSIVE UI

## 20:09:55 — Flutter Responsive UI (MediaQuery)
```dart
final size = MediaQuery.of(context).size;
final width = size.width;
final height = size.height;

final padding = MediaQuery.of(context).padding;     // system padding (notch, status bar)
final viewInsets = MediaQuery.of(context).viewInsets; // keyboard insets
final orientation = MediaQuery.of(context).orientation;

// Responsive sizing
Container(
  width: width * 0.9,          // 90% of screen width
  height: height * 0.3,
)

// Breakpoints
bool isTablet = width >= 600;
bool isDesktop = width >= 1200;
```

---

## 20:33:15 — InheritedWidget vs InheritedModel
| | `InheritedWidget` | `InheritedModel` |
|---|---|---|
| Rebuild trigger | Any change to the widget | Only when the specific aspect changes |
| Use case | Simple, single data blob | Complex data with multiple independent parts |
| Performance | Can over-rebuild | More granular rebuild control |
- `MediaQuery` uses `InheritedModel` — widgets only rebuild when their specific aspect (size, padding, etc.) changes.

---

## 20:35:03 — Responsive UI with LayoutBuilder
```dart
LayoutBuilder(
  builder: (context, constraints) {
    if (constraints.maxWidth > 600) {
      return const TabletLayout();
    }
    return const MobileLayout();
  },
)
// LayoutBuilder gives you the PARENT's constraints, not the full screen size
```

---

## 20:42:01 — MediaQuery vs LayoutBuilder
| | MediaQuery | LayoutBuilder |
|---|---|---|
| What it measures | Full screen dimensions | Parent widget's constraints |
| Best for | Screen-level decisions | Widget-level adaptations |
| Rebuilds when | Screen resizes | Parent constraints change |
- Use `MediaQuery` at the screen/page level.
- Use `LayoutBuilder` inside reusable widgets.

---

## 20:45:48 — Flutter Widget Sizing Summary

| Widget | Behavior |
|--------|----------|
| `Expanded` | Takes ALL remaining space on main axis |
| `Flexible` | Takes up to available space (child can be smaller) |
| `SizedBox` | Fixed size / spacer |
| `FractionallySizedBox` | Fraction of parent's size |
| `AspectRatio` | Maintains width:height ratio |
| `ConstrainedBox` | Adds min/max constraints |
| `UnconstrainedBox` | Removes constraints (careful — can overflow) |
| `FittedBox` | Scales/positions child within itself |
| `IntrinsicWidth/Height` | Sizes to child's "natural" size (expensive) |

---

## 20:46:53 — Conclusion
**What you've covered:**
- Dart: full language fundamentals, OOP, async, advanced features.
- Flutter: widget system, state management (setState + Provider), navigation, HTTP, theming, responsive UI.

**Next steps to deepen the stack:**
- **State management**: Riverpod (modern, compile-safe) or Bloc (enterprise-scale).
- **Local persistence**: Hive, Isar, SQLite via `sqflite`.
- **Backend integration**: Firebase, Supabase, custom REST/GraphQL.
- **Testing**: `flutter_test`, `mockito`, integration tests.
- **Architecture patterns**: Clean Architecture, Repository pattern, MVVM.
- **CI/CD**: Fastlane, GitHub Actions for automated builds.
- **Publishing**: App Store / Play Store deployment.

---
*Notes compiled from: The Complete Dart & Flutter Developer Course by Rivaan Ranawat*