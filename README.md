# 📱 Jetpack Compose – Programmier Cheatsheet

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

# 🧭 Navigation Setup

```kotlin
val nav = rememberNavController()

NavHost(navController = nav, startDestination = "home") {

    composable("home") { HomeScreen(nav) }

    composable("detail/{name}") { backStack ->
        val name = backStack.arguments?.getString("name") ?: ""
        DetailScreen(name, nav)
    }
}
```

---

# ➡️ Navigate mit Parameter

```kotlin
nav.navigate("detail/$name")
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
            nav.navigate("detail/${state.name}")
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

- IMMER `collectAsState()`
- IMMER `.copy()` beim State ändern
- ViewModel nur 1x erstellen
- Parameter IMMER im Route String definieren

---

# 🚀 Minimal Workflow

1. State erstellen  
2. ViewModel machen  
3. UI mit `collectAsState()`  
4. Navigation bauen  
5. Events → ViewModel  

---
