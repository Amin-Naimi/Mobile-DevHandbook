
# Différence entre ViewModel et AndroidViewModel

## 1. **ViewModel**

Le `ViewModel` est une classe fournie par les composants d'architecture Android (Android Architecture Components). Elle est utilisée pour gérer les données liées à l'UI de manière indépendante du cycle de vie d'une `Activity` ou d'un `Fragment`. 

### Principales caractéristiques du `ViewModel` :
- **Indépendance du cycle de vie** : Le `ViewModel` permet de conserver les données lors des changements de configuration, comme la rotation de l'écran. Cela permet d'éviter la perte de données ou d'effectuer des appels réseau à chaque fois qu'une `Activity` est recréée.
- **Pas d'accès au `Context`** : Le `ViewModel` ne dispose pas d'un accès direct au `Context` de l'application ou de l'activité. Il est conçu pour être indépendant du contexte d'Android et de la couche UI.
- **Utilisation idéale** : Lorsqu'on a besoin de gérer des données qui ne dépendent pas du `Context` ou des ressources de l'application (par exemple, des calculs métier, des appels réseau, etc.).

### Exemple :
```kotlin
class MyViewModel : ViewModel() {
    // Logique métier sans dépendance au contexte
}
```

---

## 2. **AndroidViewModel**

`AndroidViewModel` est une sous-classe de `ViewModel`, mais elle permet d'avoir accès au `Context` de l'application via le `getApplication()`.

### Principales caractéristiques du `AndroidViewModel` :
- **Accès au `Context`** : Contrairement à `ViewModel`, `AndroidViewModel` donne accès à l'objet `Application`, ce qui permet d'interagir avec des composants nécessitant un `Context` (comme les bases de données, les préférences partagées, etc.).
- **Dépendance au contexte** : Le `AndroidViewModel` est plus dépendant du `Context`, ce qui peut compliquer les tests unitaires ou rendre le code moins flexible dans certains cas.
- **Utilisation idéale** : Lorsqu'il est nécessaire d'accéder à des ressources globales ou des services qui nécessitent un `Context`, comme une base de données SQLite, un gestionnaire de préférences partagées, ou l'accès aux ressources de l'application.

### Exemple :
```kotlin
class MyAndroidViewModel(application: Application) : AndroidViewModel(application) {
    // Accès au contexte de l'application via getApplication()
    val appContext = getApplication<Application>().applicationContext
}
```

---

## Différences clés entre `ViewModel` et `AndroidViewModel` :

| **Caractéristique**              | **`ViewModel`**                             | **`AndroidViewModel`**                               |
|----------------------------------|--------------------------------------------|-----------------------------------------------------|
| **Accès au contexte**            | Aucun accès direct au `Context`            | Accès direct au `Context` via `getApplication()`     |
| **Utilisation**                  | Utilisé pour les tâches qui ne nécessitent pas de `Context` | Utilisé pour accéder à des ressources de l'application, base de données, etc. |
| **Tests unitaires**              | Facile à tester, car pas de dépendance au `Context` | Plus difficile à tester, car dépend du `Context`    |
| **Idéal pour**                   | Logique métier indépendante de l'UI        | Accès à des services liés à l'application (base de données, ressources, etc.) |

---

## Conclusion :
- **Utilisez `ViewModel`** si vous n'avez pas besoin d'un `Context` ou d'accéder aux ressources globales de l'application. Cette approche est plus simple et plus testable.
- **Utilisez `AndroidViewModel`** si vous avez besoin d'un accès au `Context` pour interagir avec des services spécifiques à l'application (comme la base de données ou les ressources). Cependant, soyez conscient des défis liés aux tests unitaires.

