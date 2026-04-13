# 📱 Jetpack Compose – Programmier Cheatsheet (Navigation 3)

---

# 🧠 ViewModel Template

```kotlin
class MyViewModel : ViewModel() {

    private val _state = MutableStateFlow(MyState())
    val state = _state.asStateFlow()

    fun updateName(name: String) {
        _state.update { it.copy(name = name) }
    }

    fun increment() {
        _state.update { it.copy(count = it.count + 1) }
    }
}

data class MyState(
    val name: String = "",
    val count: Int = 0
)
```

---

# 🎨 State in UI

```kotlin
val vm: MyViewModel = viewModel()
val state by vm.state.collectAsState()
```

---

# ✏️ TextField

```kotlin
TextField(
    value = state.name,
    onValueChange = { vm.updateName(it) },
    label = { Text("Name") }
)
```

---

# 🔘 Button

```kotlin
Button(onClick = { vm.increment() }) {
    Text("Click")
}
```

---

# 🧭 Navigation 3 – Screen Definition

```kotlin
sealed class Screen {

    data object Home : Screen()

    data class Detail(val name: String) : Screen()
}
```

---

# 🧭 Navigation Setup

```kotlin
val nav = rememberNavController()

NavHost(navController = nav, startDestination = Screen.Home) {

    composable<Screen.Home> {
        HomeScreen(nav)
    }

    composable<Screen.Detail> { entry ->
        val args = entry.toRoute<Screen.Detail>()
        DetailScreen(args.name, nav)
    }
}
```

---

# ➡️ Navigate mit Parameter

```kotlin
nav.navigate(Screen.Detail(name))
```

---

# 🔙 Zurück

```kotlin
nav.popBackStack()
```

---

# 🏠 HomeScreen

```kotlin
@Composable
fun HomeScreen(nav: NavController, vm: MyViewModel = viewModel()) {

    val state by vm.state.collectAsState()

    Column {
        TextField(
            value = state.name,
            onValueChange = { vm.updateName(it) }
        )

        Button(onClick = {
            nav.navigate(Screen.Detail(state.name))
        }) {
            Text("Next")
        }
    }
}
```

---

# 📄 DetailScreen

```kotlin
@Composable
fun DetailScreen(name: String, nav: NavController) {

    Column {
        Text("Hello $name")

        Button(onClick = {
            nav.popBackStack()
        }) {
            Text("Back")
        }
    }
}
```

---

# 📐 Layout Basics

```kotlin
Column(
    modifier = Modifier
        .fillMaxSize()
        .padding(16.dp),
    verticalArrangement = Arrangement.Center
)
```

---

# ⚠️ WICHTIG (Prüfung)

- KEINE Strings mehr für Navigation
- IMMER `sealed class Screen`
- IMMER `composable<Screen.X>`
- IMMER `toRoute<>()` für Argumente
- IMMER `collectAsState()`
- IMMER `.copy()` beim State ändern

---

# 🚀 Minimal Workflow

1. State erstellen  
2. ViewModel machen  
3. Screens (sealed class)  
4. NavHost mit `composable<>()`  
5. Navigation mit `Screen.X(...)`  
6. UI mit `collectAsState()`  

---

# 💯 Merksatz

👉 Navigation 2 = Strings  
👉 Navigation 3 = Klassen
