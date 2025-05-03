# 📱 Navigation avec Jetpack Compose

Ce guide explique comment configurer et utiliser la **navigation** dans une application Android avec **Jetpack Compose**, y compris l’utilisation optionnelle d’un `when` pour centraliser la logique de navigation.

---

## 📦 Dépendances requises

Dans `build.gradle` (Module):

```kotlin
implementation("androidx.navigation:navigation-compose:2.7.7")
implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.7.0")
```

---

## 🗺️ Définir les routes d’écrans

Dans un fichier `Screen.kt` :

```kotlin
sealed class Screen(val route: String) {
    object Main : Screen("main")
    object AddStudent : Screen("add_student")
    object DeleteStudent : Screen("delete_student")
    object UpdateStudent : Screen("update_student")
    object StudentList : Screen("student_list")
}
```

---

## 🔄 Configuration de la navigation

Dans votre composable principal `StudentApp.kt` :

```kotlin
@Composable
fun StudentApp(
    menuViewModel: MenuViewModel,
    studentViewModel: StudentViewModel
) {
    val navController = rememberNavController()

    NavHost(navController = navController, startDestination = Screen.Main.route) {
        composable(Screen.Main.route) {
            MainScreen(viewModel = menuViewModel, navController = navController)
        }
        composable(Screen.AddStudent.route) {
            AddStudentScreen(viewModel = studentViewModel)
        }
        composable(Screen.DeleteStudent.route) {
            DeleteStudentScreen(viewModel = studentViewModel)
        }
        composable(Screen.UpdateStudent.route) {
            UpdateStudentScreen(viewModel = studentViewModel)
        }
        composable(Screen.StudentList.route) {
            StudentListScreen(viewModel = studentViewModel)
        }
    }
}
```

---

## 🎯 Centraliser la navigation avec `when`

Vous pouvez créer une fonction utilitaire pour centraliser les appels de navigation :

```kotlin
fun navigateTo(screen: Screen, navController: NavController) {
    when (screen) {
        is Screen.Main -> navController.navigate(Screen.Main.route)
        is Screen.AddStudent -> navController.navigate(Screen.AddStudent.route)
        is Screen.DeleteStudent -> navController.navigate(Screen.DeleteStudent.route)
        is Screen.UpdateStudent -> navController.navigate(Screen.UpdateStudent.route)
        is Screen.StudentList -> navController.navigate(Screen.StudentList.route)
    }
}
```

### 🔘 Exemple d’utilisation dans un écran

```kotlin
Button(onClick = {
    navigateTo(Screen.AddStudent, navController)
}) {
    Text("Ajouter un étudiant")
}
```

---

## 🔙 Revenir à l’écran précédent

```kotlin
navController.popBackStack()
```

---

## 🧱 Arborescence recommandée

```
├── ui/
│   ├── screens/
│   │   ├── MainScreen.kt
│   │   ├── AddStudentScreen.kt
│   │   └── ...
│   └── StudentApp.kt
├── navigation/
│   ├── Screen.kt
│   └── NavigationUtils.kt
├── viewModel/
│   ├── MenuViewModel.kt
│   └── StudentViewModel.kt
```

---

## ✅ Conclusion

- Jetpack Compose permet une navigation simple avec `NavHost` et `composable`.
- Pour une structure claire, utilisez un `sealed class` pour vos routes.
- Pour centraliser la logique, utilisez un `when` dans une fonction dédiée à la navigation.
