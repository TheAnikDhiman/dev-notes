# FLUTTER — Complete Notes for App Development

> Goal: one single resource covering everything from your first widget to production-ready apps (state management, networking, Firebase, architecture). Simple language, definition + code for every topic. Assumes you already know Dart.

---

## Table of Contents
1. [Introduction to Flutter](#1-introduction-to-flutter)
2. [Project Structure & Setup](#2-project-structure--setup)
3. [Widgets — The Core Idea](#3-widgets--the-core-idea)
4. [StatelessWidget vs StatefulWidget](#4-statelesswidget-vs-statefulwidget)
5. [Basic UI Widgets](#5-basic-ui-widgets)
6. [Layout Widgets](#6-layout-widgets)
7. [Lists & Grids](#7-lists--grids)
8. [Navigation & Routing](#8-navigation--routing)
9. [Forms & User Input](#9-forms--user-input)
10. [Async UI — FutureBuilder & StreamBuilder](#10-async-ui--futurebuilder--streambuilder)
11. [Networking (API calls)](#11-networking-api-calls)
12. [State Management](#12-state-management)
13. [Local Storage](#13-local-storage)
14. [Firebase Integration](#14-firebase-integration)
15. [Theming & Material 3](#15-theming--material-3)
16. [Animations](#16-animations)
17. [App Architecture (Clean Architecture / MVVM)](#17-app-architecture-clean-architecture--mvvm)
18. [Packages & pubspec.yaml](#18-packages--pubspecyaml)
19. [Responsive & Adaptive Design](#19-responsive--adaptive-design)
20. [Testing](#20-testing)
21. [Building & Deploying](#21-building--deploying)
22. [Flutter Cheat Sheet — Common Mistakes & Fixes](#22-flutter-cheat-sheet--common-mistakes--fixes)

---

## 1. Introduction to Flutter

**Definition:** Flutter is Google's open-source UI toolkit for building natively-compiled apps for mobile, web, and desktop from a **single codebase**, written in Dart.

**How it works under the hood:**
- Flutter doesn't use native OS widgets (like Android's Button or iOS's UIButton). Instead, it **draws every single pixel itself** using its own rendering engine, **Impeller** (the default renderer in 2026 on iOS/Android/macOS, replacing the older Skia engine). This is why a Flutter app looks identical on Android and iOS.
- Your UI is described entirely in Dart code as a **tree of widgets**.
- **Hot Reload:** injects updated code into a running app in under a second, keeping app state intact — this is Flutter's biggest productivity feature during development.

**Why Flutter (vs React Native/native)?**
- One codebase → Android, iOS, Web, Windows, macOS, Linux.
- Near-native performance (compiled, not interpreted).
- Huge, mature widget catalog + pub.dev package ecosystem.

---

## 2. Project Structure & Setup

```
my_app/
├── android/          # native Android project
├── ios/              # native iOS project
├── lib/              # 🔴 YOUR DART CODE LIVES HERE — 95% of your work
│   └── main.dart     # entry point
├── test/             # unit/widget tests
├── pubspec.yaml      # dependencies, assets, app metadata
└── pubspec.lock      # exact locked versions of dependencies
```

### The minimum app
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp()); // runApp attaches the widget tree to the screen
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'My App',
      theme: ThemeData(primarySwatch: Colors.blue, useMaterial3: true),
      home: const HomeScreen(),
    );
  }
}
```

- **`runApp()`**: takes a widget and makes it the root of the widget tree, rendered to the screen.
- **`MaterialApp`**: a wrapper widget that sets up Material Design (theme, routes, navigator, localization) for your whole app. (`CupertinoApp` exists for pure iOS-style apps.)
- **`BuildContext`**: a handle/reference to a widget's location in the widget tree. Used to find theme data, navigate, show dialogs, etc. Think of it as "where am I in the tree right now."

---

## 3. Widgets — The Core Idea

**Definition:** In Flutter, **everything is a widget** — text, padding, layout, buttons, even the app itself. A widget is an immutable description of a part of the UI. Widgets are combined (nested) to build the full screen — this is called the **widget tree**.

```dart
Widget build(BuildContext context) {
  return Scaffold(               // gives basic screen structure
    appBar: AppBar(title: const Text('Home')),
    body: Center(
      child: Text('Hello Flutter'),
    ),
  );
}
```

**Key concept — everything nests:** `Scaffold` → `body` → `Center` → `child` → `Text`. Flutter UI = a big tree of widgets nested inside each other.

**`Scaffold`**: implements the basic Material Design visual layout — gives you `appBar`, `body`, `floatingActionButton`, `drawer`, `bottomNavigationBar` slots out of the box.

**Three widget categories to know:**
| Type | Purpose | Examples |
|---|---|---|
| Structural | screen layout | `Scaffold`, `AppBar`, `SafeArea` |
| Layout | arrange other widgets | `Row`, `Column`, `Stack`, `Container` |
| Leaf/content | actual content | `Text`, `Icon`, `Image` |

---

## 4. StatelessWidget vs StatefulWidget

### StatelessWidget
**Definition:** A widget that has **no mutable state** — once built, it never changes on its own (it only rebuilds if its parent gives it new data).
```dart
class Greeting extends StatelessWidget {
  final String name;
  const Greeting({super.key, required this.name});

  @override
  Widget build(BuildContext context) {
    return Text('Hello, $name');
  }
}
```

### StatefulWidget
**Definition:** A widget that **can hold mutable state** that changes over time (e.g., a counter, a checkbox, form input) and needs to rebuild the UI when that state changes.
```dart
class Counter extends StatefulWidget {
  const Counter({super.key});

  @override
  State<Counter> createState() => _CounterState();
}

class _CounterState extends State<Counter> {
  int count = 0;

  void increment() {
    setState(() {          // 🔴 tells Flutter: "state changed, rebuild this widget"
      count++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('Count: $count'),
        ElevatedButton(onPressed: increment, child: const Text('Add')),
      ],
    );
  }
}
```

**`setState()`**: the core mechanism that tells Flutter "data changed, please call `build()` again and update the screen." Without it, changing a variable does NOT update the UI.

### StatefulWidget lifecycle (important!)
```dart
class _MyWidgetState extends State<MyWidget> {
  @override
  void initState() {
    super.initState();
    // runs ONCE when the widget is first created — good for setting up controllers, API calls
  }

  @override
  void didChangeDependencies() {
    super.didChangeDependencies();
    // runs after initState, and again if an InheritedWidget it depends on changes
  }

  @override
  Widget build(BuildContext context) {
    // runs every time the UI needs to redraw
    return Container();
  }

  @override
  void dispose() {
    // runs when the widget is permanently removed — clean up controllers, streams, listeners here
    super.dispose();
  }
}
```

---

## 5. Basic UI Widgets

```dart
// Text
Text(
  'Hello',
  style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold, color: Colors.blue),
)

// Icon
Icon(Icons.favorite, color: Colors.red, size: 30)

// Image
Image.network('https://example.com/pic.png')
Image.asset('assets/logo.png')

// Buttons
ElevatedButton(onPressed: () {}, child: const Text('Click'))
TextButton(onPressed: () {}, child: const Text('Cancel'))
OutlinedButton(onPressed: () {}, child: const Text('Outline'))
IconButton(icon: const Icon(Icons.add), onPressed: () {})

// Container — the "div" of Flutter: box model (padding, margin, decoration, size)
Container(
  width: 100,
  height: 100,
  padding: const EdgeInsets.all(8),
  margin: const EdgeInsets.symmetric(horizontal: 12),
  decoration: BoxDecoration(
    color: Colors.blue,
    borderRadius: BorderRadius.circular(12),
    boxShadow: [BoxShadow(color: Colors.black26, blurRadius: 4)],
  ),
  child: const Text('Box'),
)

// SizedBox — mostly used to add fixed spacing
const SizedBox(height: 16)

// Padding — adds space around a widget
Padding(padding: const EdgeInsets.all(16), child: const Text('Padded'))
```

---

## 6. Layout Widgets

### Row & Column
**Definition:** Arrange children **horizontally** (`Row`) or **vertically** (`Column`).
```dart
Row(
  mainAxisAlignment: MainAxisAlignment.spaceBetween, // alignment along the row direction
  crossAxisAlignment: CrossAxisAlignment.center,      // alignment perpendicular to it
  children: [
    Icon(Icons.star),
    Text('Rating'),
    Icon(Icons.star),
  ],
)

Column(
  mainAxisAlignment: MainAxisAlignment.center,
  children: const [
    Text('Line 1'),
    Text('Line 2'),
  ],
)
```
**Definition — MainAxis vs CrossAxis:** for a `Row`, main axis = horizontal, cross axis = vertical. For a `Column`, it's reversed. This trips up almost everyone at first.

### Expanded & Flexible
**Definition:** `Expanded` forces a child to fill the remaining available space along the main axis. `Flexible` lets a child take up space proportionally but doesn't force it to fill everything.
```dart
Row(
  children: [
    Expanded(flex: 2, child: Container(color: Colors.red)),
    Expanded(flex: 1, child: Container(color: Colors.blue)),
  ],
) // red takes 2/3 width, blue takes 1/3
```

### Stack
**Definition:** Overlaps widgets on top of each other (like layers in Photoshop) — used for badges, overlays, backgrounds with text on top.
```dart
Stack(
  alignment: Alignment.center,
  children: [
    Image.asset('assets/bg.png'),
    const Positioned(top: 10, right: 10, child: Icon(Icons.notifications)),
    const Text('Overlay Text'),
  ],
)
```

### Center, Align
```dart
Center(child: Text('Centered'))
Align(alignment: Alignment.bottomRight, child: Text('Bottom Right'))
```

### Wrap
**Definition:** Like `Row`/`Column`, but automatically wraps children to the next line when there's no more space — great for tags/chips.
```dart
Wrap(
  spacing: 8,
  runSpacing: 8,
  children: [Chip(label: Text('Dart')), Chip(label: Text('Flutter'))],
)
```

---

## 7. Lists & Grids

### ListView
**Definition:** A scrollable list of widgets. `.builder` is the version you should use for dynamic/long lists — it only builds items that are visible on screen (**lazy loading** → huge performance win).
```dart
ListView.builder(
  itemCount: items.length,
  itemBuilder: (context, index) {
    return ListTile(
      leading: const Icon(Icons.person),
      title: Text(items[index]),
      onTap: () => print('Tapped ${items[index]}'),
    );
  },
)

// Fixed small list (all children built at once)
ListView(
  children: const [Text('A'), Text('B'), Text('C')],
)

// Separator between items
ListView.separated(
  itemCount: items.length,
  separatorBuilder: (context, index) => const Divider(),
  itemBuilder: (context, index) => Text(items[index]),
)
```

### GridView
```dart
GridView.builder(
  gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(crossAxisCount: 2),
  itemCount: items.length,
  itemBuilder: (context, index) => Card(child: Text(items[index])),
)
```

### ListTile
**Definition:** A ready-made row layout for a single list item (icon + title + subtitle + trailing widget) — used constantly in settings screens, contact lists, etc.
```dart
ListTile(
  leading: const Icon(Icons.email),
  title: const Text('Email'),
  subtitle: const Text('anik@example.com'),
  trailing: const Icon(Icons.arrow_forward_ios),
  onTap: () {},
)
```

---

## 8. Navigation & Routing

### Basic Navigator (imperative)
**Definition:** `Navigator` manages a stack of screens (routes) — `push` adds a new screen on top, `pop` removes the current one and goes back.
```dart
// Go to a new screen
Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => const DetailScreen()),
);

// Go back
Navigator.pop(context);

// Pass data forward
Navigator.push(context, MaterialPageRoute(builder: (context) => DetailScreen(id: 5)));

// Get data back from a screen
final result = await Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => const SelectScreen()),
);
// in SelectScreen: Navigator.pop(context, 'selectedValue');
```

### Named routes
```dart
MaterialApp(
  initialRoute: '/',
  routes: {
    '/': (context) => const HomeScreen(),
    '/details': (context) => const DetailScreen(),
  },
)
// navigate:
Navigator.pushNamed(context, '/details');
```

### go_router (recommended in 2026 for anything beyond a simple app)
**Definition:** The officially recommended declarative routing package — handles deep links, nested navigation, and URL-based routing (important for Flutter Web) far better than the raw `Navigator`.
```dart
final router = GoRouter(
  routes: [
    GoRoute(path: '/', builder: (context, state) => const HomeScreen()),
    GoRoute(
      path: '/details/:id',
      builder: (context, state) => DetailScreen(id: state.pathParameters['id']!),
    ),
  ],
);

MaterialApp.router(routerConfig: router);

// Navigate
context.go('/details/5');
context.push('/details/5'); // adds to stack instead of replacing
```

---

## 9. Forms & User Input

### TextField (uncontrolled input)
```dart
TextField(
  decoration: const InputDecoration(labelText: 'Name', border: OutlineInputBorder()),
  onChanged: (value) => print(value),
)
```

### TextEditingController (controlled input — most common)
**Definition:** An object that lets you read/set/listen to a text field's value programmatically.
```dart
class _MyFormState extends State<MyForm> {
  final controller = TextEditingController();

  @override
  void dispose() {
    controller.dispose(); // ALWAYS dispose controllers to avoid memory leaks
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return TextField(controller: controller);
  }

  void submit() {
    print(controller.text); // read the current value
  }
}
```

### Form & validation
**Definition:** `Form` groups multiple input fields together so you can validate and save all of them at once using a `GlobalKey<FormState>`.
```dart
class _LoginFormState extends State<LoginForm> {
  final formKey = GlobalKey<FormState>();
  String email = '';

  @override
  Widget build(BuildContext context) {
    return Form(
      key: formKey,
      child: Column(
        children: [
          TextFormField(
            decoration: const InputDecoration(labelText: 'Email'),
            validator: (value) {
              if (value == null || !value.contains('@')) {
                return 'Enter a valid email';
              }
              return null; // null means valid
            },
            onSaved: (value) => email = value!,
          ),
          ElevatedButton(
            onPressed: () {
              if (formKey.currentState!.validate()) {
                formKey.currentState!.save();
                print('Email: $email');
              }
            },
            child: const Text('Submit'),
          ),
        ],
      ),
    );
  }
}
```

### Other input widgets
```dart
Checkbox(value: isChecked, onChanged: (v) => setState(() => isChecked = v!))
Switch(value: isOn, onChanged: (v) => setState(() => isOn = v))
Radio<int>(value: 1, groupValue: selected, onChanged: (v) => setState(() => selected = v))
DropdownButton<String>(
  value: selectedItem,
  items: options.map((o) => DropdownMenuItem(value: o, child: Text(o))).toList(),
  onChanged: (v) => setState(() => selectedItem = v),
)
```

---

## 10. Async UI — FutureBuilder & StreamBuilder

### FutureBuilder
**Definition:** A widget that rebuilds itself automatically based on the state of a `Future` — showing a loading spinner, then either the data or an error, without you manually managing booleans.
```dart
FutureBuilder<String>(
  future: fetchUserName(),          // your async function
  builder: (context, snapshot) {
    if (snapshot.connectionState == ConnectionState.waiting) {
      return const CircularProgressIndicator();
    } else if (snapshot.hasError) {
      return Text('Error: ${snapshot.error}');
    } else if (snapshot.hasData) {
      return Text('Hello, ${snapshot.data}');
    }
    return const SizedBox();
  },
)
```

### StreamBuilder
**Definition:** Same idea as `FutureBuilder`, but rebuilds every time a `Stream` emits a new value — perfect for real-time data (chat, Firestore live updates).
```dart
StreamBuilder<int>(
  stream: countStream(),
  builder: (context, snapshot) {
    if (!snapshot.hasData) return const CircularProgressIndicator();
    return Text('Count: ${snapshot.data}');
  },
)
```

**Common mistake:** never call `fetchUserName()` directly inside `build()` without storing it — that re-triggers the Future on every rebuild. Store it in `initState()`:
```dart
late Future<String> userFuture;

@override
void initState() {
  super.initState();
  userFuture = fetchUserName(); // called ONCE
}
```

---

## 11. Networking (API calls)

**Definition:** Almost every real app talks to a backend/API. The `http` package (or `dio` for more advanced needs like interceptors) is the standard way to make network calls in Dart/Flutter.

```yaml
# pubspec.yaml
dependencies:
  http: ^1.2.0
```

```dart
import 'package:http/http.dart' as http;
import 'dart:convert';

Future<Map<String, dynamic>> fetchUser(int id) async {
  final response = await http.get(Uri.parse('https://api.example.com/users/$id'));

  if (response.statusCode == 200) {
    return jsonDecode(response.body);
  } else {
    throw Exception('Failed to load user');
  }
}

// POST request
Future<void> createUser(String name) async {
  final response = await http.post(
    Uri.parse('https://api.example.com/users'),
    headers: {'Content-Type': 'application/json'},
    body: jsonEncode({'name': name}),
  );
  print(response.statusCode);
}
```

### Model classes (parsing JSON properly)
**Definition:** Instead of using raw `Map<String, dynamic>` everywhere, create a typed model class with a `fromJson` factory constructor. Safer and more maintainable — standard practice in real projects.
```dart
class User {
  final int id;
  final String name;

  User({required this.id, required this.name});

  factory User.fromJson(Map<String, dynamic> json) {
    return User(id: json['id'], name: json['name']);
  }

  Map<String, dynamic> toJson() => {'id': id, 'name': name};
}

// Usage
final user = User.fromJson(jsonDecode(response.body));
```

**Dio** (used when you need interceptors, timeouts, auth headers globally, retries):
```dart
final dio = Dio(BaseOptions(baseUrl: 'https://api.example.com', connectTimeout: const Duration(seconds: 5)));
final response = await dio.get('/users/1');
```

---

## 12. State Management

**Definition:** State management is HOW your app stores data (state) and pushes updates to the UI when that data changes. `setState()` works fine for tiny apps, but breaks down as apps grow (can't easily share state across distant widgets, causes unnecessary rebuilds).

### The problem setState doesn't solve well
- Sharing state between widgets that are far apart in the tree (e.g., cart count shown in both AppBar and a product page).
- Avoiding unnecessary rebuilds of the whole widget subtree.
- Separating business logic from UI code (hard to test `setState`-heavy widgets).

### Provider (simple, beginner-friendly — still common in smaller apps)
```dart
class CounterProvider extends ChangeNotifier {
  int count = 0;
  void increment() {
    count++;
    notifyListeners(); // tells all listening widgets to rebuild
  }
}

// Register at the top of the app
ChangeNotifierProvider(create: (_) => CounterProvider(), child: const MyApp())

// Use in a widget
Consumer<CounterProvider>(
  builder: (context, counter, child) => Text('${counter.count}'),
)
// or:
context.watch<CounterProvider>().count;   // rebuilds on change
context.read<CounterProvider>().increment(); // just call a method, no rebuild
```

### Riverpod (recommended default in 2026 — compile-safe, less boilerplate than Provider, no BuildContext needed)
**Definition:** A reactive state management/dependency-injection framework built by the same author as Provider, fixing its main limitations (compile-time safety, testability, no context dependency).
```dart
// 1. Define a provider
final counterProvider = StateProvider<int>((ref) => 0);

// 2. Wrap app in ProviderScope
void main() {
  runApp(const ProviderScope(child: MyApp()));
}

// 3. Consume in a ConsumerWidget
class CounterScreen extends ConsumerWidget {
  const CounterScreen({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final count = ref.watch(counterProvider);       // rebuilds on change
    return Column(
      children: [
        Text('$count'),
        ElevatedButton(
          onPressed: () => ref.read(counterProvider.notifier).state++,
          child: const Text('Add'),
        ),
      ],
    );
  }
}

// Async data with Riverpod (very common for API calls)
final userProvider = FutureProvider<User>((ref) async {
  return fetchUser(1);
});

// in UI:
final userAsync = ref.watch(userProvider);
userAsync.when(
  data: (user) => Text(user.name),
  loading: () => const CircularProgressIndicator(),
  error: (err, stack) => Text('Error: $err'),
);
```

### Bloc / Cubit (enterprise standard — strict, predictable, testable)
**Definition:** Bloc (Business Logic Component) separates UI from business logic using **Events** (input) and **States** (output). `Cubit` is a simplified version of Bloc without formal events — you just call methods directly.
```dart
// Cubit — simpler version
class CounterCubit extends Cubit<int> {
  CounterCubit() : super(0);
  void increment() => emit(state + 1); // emit a new state
}

// Provide it
BlocProvider(create: (_) => CounterCubit(), child: const MyApp())

// Consume it
BlocBuilder<CounterCubit, int>(
  builder: (context, count) => Text('$count'),
)

context.read<CounterCubit>().increment();
```

### Which one should you pick?
| Package | Best for | Learning curve |
|---|---|---|
| `setState` | Tiny/local widget-only state | None |
| `Provider` | Small-medium apps, quick projects | Easy |
| `Riverpod` | Most apps in 2026 — best default choice | Medium |
| `Bloc`/`Cubit` | Large teams, enterprise, strict audit trail | Medium-High |

---

## 13. Local Storage

### SharedPreferences — simple key-value storage (settings, tokens, flags)
```yaml
dependencies:
  shared_preferences: ^2.2.0
```
```dart
final prefs = await SharedPreferences.getInstance();
await prefs.setString('username', 'anik');
await prefs.setBool('isLoggedIn', true);

String? name = prefs.getString('username');
```

### sqflite — local SQL database (structured, relational data)
```dart
final db = await openDatabase(
  'app.db',
  version: 1,
  onCreate: (db, version) {
    return db.execute('CREATE TABLE users(id INTEGER PRIMARY KEY, name TEXT)');
  },
);
await db.insert('users', {'name': 'Anik'});
final users = await db.query('users');
```

### Hive — fast NoSQL key-value database (popular alternative to sqflite)
```dart
var box = await Hive.openBox('myBox');
box.put('name', 'Anik');
String name = box.get('name');
```

---

## 14. Firebase Integration

**Definition:** Firebase is Google's Backend-as-a-Service platform — gives you authentication, a real-time/NoSQL database, file storage, push notifications, and more, without building your own backend.

```yaml
dependencies:
  firebase_core: ^3.0.0
  firebase_auth: ^5.0.0
  cloud_firestore: ^5.0.0
```

```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized(); // required before any Firebase call
  await Firebase.initializeApp();
  runApp(const MyApp());
}
```

### Firebase Auth
```dart
// Sign up
await FirebaseAuth.instance.createUserWithEmailAndPassword(
  email: 'test@test.com',
  password: '123456',
);

// Sign in
await FirebaseAuth.instance.signInWithEmailAndPassword(
  email: 'test@test.com',
  password: '123456',
);

// Listen to auth state (great with StreamBuilder to auto-redirect logged-in users)
FirebaseAuth.instance.authStateChanges().listen((User? user) {
  print(user == null ? 'Logged out' : 'Logged in as ${user.email}');
});
```

### Cloud Firestore (real-time NoSQL database)
```dart
final firestore = FirebaseFirestore.instance;

// Write
await firestore.collection('users').doc('user1').set({'name': 'Anik'});

// Read once
final doc = await firestore.collection('users').doc('user1').get();
print(doc.data());

// Real-time listen (used with StreamBuilder)
firestore.collection('users').snapshots().listen((snapshot) {
  for (var doc in snapshot.docs) {
    print(doc.data());
  }
});
```

---

## 15. Theming & Material 3

**Definition:** Theming lets you define colors, fonts, and component styles **once** and apply them consistently across the whole app.

```dart
MaterialApp(
  theme: ThemeData(
    useMaterial3: true,
    colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
    textTheme: const TextTheme(
      bodyLarge: TextStyle(fontSize: 16),
    ),
    elevatedButtonTheme: ElevatedButtonThemeData(
      style: ElevatedButton.styleFrom(backgroundColor: Colors.deepPurple),
    ),
  ),
  darkTheme: ThemeData.dark(useMaterial3: true),
  themeMode: ThemeMode.system, // auto light/dark based on device setting
)

// Access theme anywhere via context
Theme.of(context).colorScheme.primary
Theme.of(context).textTheme.bodyLarge
```

**Material 3 (`useMaterial3: true`)** is Google's current design system (rounded shapes, dynamic color) and is the default for new Flutter projects in 2026.

---

## 16. Animations

### Implicit animations (easiest — just wrap a widget)
```dart
AnimatedContainer(
  duration: const Duration(milliseconds: 300),
  width: isExpanded ? 200 : 100,
  color: isExpanded ? Colors.blue : Colors.red,
)

AnimatedOpacity(
  opacity: isVisible ? 1.0 : 0.0,
  duration: const Duration(milliseconds: 300),
  child: const Text('Fading text'),
)
```

### Explicit animations (full control — AnimationController)
```dart
class _MyWidgetState extends State<MyWidget> with SingleTickerProviderStateMixin {
  late AnimationController controller;
  late Animation<double> animation;

  @override
  void initState() {
    super.initState();
    controller = AnimationController(vsync: this, duration: const Duration(seconds: 1));
    animation = Tween<double>(begin: 0, end: 1).animate(controller);
    controller.forward(); // start the animation
  }

  @override
  void dispose() {
    controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return FadeTransition(opacity: animation, child: const Text('Hi'));
  }
}
```

**`SingleTickerProviderStateMixin`**: a mixin that provides the "ticker" (heartbeat) needed to drive an `AnimationController`. Required whenever you use `vsync: this`.

### Hero animation (shared element transition between screens)
```dart
Hero(tag: 'profile-pic', child: Image.asset('assets/profile.png'))
// same tag on the destination screen — Flutter animates the transition automatically
```

---

## 17. App Architecture (Clean Architecture / MVVM)

**Definition:** As apps grow, dumping everything into widgets becomes unmanageable. The 2026-recommended pattern is **Clean Architecture + MVVM**, splitting code into layers:

```
lib/
├── data/                  # Data layer — talks to the outside world
│   ├── models/            # fromJson/toJson data classes
│   └── repositories/      # implements data-fetching logic (API, local DB)
├── domain/                # Business logic — pure Dart, no Flutter imports
│   └── use_cases/
├── presentation/           # UI layer
│   ├── screens/
│   ├── widgets/
│   └── providers/ (or view_models, cubits, notifiers)
```

**Golden rule:** your state management package (Riverpod/Bloc/Provider) should ONLY live in the presentation layer. Your data/domain layers should have zero dependency on it — this keeps business logic testable and swappable.

**Repository pattern example:**
```dart
abstract class UserRepository {
  Future<User> getUser(int id);
}

class UserRepositoryImpl implements UserRepository {
  @override
  Future<User> getUser(int id) async {
    final response = await http.get(Uri.parse('https://api.example.com/users/$id'));
    return User.fromJson(jsonDecode(response.body));
  }
}
```

---

## 18. Packages & pubspec.yaml

**Definition:** `pubspec.yaml` is the configuration file for a Flutter project — dependencies, assets, fonts, and metadata.

```yaml
name: my_app
description: A new Flutter app.
version: 1.0.0+1

environment:
  sdk: '>=3.0.0 <4.0.0'

dependencies:
  flutter:
    sdk: flutter
  http: ^1.2.0
  provider: ^6.1.0
  flutter_riverpod: ^2.5.0
  go_router: ^14.0.0

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^4.0.0

flutter:
  uses-material-design: true
  assets:
    - assets/images/
  fonts:
    - family: Poppins
      fonts:
        - asset: assets/fonts/Poppins-Regular.ttf
```

- Run `flutter pub get` after editing to fetch dependencies.
- `pub.dev` is the official package repository (same idea as npm for JS or pip for Python).
- Version syntax `^1.2.0` means "compatible with 1.2.0, allow updates up to but not including 2.0.0."

---

## 19. Responsive & Adaptive Design

**Definition:** Making your app look good across phone, tablet, desktop, and web screen sizes.

```dart
// MediaQuery — get screen size/orientation
double screenWidth = MediaQuery.of(context).size.width;
bool isPortrait = MediaQuery.of(context).orientation == Orientation.portrait;

// LayoutBuilder — build different layouts based on available space
LayoutBuilder(
  builder: (context, constraints) {
    if (constraints.maxWidth > 600) {
      return const WideLayout();
    } else {
      return const NarrowLayout();
    }
  },
)

// OrientationBuilder
OrientationBuilder(
  builder: (context, orientation) {
    return orientation == Orientation.portrait ? const Column() : const Row();
  },
)
```

---

## 20. Testing

**Definition:** Flutter supports 3 levels of testing: unit tests (pure logic), widget tests (single widget in isolation), and integration tests (full app flow).

```dart
// Unit test
import 'package:test/test.dart';

void main() {
  test('adds two numbers', () {
    expect(add(2, 3), 5);
  });
}

// Widget test
import 'package:flutter_test/flutter_test.dart';

void main() {
  testWidgets('Counter increments', (WidgetTester tester) async {
    await tester.pumpWidget(const MyApp());
    expect(find.text('0'), findsOneWidget);

    await tester.tap(find.byIcon(Icons.add));
    await tester.pump(); // rebuild after state change

    expect(find.text('1'), findsOneWidget);
  });
}
```

---

## 21. Building & Deploying

```bash
# Run in debug mode
flutter run

# Build release APK (Android)
flutter build apk --release

# Build App Bundle (required for Play Store)
flutter build appbundle --release

# Build for iOS (needs macOS + Xcode)
flutter build ios --release

# Build for web
flutter build web

# Check for issues before release
flutter analyze
flutter doctor
```

- **Play Store:** upload the `.aab` (App Bundle) from `build/app/outputs/bundle/release/`.
- **App Store:** build via Xcode/Transporter after `flutter build ios`.
- Always increment `version: 1.0.0+1` in `pubspec.yaml` before each release (`+1` is the build number).

---

## 22. Flutter Cheat Sheet — Common Mistakes & Fixes

| Mistake | Fix |
|---|---|
| Calling `setState()` inside `build()` | Never — causes infinite rebuild loop. Only call it from event handlers (`onPressed`, etc.) |
| Not disposing `TextEditingController`/`AnimationController` | Always dispose in `dispose()` to prevent memory leaks |
| Calling API inside `build()` | Call it in `initState()` and store the `Future` in a variable |
| "RenderFlex overflowed" error | Wrap with `Expanded`/`Flexible`, or use `SingleChildScrollView` |
| Forgetting `const` on unchanging widgets | Add `const` wherever possible — big performance win |
| Using `Navigator.push` everywhere in large apps | Switch to `go_router` for maintainability + deep links |
| Business logic inside widgets | Move it to a state management layer (Riverpod/Bloc) — keeps widgets "dumb" |
| Rebuilding the whole screen for one small state change | Use `Consumer`/`ref.watch` on the smallest possible widget, not the whole screen |

---

## Final Tips for Practice
1. Build small: a counter app → a to-do list app → an app that calls a real API → an app with Firebase auth. Each step teaches you a new section above.
2. Sections 8–14 (Navigation, Forms, Async UI, Networking, State Management, Storage, Firebase) are what 90% of real apps are made of — get comfortable there first.
3. Since you already use Riverpod in your projects (like SnapLingo), focus extra practice time on Section 17 (Architecture) — that's what separates "an app that works" from "an app that scales."
4. Use `flutter doctor` whenever something feels broken in your setup — it diagnoses environment issues instantly.
