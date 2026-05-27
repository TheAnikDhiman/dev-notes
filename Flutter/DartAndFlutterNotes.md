# The Complete Dart & Flutter Developer Course
### By Rivaan Ranawat | Beginner to Advanced
---

## Table of Contents
1. [PART 1 — Dart Fundamentals](#part-1--dart-fundamentals)
2. [PART 2 — Object Oriented Programming](#part-2--object-oriented-programming)
3. [PART 3 — Collections & Data Structures](#part-3--collections--data-structures)
4. [PART 4 — Async Dart](#part-4--async-dart)
5. [PART 5 — Advanced Dart Features](#part-5--advanced-dart-features)
6. [PART 6 — Flutter Setup & Basics](#part-6--flutter-setup--basics)
7. [PART 7 — Flutter Core Widgets](#part-7--flutter-core-widgets)
8. [PART 8 — Currency Converter App](#part-8--currency-converter-app)
9. [PART 9 — Weather App](#part-9--weather-app)
10. [PART 10 — Flutter Internals](#part-10--flutter-internals)
11. [PART 11 — Shop App](#part-11--shop-app)
12. [PART 12 — Responsive UI](#part-12--responsive-ui)

---

# PART 1 — Dart Fundamentals

---

## What is Dart?

Dart is a programming language made by Google. It is the language you write Flutter apps in.  
Think of Dart as the pen — Flutter is the paper you draw on.

- Dart is **strongly typed** — every variable has a fixed type (int, String, etc.)
- It is **object-oriented** — everything is an object
- Dart compiles to **native machine code** on mobile/desktop, and to **JavaScript** for the web
- This is why Flutter apps feel fast — they run as real compiled code, not interpreted scripts
- Dart has **sound null safety** — the compiler guarantees a variable can't be null unless you say so
- Two compilation modes:
  - **JIT** (Just-in-Time) — used during development. Enables hot reload
  - **AOT** (Ahead-of-Time) — used for production builds. Gives fast performance

---

## Dart SDK

SDK = Software Development Kit. It includes the compiler, runtime, and built-in libraries.

- When you install Flutter, Dart comes bundled with it — no separate install needed
- `dart --version` → check if Dart is installed
- `dart run filename.dart` → run a Dart file from terminal
- **DartPad** (`dartpad.dev`) → browser-based editor, useful for quick practice without setup

---

## main() Function & print()

Every Dart program starts from `main()`. Without it, the program doesn't know where to begin.

- `void main()` → entry point of every Dart program
- `void` means the function returns nothing
- `print()` → outputs to the console
- **String interpolation**: embed variables or expressions inside a string using `$` or `${}`
  - `$variable` → for simple variables
  - `${expression}` → for expressions like `${2 + 2}`

```dart
void main() {
  print('Hello');          // prints text
  print('Sum: ${2 + 2}'); // prints: Sum: 4
}
```

---

## Comments

Comments are notes for the programmer — the compiler ignores them completely.

- `//` → single line comment
- `/* ... */` → multi-line comment
- `///` → documentation comment (shown in IDE tooltips as help text)
- Always use comments to explain *why* something is done, not *what* is obvious

---

## Variables & Data Types

A variable is a named box that stores a value. Every box has a type.

**Core types in Dart:**
- `int` → whole numbers (1, 42, -7)
- `double` → decimal numbers (3.14, -0.5)
- `String` → text ('hello', "world")
- `bool` → true or false only
- `num` → parent type of both int and double

**How to declare:**
- `int age = 25;` → explicit type (clear and readable)
- `var city = 'Delhi';` → Dart infers the type (city is locked as String)
- `dynamic x = 5;` → can hold any type at any time (avoid — loses type safety)

**`final` vs `const`:**
- `final` → set once at runtime, can't change after that
  - Use when value is known only when the app runs (e.g., current time, user input)
- `const` → set at compile time, never changes ever
  - Use for truly fixed values like `const double pi = 3.14159`
- Key difference: `final DateTime now = DateTime.now()` works. `const DateTime now = DateTime.now()` does NOT — because DateTime.now() is not known at compile time

---

## Null Safety

Null safety means the compiler protects you from null errors (the most common crash source).

- By default, no variable can be null in Dart
- If you WANT a variable to be nullable, add `?` → `String? nickname`
- **`?.` (null-safe access)** → call only if not null, else return null
- **`??` (null coalescing)** → return a fallback if the left side is null
- **`!` (null assertion)** → "I promise this is not null" — crashes if you're wrong

```dart
String? name;
print(name?.length);     // safe — returns null, not a crash
print(name ?? 'Guest'); // if name is null, print 'Guest'
name!.length;            // force it — dangerous if name is actually null
```

> Rule: Use `?` when null is genuinely possible. Use `??` to provide fallbacks. Avoid `!` unless you're certain.

---

## Strings Deep Dive

Strings are text values wrapped in quotes.

- Single quotes `'text'` and double quotes `"text"` are both valid — pick one and be consistent
- Triple quotes `'''...'''` → multi-line strings (preserves line breaks)
- Strings are **immutable** — you can't change a string, you create a new one
- Common string methods:
  - `.length` → number of characters
  - `.toUpperCase()` / `.toLowerCase()`
  - `.contains('x')` → true/false
  - `.replaceAll('old', 'new')`
  - `.split(',')` → splits into a List
  - `.trim()` → removes leading/trailing spaces
  - `.isEmpty` / `.isNotEmpty`
  - `.startsWith('H')`

---

## Numbers

- `int.parse('42')` → converts String to int
- `double.parse('3.14')` → converts String to double
- `42.toString()` → converts number to String
- `.toStringAsFixed(2)` → formats a double to 2 decimal places (useful for displaying prices)
- `.round()` / `.ceil()` / `.floor()` → rounding methods

---

## Operators

**Arithmetic:** `+  -  *  /  ~/  %`
- `/` always returns double
- `~/` is integer division (drops decimal)
- `%` is remainder (e.g., 10 % 3 = 1)

**Comparison:** `==  !=  >  <  >=  <=` → always return bool

**Logical:** `&&` (and) `||` (or) `!` (not)

**Assignment:** `=  +=  -=  *=  /=`

**Null-aware:**
- `??` → return right side if left is null
- `??=` → assign only if current value is null
- `?.` → safely access member if not null

**Type check:**
- `is` → checks type: `x is String`
- `is!` → checks not of type: `x is! int`

---

## Control Flow

**if / else if / else:**
- Used when you need to make decisions based on conditions
- Only the first matching block runs

**Ternary operator:** → shorthand for simple if-else
```dart
String label = score >= 50 ? 'Pass' : 'Fail';
```

**switch statement:**
- Use when you're checking one variable against many possible values
- Always use `break` or the case falls through to the next one
- Dart 3+ has **switch expressions** — cleaner one-liner version

---

## Loops

Loops repeat a block of code multiple times.

**for loop** — use when you know how many times to repeat
**for-in loop** — use when iterating over a collection (list, set, etc.)
**while loop** — use when repeating until a condition becomes false
**do-while loop** — same as while, but always runs at least once

**Loop control:**
- `break` → exit the loop entirely
- `continue` → skip current iteration, go to next

---

## Functions

A function is a named block of reusable code.

- **Return type** goes before the name: `int add(int a, int b) { return a + b; }`
- **Arrow function** → shorthand for single-expression functions: `int add(int a, int b) => a + b;`
- **Named parameters** → called by name, can have defaults: `greet({required String name, int age = 0})`
- **Optional positional parameters** → wrapped in `[]`: `String full(String first, [String? last])`
- **Functions are first-class objects** → you can pass a function as a parameter to another function
- **Anonymous functions** → functions without names, often used as callbacks: `(int x) => x * 2`
- **Higher-order functions** → functions that take or return other functions (like `.map()`, `.where()`)

> Key insight: In Dart, functions are values. You can store them in variables, pass them around, and return them.

---

# PART 2 — Object Oriented Programming

---

## What is OOP?

OOP (Object-Oriented Programming) is a way of writing code by modeling real-world things as objects.

- An **object** = data (properties) + behavior (methods) bundled together
- A **class** = the blueprint/template for creating objects
- An **instance** = one specific object created from that class
- Example: `Car` is a class. Your specific car (red Honda) is an instance.

---

## Classes & Constructors

A class defines what properties and methods an object will have.

- **Instance variables** → properties of the object (e.g., name, age)
- **Constructor** → special method called when creating an object. Initializes properties.
- `Person(this.name, this.age)` → shorthand constructor — automatically assigns params to fields
- **Named constructor** → alternative way to create an object with different logic: `Person.anonymous()`
- **Factory constructor** → returns an existing or computed instance instead of always creating new

```dart
class Person {
  String name;
  int age;
  Person(this.name, this.age);     // constructor
  void greet() => print('Hi, I am $name');
}

var p = Person('Anik', 20);
p.greet();
```

**Getters & Setters:**
- `get` → computed property (read-only calculated value)
- `set` → controlled write access (you can add validation)

---

## Inheritance (extends)

Inheritance lets one class reuse the properties and methods of another class.

- Use `extends` → "this class IS a kind of that class"
- `super` → refers to the parent class. Used to call parent constructor or methods
- `@override` → marks that you're replacing the parent's method with your own
- Dart supports only **single inheritance** — a class can extend only one other class

```dart
class Animal { void speak() => print('...'); }
class Dog extends Animal {
  @override
  void speak() => print('Woof!');
}
```

> Analogy: Animal is the parent. Dog is a child. Dog inherits everything and can override specific things.

---

## implements (Interfaces)

`implements` is a contract — "this class promises to have ALL these methods."

- Unlike `extends`, you don't inherit any implementation — you must write everything yourself
- A class can implement **multiple** interfaces (unlike extends which only allows one)
- Any class can act as an interface in Dart — there's no separate `interface` keyword

```dart
abstract class Flyable { void fly(); }
class Bird implements Flyable {
  @override
  void fly() => print('Bird flies');
}
```

> Use `implements` when you want to guarantee a class has a specific set of methods, but different implementations.

---

## Abstract Classes

An abstract class is a class that cannot be created directly — it's only a template.

- Use `abstract` keyword
- Abstract methods have no body — subclasses MUST implement them
- Can also have regular (concrete) methods that subclasses inherit
- Difference from `implements`: extending abstract class gives you the concrete methods for free. Implementing it makes you write everything.

---

## The 4 Pillars of OOP

**1. Encapsulation** — hide internal details, expose only what's needed
- In Dart, prefix with `_` to make something private to the file: `_balance`
- Use getters/setters to control how private data is accessed

**2. Inheritance** — one class reuses another's code via `extends`
- Promotes code reuse, reduces duplication

**3. Polymorphism** — same method name, different behavior depending on the actual object
- You can call the same method on different subclass objects and get different results
- Decided at runtime, not compile time

**4. Abstraction** — hide the "how", show only the "what"
- You call `car.start()` without caring about the engine internals
- Achieved via abstract classes and interfaces

---

## Mixins

A mixin adds capabilities to a class without using inheritance.

- Use `mixin` to define it, `with` to apply it
- Mixins can't have constructors
- You can apply multiple mixins to one class
- Think of mixins as "plug-ins" you attach to a class

```dart
mixin Logger { void log(String msg) => print('[LOG] $msg'); }

class UserService with Logger {
  void create() => log('User created');
}
```

> Use case: You want Logger behavior in 10 different classes. Instead of inheriting from a Logger class (which limits you to one parent), you mix it in.

---

## Class Modifiers (Dart 3+)

| Modifier | Meaning |
|----------|---------|
| `final class` | Can't be extended or implemented outside the file |
| `base class` | Can extend but NOT implement |
| `interface class` | Can implement but NOT extend |
| `sealed class` | All subclasses must be in same file — allows exhaustive switch |
| `abstract` | Can't be instantiated directly |

- **Sealed classes** are very useful with pattern matching — the compiler knows all possible subtypes

---

# PART 3 — Collections & Data Structures

---

## Lists

A List is an ordered collection of items. Like an array in other languages.

- Ordered → items maintain their insertion order
- Can contain duplicates
- Zero-indexed → first item is at index 0
- Generic type: `List<int>` means only integers allowed
- Common operations: `add()`, `remove()`, `contains()`, `length`, `sort()`, `map()`, `where()`, `reduce()`
- `map()` → transform each item and return a new iterable
- `where()` → filter items by condition
- `reduce()` → combine all items into one value

> Think of a List like a numbered shelf — order matters, duplicates allowed.

---

## Sets

A Set is an unordered collection where every item is unique.

- No duplicates — adding the same item twice is silently ignored
- Unordered — no guarantee of order
- Fast `contains()` check (O(1) — constant time regardless of set size)
- Set operations: `union()`, `intersection()`, `difference()`
- Use `{}` syntax: `Set<String> tags = {'flutter', 'dart'}`

> Use Set when: uniqueness matters. Use List when: order or duplicates matter.

---

## Maps

A Map is a collection of key-value pairs.

- Each key is unique. Values can repeat.
- Like a dictionary — look up a value by its key
- `map['key']` → returns the value (or null if key doesn't exist, no crash)
- `containsKey()` / `containsValue()` → check existence
- `entries` → iterate over key-value pairs together
- `putIfAbsent()` → only adds if key not already there

> Analogy: A Map is like a contact book — names (keys) are unique, phone numbers (values) are the data.

---

## Enums

Enums define a fixed set of named constants.

- Use when a variable can only be one of a defined set of values
- Much safer than using raw strings or ints ("Monday" vs `Day.monday`)
- Dart 3 enhanced enums — can have properties and methods
- `.name` → string version of the enum value
- `.index` → its position in the declaration order
- Works great with switch — IDE warns if you miss a case

---

# PART 4 — Async Dart

---

## Exception Handling

Exceptions are unexpected errors that happen at runtime. If uncaught, they crash the app.

- `try` block → code that might throw an error
- `on SpecificException` → catch a specific type of error
- `catch (e, stack)` → catch any error and get the error object + stack trace
- `finally` → always runs whether or not an error occurred (use for cleanup)
- You can create custom exceptions by implementing the `Exception` class

> Rule: Always handle exceptions in async code (network calls, file reads, parsing). If something can fail, it probably will at some point.

---

## Futures (Async / Await)

A Future represents a value that doesn't exist yet — it'll be available sometime in the future.

- Think of a Future like a "promise" — "I'll give you the result when I'm done"
- `async` keyword → marks a function as asynchronous. It will return a Future.
- `await` keyword → pause here and wait for the Future to complete before continuing
- A function marked `async` ALWAYS returns a Future, even if you return a plain value
- `Future.wait([f1, f2, f3])` → run multiple futures at the same time and wait for ALL to finish
- `Future.delayed(Duration(seconds: 2))` → creates an artificial delay (useful for testing)

> Why does this matter? Network calls, database reads, file access — all take time. If you don't use async, your UI would freeze while waiting. async/await lets the app keep running while waiting for data.

**`.then()` vs `async/await`:**  
Both work. `async/await` is cleaner and easier to read — prefer it.

---

## Streams

A Stream is a sequence of asynchronous events over time — like a river of data.

- While a Future gives you ONE value when done, a Stream gives you MULTIPLE values over time
- `yield` → sends a value into the stream (used in `async*` functions)
- `listen()` → subscribes to the stream and reacts to each value
- `cancel()` → stops listening (important to avoid memory leaks)
- **Single-subscription stream** → only one listener at a time (most common)
- **Broadcast stream** → multiple listeners allowed (e.g., button click events)
- StreamController → manually creates and controls a stream

| | Future | Stream |
|--|--------|--------|
| Values | One | Many (over time) |
| Ends | Once | When done or cancelled |
| Use case | HTTP response | Sensor data, live feeds, UI events |

---

# PART 5 — Advanced Dart Features

---

## Records (Dart 3+)

Records are lightweight, immutable data containers to group related values together.

- Like a tuple in other languages
- No need to create a class just to return two values from a function
- Two styles: positional `(10, 20)` and named `(x: 10, y: 20)`
- Access positional fields with `.$1`, `.$2`; named fields with `.fieldName`
- Supports **destructuring** — unpack multiple return values cleanly

> Use case: Want to return both a username AND an age from a function? Return a Record instead of creating a class.

---

## Pattern Matching (Dart 3+)

Pattern matching lets you check a value's shape and extract data in one step.

- Works with `switch` expressions and statements
- You can match on types, list structure, map keys, and object shapes
- **Guard clauses** → add `when` to add an extra condition inside a case
- Makes complex conditional logic much more readable

---

## Extensions

Extensions add new methods to an existing class without modifying it or subclassing it.

- You can extend built-in types like String, int, List
- Define with `extension NameHere on TypeHere { ... }`
- The methods you add are available everywhere you use that type
- Keeps code clean — instead of utility functions scattered everywhere, attach them to the type

```dart
extension on String {
  bool get isEmail => contains('@') && contains('.');
}
'test@gmail.com'.isEmail  // true — feels natural
```

---

# PART 6 — Flutter Setup & Basics

---

## What is Flutter?

Flutter is Google's UI toolkit for building apps from a single codebase.

- Write once → deploy to iOS, Android, Web, Windows, macOS, Linux
- Flutter renders its own UI using the **Skia** / **Impeller** graphics engine
- This means Flutter does NOT use native UI components — it draws everything itself
- Why this matters: pixel-perfect UI on all platforms, consistent look everywhere
- Core advantage: **Hot Reload** — see changes instantly without restarting the app

---

## Installing Flutter

- Download from `flutter.dev` and add to PATH
- Or install via Homebrew on Mac: `brew install flutter`
- `flutter doctor` → checks your setup and tells you what's missing
- `flutter doctor -v` → verbose output with more details

---

## Setting Up Editors

**Android Studio:**
- Install Flutter + Dart plugins from Settings → Plugins
- Accept SDK licenses: `flutter doctor --android-licenses`
- Create an AVD (Android Virtual Device) from Device Manager

**VS Code:**
- Install Flutter extension (Dart comes automatically with it)
- Key shortcuts:
  - `Ctrl+Shift+P` → Command Palette (access all Flutter commands)
  - Hot Reload → press `r` in terminal, or just save the file
  - Hot Restart → `R` → resets app state

---

## Flutter Project Structure

```
my_app/
├── lib/           → YOUR CODE IS HERE
│   └── main.dart  → Entry point of the app
├── pubspec.yaml   → Dependencies, fonts, assets (like package.json)
├── android/       → Android configs (rarely touched)
├── ios/           → iOS configs (rarely touched)
└── test/          → Test files
```

- 90% of your work happens inside `lib/`
- `pubspec.yaml` is where you add packages and declare assets/fonts
- `flutter pub get` → downloads packages listed in pubspec.yaml

---

## Running Flutter App

```bash
flutter run              # run on connected device/emulator
flutter run -d chrome    # run as web app
flutter build apk        # build Android APK
flutter build ios        # build iOS
```

---

## Importing Packages

- `import 'package:flutter/material.dart'` → Material Design widgets (most common)
- `import 'package:flutter/cupertino.dart'` → iOS-style widgets
- `import 'package:flutter/widgets.dart'` → base Flutter widgets only (no theme)
- For your own files, use relative imports: `import '../widgets/my_button.dart'`

---

## runApp() Function

`runApp()` takes a Widget and makes it the root of the entire app.

- Called inside `main()` — it bootstraps the whole Flutter app
- Whatever you pass to `runApp()` becomes the top of the widget tree
- Usually you pass a `MaterialApp` or `CupertinoApp` here

---

## What are Widgets?

In Flutter, EVERYTHING is a widget — text, buttons, layouts, padding, animation, the app itself.

- A widget is an **immutable description of a part of the UI**
- Widgets don't draw anything directly — they describe what to draw
- Flutter reads the widget descriptions and builds the actual UI
- Because widgets are just Dart objects (not drawn elements), creating/recreating them is cheap
- Analogy: A widget is like a blueprint. The Flutter engine builds the actual building.

**Two types:**
- **StatelessWidget** → UI that never changes after being built
- **StatefulWidget** → UI that can change based on data

---

## What is State?

State is data that can change over time and cause the UI to update.

- Examples of state: counter value, text typed by user, data fetched from API, toggle on/off
- When state changes → Flutter re-calls `build()` → UI updates
- State lives inside a `State` class (for StatefulWidget)

---

## StatelessWidget

Used when your widget never needs to update based on changing data.

- Extends `StatelessWidget`
- Has only a `build()` method that returns the UI
- Once built, it stays the same unless the parent widget passes different props
- Always mark as `const` when all its properties are constants → better performance

---

## StatefulWidget

Used when your widget needs to react to changing data.

- Consists of TWO classes: the Widget class (immutable config) + the State class (mutable data)
- State class holds all the variables that can change
- `setState(() { ... })` → wraps your state change + tells Flutter to rebuild the UI
- Access the widget's properties inside State using `widget.propertyName`
- Never call setState in `build()` — creates infinite loop

```dart
class _PageState extends State<MyPage> {
  int count = 0;
  // change count → call setState → build() is called again → UI shows new value
}
```

---

## Widget Lifecycle

The sequence of events from when a StatefulWidget is created to when it's destroyed:

1. **Constructor** → Widget is created with initial config
2. **createState()** → Creates the associated State object
3. **initState()** → Called ONCE when the widget enters the tree. Use for one-time setup (fetch data, init controllers)
4. **build()** → Called every time the widget needs to render. Can be called many times.
5. **setState()** → Marks widget as needing rebuild → triggers build() again
6. **didUpdateWidget()** → Called when parent passes new config
7. **didChangeDependencies()** → Called when an InheritedWidget dependency changes
8. **deactivate()** → Widget temporarily removed from tree
9. **dispose()** → Widget permanently destroyed. Clean up here (cancel subscriptions, dispose controllers)

> Rule: initState for setup, dispose for cleanup — always.

---

## BuildContext

BuildContext is a reference to a widget's position in the widget tree.

- Every widget's `build()` receives a `context` parameter
- Used to look up things in the tree: `Theme.of(context)`, `MediaQuery.of(context)`, `Navigator.of(context)`
- Context knows where you are in the tree, so it can walk up and find what you need
- NEVER store context and use it after an async gap — the widget might be gone by then
- Always check `context.mounted` before using context after an await

---

## Material vs Cupertino Design

**Material Design** → Google's design language. Looks like Android apps.  
**Cupertino Design** → Apple's design language. Looks like iOS apps.

- Flutter supports both — you can even mix them
- `MaterialApp` → top-level widget for Material apps
- `CupertinoApp` → top-level widget for iOS-style apps
- Most apps start with MaterialApp (wider widget coverage, better documentation)

---

## Widget Tree

Widgets are nested inside each other, forming a tree.

- Parent widgets pass constraints DOWN to children
- Children report their size UP to the parent
- Parent decides the position of the child
- Flutter walks this tree to build and update the UI
- Extract large subtrees into their own widget classes for readability and reuse

```
MaterialApp → Scaffold → Column → [Text, Button, TextField]
```

---

# PART 7 — Flutter Core Widgets

---

## Text Widget

Displays a string on screen. The most basic display widget.

- Style it with `TextStyle`: fontSize, fontWeight, color, letterSpacing, fontStyle, decoration
- `textAlign` → align text within its container
- `maxLines` → limit lines shown
- `overflow: TextOverflow.ellipsis` → shows `...` when text doesn't fit
- `RichText` → for text with MULTIPLE different styles in the same paragraph
- `Theme.of(context).textTheme.headlineMedium` → use theme text styles for consistency

---

## Colors

- `Colors.blue` → predefined Material colors
- `Colors.blue[300]` → shade of a color (50 is lightest, 900 is darkest)
- `Color(0xFF2196F3)` → custom color in hex (0xFF = fully opaque)
- `Colors.black.withOpacity(0.5)` → semi-transparent
- **Best practice**: prefer `Theme.of(context).colorScheme.primary` over hardcoded colors → respects light/dark mode and theming

---

## Container Widget

The most versatile single-child widget. Like a `<div>` in HTML.

- Can have: width, height, padding, margin, background color, border, border radius, shadow, gradient
- `BoxDecoration` → full decoration: color, borderRadius, border, boxShadow, gradient
- If you only need padding → use `Padding` widget (lighter)
- If you only need color → use `ColoredBox` (even lighter)
- Container tries to be as big as its parent if no child. As small as its child if child exists.

**Padding vs Margin:**
- `padding` → space INSIDE (between border and content)
- `margin` → space OUTSIDE (between container and its surroundings)

---

## Column and Row Widgets

Column → arranges children vertically. Row → arranges children horizontally.

**Key properties (same for both):**
- `mainAxisAlignment` → alignment along the main axis (Column: vertical, Row: horizontal)
  - `start`, `center`, `end`, `spaceBetween`, `spaceAround`, `spaceEvenly`
- `crossAxisAlignment` → alignment on the opposite axis
  - `start`, `center`, `end`, `stretch`
- `mainAxisSize: MainAxisSize.min` → shrink to fit children (instead of taking all available space)
- Use `SizedBox(height: 16)` or `SizedBox(width: 16)` as spacers between children

> Column's main axis is vertical. Row's main axis is horizontal. Keep this mental model clear.

---

## Scaffold Widget

Scaffold provides the basic visual structure of a screen.

- Every screen in a Flutter app typically has ONE Scaffold
- Built-in slots: `appBar`, `body`, `floatingActionButton`, `drawer`, `bottomNavigationBar`
- `backgroundColor` → sets the screen background color
- Think of Scaffold as the frame of a page

---

## AppBar Widget

The top navigation bar of a screen.

- `title` → widget shown in the center/left of the bar
- `centerTitle: true` → center the title
- `leading` → widget on the left (usually back button or menu icon)
- `actions` → list of widgets on the right (search, more options, etc.)
- `elevation` → shadow below the bar (0 = flat, no shadow)
- `bottom` → space to add a TabBar below the AppBar

---

## TextField Widget

The primary input widget for user text entry.

- Attach a `TextEditingController` to read what the user typed
- `keyboardType` → what keyboard layout to show (number, email, text, etc.)
- `obscureText: true` → for password fields (hides characters)
- `onChanged` → callback fires on every keystroke
- `onSubmitted` → callback fires when user presses Enter/Done
- `InputDecoration` → controls the visual appearance: hint text, label, icons, border, fill color
- **IMPORTANT**: Always call `controller.dispose()` in `dispose()` → prevents memory leaks

---

## Buttons in Flutter

**TextButton** → flat, no background, no elevation. Good for secondary actions.  
**ElevatedButton** → has a filled background with elevation. Good for primary actions.  
**OutlinedButton** → has a border outline but no fill. Good for secondary/cancel actions.  
**IconButton** → just an icon, tappable. Used in AppBars, lists.  
**FloatingActionButton (FAB)** → floating circle button, usually one per screen.

- All buttons: `onPressed: null` → disables the button automatically
- All buttons have `.styleFrom()` for styling: `backgroundColor`, `foregroundColor`, `shape`, `padding`
- `ElevatedButton.icon()` → button with an icon AND text combined

---

## Padding Widget

Adds space around a single child widget.

- Simpler and cheaper than Container when you ONLY need padding
- Use `EdgeInsets` for the padding value:
  - `.all(16)` → same on all 4 sides
  - `.symmetric(horizontal: 24, vertical: 12)` → different horizontal/vertical
  - `.only(left: 8, top: 4)` → specific sides only
  - `.fromLTRB(left, top, right, bottom)` → all 4 manually

---

## GestureDetector & InkWell

These detect taps and other gestures on any widget.

**GestureDetector:**
- Wraps any widget to detect: `onTap`, `onLongPress`, `onDoubleTap`, `onPanUpdate`, etc.
- No visual feedback (no ripple or animation)

**InkWell:**
- Material-style ripple effect when tapped
- Use this inside a Scaffold/Material for proper visual feedback
- Add `borderRadius` to make the ripple match rounded corners

> Rule: Use InkWell for list items and cards where you want a ripple. GestureDetector for custom gestures or when visual feedback isn't needed.

---

## Card Widget

A Material card — white surface with elevation and rounded corners.

- `elevation` → how much shadow (depth effect)
- `shape` → usually `RoundedRectangleBorder`
- Cards don't have padding built in — wrap content in `Padding` inside the card

---

## Stack Widget

Places widgets on top of each other (Z-axis layering).

- Children later in the list appear on top
- Use `Positioned` widget inside Stack to place children at specific coordinates
- Use for: overlapping elements, image with text on top, floating badges

---

## SingleChildScrollView

Makes its content scrollable when it overflows the screen.

- Wraps a Column (or other widget) that might be taller than the screen
- `scrollDirection: Axis.vertical` (default) or `Axis.horizontal`
- Different from ListView — here the entire content is built at once (not lazy)

---

## ListView.builder

Efficiently renders a list with many items.

- `itemCount` → total number of items
- `itemBuilder` → function called with index, returns the widget for that item
- **Lazy** → only builds items currently visible on screen. Efficient for large lists.
- Difference from `Column`: Column builds ALL children at once. ListView.builder builds on demand.

**ListTile** → pre-built list item widget:
- `leading` → widget on the left (icon, avatar)
- `title` → main text
- `subtitle` → secondary text below title
- `trailing` → widget on the right (icon, text, checkbox)
- `onTap` → tap handler

---

## ClipRRect

Clips its child to a rounded rectangle — use to round the corners of images.

- `borderRadius: BorderRadius.circular(12)` → rounded corners
- Useful when wrapping `Image.network` to make it rounded

---

## BackdropFilter & ImageFilter

Creates a blur/frosted glass effect on widgets behind it.

- Must be placed inside a Stack — there must be something behind it to blur
- `ImageFilter.blur(sigmaX: 10, sigmaY: 10)` → controls blur intensity
- Use with a semi-transparent Container on top for the frosted glass look

---

## Expanded & Flexible

Both are used inside Row or Column to control how children share available space.

- **Expanded** → forces the child to fill ALL remaining space along the main axis
- **Flexible** → child can take UP TO the available space but can be smaller
- `flex` parameter → defines the ratio of space taken (flex: 2 takes twice as much as flex: 1)

---

## SafeArea

Pads content to avoid system UI overlaps (status bar, notch, home indicator).

- Always use SafeArea on screens that go edge-to-edge
- Prevents content from being hidden behind device notches or the bottom nav bar

---

## IndexedStack

Shows only one child at a time by index, but keeps all others alive in memory.

- Unlike PageView, ALL children are built — just only one is visible
- State is preserved when switching between pages
- Used with BottomNavigationBar to prevent state loss when switching tabs

---

## BottomNavigationBar

The bottom tab bar — lets users switch between main sections of the app.

- `currentIndex` → which tab is active
- `onTap` → callback when user taps a tab, gives you the new index
- `items` → list of `BottomNavigationBarItem` (icon + label for each tab)
- Use with IndexedStack to preserve state across tabs

---

## FutureBuilder

A widget that rebuilds itself based on the state of a Future.

- Used to show loading spinner while data is being fetched, then the actual data
- `future` → the Future to listen to
- `builder` → function that returns different widgets based on `snapshot.connectionState`
- `snapshot.connectionState` states: `waiting` → `done`
- `snapshot.hasData` → true when data is ready
- `snapshot.hasError` → true when an error occurred
- `snapshot.data` → the actual value (once done)

> Pattern: If waiting → show CircularProgressIndicator. If error → show error text. If data → show UI.

---

## StreamBuilder

Like FutureBuilder but for Streams — rebuilds every time the stream emits a new value.

- Same snapshot-based API as FutureBuilder
- `connectionState` adds `active` → means stream is live and emitting

---

## PlaceHolder Widget

A development helper widget — shows a box with an X inside.

- Use while building a UI to mark sections you haven't built yet
- Replace with real widgets once ready

---

## Chip Widget

Small, compact UI elements for tags, filters, selections.

- `Chip` → basic chip (read-only label)
- `FilterChip` → selectable chip (selected/unselected state)
- `ActionChip` → chip with a tap action
- `InputChip` → chip with a delete button

---

## Image Widget

Displays images from different sources.

- `Image.network(url)` → load from internet
- `Image.asset(path)` → load from project assets (declare in pubspec.yaml)
- `fit: BoxFit.cover` → fill the box, may crop
- `fit: BoxFit.contain` → fit inside, may letterbox
- `loadingBuilder` → show a placeholder while loading
- `errorBuilder` → show something if image fails to load
- `ClipRRect` wrapper → add rounded corners to the image

---

# PART 8 — Currency Converter App

---

## Project Setup

- `flutter create currency_converter` → creates the project
- Clean out the default boilerplate in `main.dart`
- File structure: keep `main.dart` for the entry point, extract screens into separate files

---

## App Concepts Used

**MaterialApp** → root widget that sets up theme and the first screen  
**Scaffold** → gives the screen its structure (AppBar + body)  
**StatefulWidget** → needed because the result updates when user taps Convert  
**TextEditingController** → reads the amount the user typed  
**DropdownButton** → lets user pick target currency from a list  
**ElevatedButton** → triggers the conversion  
**setState()** → updates the result displayed on screen  

---

## Key Logic

- Store conversion rates in a `Map<String, double>` → maps currency code to rate
- On button press: read controller text → parse to double → multiply by rate → setState
- Display result with `.toStringAsFixed(2)` → formats to 2 decimal places
- Always dispose the TextEditingController in `dispose()`

---

## Things Learned from This Project

- How StatefulWidget + setState works in practice
- How to read and clear TextField values
- How to use DropdownButton with dynamic items from a list
- How layout widgets (Column, Padding, SizedBox) compose a full screen
- Why dispose() matters for memory management

---

# PART 9 — Weather App

---

## Project Concepts

This project is bigger — it introduces real API calls, JSON parsing, and async UI.

---

## http Package

- Add to `pubspec.yaml`: `http: ^1.2.0`
- Import: `import 'package:http/http.dart' as http`
- `http.get(Uri.parse(url))` → makes a GET request, returns a Future
- `response.statusCode` → HTTP status (200 = OK, 404 = not found, etc.)
- `response.body` → the raw response text (usually JSON)
- `jsonDecode(response.body)` → converts JSON string to a Dart Map

> Always check statusCode before using the response. If it's not 200, throw an error.

---

## Working with APIs

- OpenWeatherMap gives weather data as JSON at a URL with your API key
- JSON is just a Map in Dart — access data with `json['key']` and `json['nested']['key']`
- For lists in JSON: `(json['list'] as List).map(...).toList()`
- Create a model class to represent the data cleanly

---

## Handling Future in initState

- Don't `await` directly inside `initState` — it returns void
- Instead, store the Future in a variable and pass it to FutureBuilder
- FutureBuilder handles the loading/error/done states for you automatically

```dart
late Future<WeatherData> _weatherFuture;

@override
void initState() {
  super.initState();
  _weatherFuture = fetchWeather();  // store, don't await
}
```

---

## FutureBuilder Pattern

The standard pattern for API-driven screens:
1. Fetch returns a Future
2. Pass Future to FutureBuilder
3. FutureBuilder shows spinner while waiting
4. Shows data when done
5. Shows error message if it fails

This keeps your UI code clean and declarative.

---

## Passing Arguments Between Screens

- Pass data to a new screen via its constructor parameters
- The receiving screen declares them as `final` fields
- `Navigator.push` creates the screen and passes the data right there

---

## intl Package for Date Formatting

- `intl: ^0.19.0` → add to pubspec
- `DateFormat('MMM dd, yyyy').format(dateTime)` → "Feb 23, 2026"
- `DateFormat('EEEE').format(dateTime)` → "Monday"
- `DateTime.parse(isoString)` → parses an ISO 8601 date string into a DateTime object

---

# PART 10 — Flutter Internals

---

## Layout Principle: Constraints Go Down, Sizes Go Up

This is the MOST important concept to understand how Flutter layouts work.

- **Parent sends constraints DOWN** → "you can be at most this wide and this tall"
- **Child decides its own size** → within those constraints
- **Child reports size UP** to parent
- **Parent positions the child**

This explains a lot of confusing behavior:
- Why Column stretches children vertically by default
- Why an unbounded Container crashes
- Why Expanded only works inside Row/Column

---

## Flutter's 3 Trees

Flutter maintains 3 parallel trees at all times:

| Tree | What it is | Rebuilt? |
|------|-----------|---------|
| **Widget Tree** | Dart code you write — immutable config | Every setState |
| **Element Tree** | Live instances that glue widgets to render objects | Reused when possible |
| **RenderObject Tree** | Handles layout, paint, hit testing | Only when necessary |

- Widget tree rebuilds frequently — cheap, just Dart objects
- Element tree is smart — reuses elements to avoid full rebuilds
- RenderObject tree changes only when actual layout/paint changes needed

- `BuildContext` is actually an `Element` — that's why it knows where in the tree you are
- This 3-tree architecture is what makes Flutter performant despite frequent rebuilds

---

## InheritedWidget

InheritedWidget is Flutter's mechanism for passing data DOWN the tree without passing through every constructor.

- `Theme`, `MediaQuery`, `Navigator` all use InheritedWidget internally
- When you call `Theme.of(context)`, Flutter walks UP the tree to find the nearest Theme widget
- Widgets that read from an InheritedWidget only rebuild when that specific data changes

---

# PART 11 — Shop App

---

## Project Overview

Multi-screen e-commerce app. Covers: custom theming, navigation, Provider state management, responsive UI.

---

## Custom Themes & Fonts

- Declare custom fonts in `pubspec.yaml` under `flutter: fonts:`
- Apply via `ThemeData.fontFamily: 'FontName'`
- `ThemeData` → sets app-wide styling: colors, text styles, button styles, card styles
- `ColorScheme.fromSeed(seedColor: Colors.orange)` → generates a full color palette from one color
- Individual widgets inherit theme automatically — override only where needed with `.copyWith()`

---

## Navigation

Flutter's Navigator manages screens like a stack.

- `Navigator.push(context, MaterialPageRoute(...))` → go to new screen (pushes on stack)
- `Navigator.pop(context)` → go back (pops from stack)
- `Navigator.pushReplacement(...)` → replace current screen (no back option)
- `await Navigator.push(...)` → wait for screen to pop and get a returned value
- Named routes → define routes in `MaterialApp.routes`, navigate by name string
- State of a screen is kept in its `State` object → popping a screen destroys its state
- This is why you need global state for data shared across multiple screens

---

## Provider — State Management

Provider is a package for managing state that needs to be shared across multiple widgets/screens.

**Why needed?**
- `setState()` only rebuilds within ONE widget
- If two screens need to share data (e.g., cart items), setState isn't enough

**How it works:**
- Create a class extending `ChangeNotifier` → this is your "store" of shared state
- Call `notifyListeners()` inside it whenever data changes → triggers rebuilds in all listeners
- Wrap the top of your app with `ChangeNotifierProvider` to make it available everywhere
- `context.watch<CartProvider>()` → reads data AND rebuilds when it changes
- `context.read<CartProvider>()` → reads data WITHOUT subscribing to rebuilds
- `context.select<CartProvider, int>((c) => c.count)` → rebuild only when this specific value changes
- `MultiProvider` → wrap multiple providers at once

> Mental model: ChangeNotifier is like a radio station. Widgets that `.watch()` are tuned in. When you `notifyListeners()`, all tuned-in widgets get the broadcast and update.

---

## SnackBar

A temporary message that appears at the bottom of the screen.

- `ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text(...)))` 
- Auto-dismisses after a duration
- Can have an action button (e.g., "UNDO")
- Use `ScaffoldMessenger` (not just Scaffold) — it persists across navigations

---

## Dialogs

Pop-up overlays for user decisions or important messages.

- `showDialog()` → shows an `AlertDialog` in the center of the screen
- `showModalBottomSheet()` → slides up from the bottom
- Both are awaitable — `final result = await showDialog(...)` → get user's choice
- Return data from dialogs via `Navigator.pop(ctx, result)` inside the dialog

---

# PART 12 — Responsive UI

---

## MediaQuery

MediaQuery gives information about the current device screen.

- `MediaQuery.of(context).size.width` → screen width in logical pixels
- `MediaQuery.of(context).size.height` → screen height
- `MediaQuery.of(context).padding` → system padding (status bar, notch)
- `MediaQuery.of(context).viewInsets` → keyboard height when visible
- `MediaQuery.of(context).orientation` → portrait or landscape
- Use screen width to make layout decisions: `width >= 600 ? TabletLayout() : MobileLayout()`

---

## LayoutBuilder

LayoutBuilder gives you the PARENT widget's constraints — not the full screen size.

- Useful inside reusable widgets that don't know their parent size ahead of time
- `constraints.maxWidth` → max width the parent allows
- `constraints.maxHeight` → max height the parent allows

**MediaQuery vs LayoutBuilder:**

| | MediaQuery | LayoutBuilder |
|--|-----------|--------------|
| Measures | Full screen | Parent widget constraints |
| Best for | Screen-level layout decisions | Reusable widget adaptations |
| Rebuilds when | Screen resizes | Parent constraints change |

> Rule: Use MediaQuery at the page/screen level. Use LayoutBuilder inside components.

---

## InheritedWidget vs InheritedModel

| | InheritedWidget | InheritedModel |
|--|----------------|----------------|
| Rebuild trigger | Any change to the widget | Only when the specific "aspect" changes |
| Performance | Can over-rebuild | More granular |
| Example | Theme | MediaQuery |

- MediaQuery uses InheritedModel — a widget reading only `size` won't rebuild when `orientation` changes

---

## Widget Sizing Summary

| Widget | Behavior |
|--------|---------|
| `Expanded` | Takes ALL remaining space on main axis |
| `Flexible` | Takes up to available space (child can be smaller) |
| `SizedBox` | Fixed size or gap |
| `FractionallySizedBox` | Fraction of parent size (e.g., 80% of parent width) |
| `AspectRatio` | Maintains width:height ratio |
| `ConstrainedBox` | Adds min/max size constraints |
| `FittedBox` | Scales and positions child to fit within itself |

---

## Project Architecture Best Practices

**Folder structure to follow:**
```
lib/
├── models/      → Data classes (Product, User, WeatherData)
├── screens/     → Full page widgets
├── widgets/     → Reusable small widgets
├── services/    → API calls, database logic
└── providers/   → State management (ChangeNotifier classes)
```

**Why this matters:**
- Separation of concerns → UI code stays clean
- Easier to test individual pieces
- Easy to find things as the project grows

---

## What's Next After This Course

- **State Management deep dive** → Riverpod (modern, compile-safe), Bloc (enterprise scale)
- **Local storage** → Hive, Isar, SQLite (sqflite)
- **Backend** → Firebase, Supabase, REST API with Dio
- **Testing** → unit tests, widget tests, integration tests
- **Architecture** → Clean Architecture, Repository pattern, MVVM
- **Publishing** → Play Store (APK/AAB), App Store

---

## Quick Reference — Most Used Widgets

| Widget | Purpose |
|--------|---------|
| `Text` | Display text |
| `TextField` | Text input |
| `Container` | Box with styling |
| `Padding` | Add space around widget |
| `Column / Row` | Vertical / horizontal layout |
| `Stack` | Layer widgets on top of each other |
| `Scaffold` | Screen structure |
| `AppBar` | Top navigation bar |
| `ListView.builder` | Scrollable list (lazy) |
| `Card` | Elevated surface |
| `ElevatedButton` | Primary action button |
| `GestureDetector` | Detect taps, gestures |
| `FutureBuilder` | Async UI from a Future |
| `StreamBuilder` | Async UI from a Stream |
| `Navigator` | Screen navigation |
| `Provider` | Cross-widget state sharing |
| `MediaQuery` | Screen size info |
| `SafeArea` | Avoid system UI overlaps |

---
*Notes by Anik | Rivaan Ranawat — The Complete Dart & Flutter Developer Course*