# 🧠 Résumé pratique : Flow vs StateFlow vs LiveData vs stateIn

## 📍 Objectif

Comprendre rapidement les différences et usages de :
- `Flow`
- `StateFlow`
- `LiveData`
- `stateIn()`

---

## 🔄 1. `Flow`

> Flux de données **asynchrone**, **froid** (il commence à émettre seulement quand on le collecte).

```kotlin
val students: Flow<List<Student>> = repository.getStudents()
```

### ✔️ Avantages :
- Léger et puissant (`map`, `filter`, `combine`, etc.)
- Naturellement utilisé avec `Room`
- Compatible avec Compose via `collectAsState()`

### ⚠️ Limites :
- Ne garde **pas de valeur actuelle**
- Doit être collecté à chaque fois

---

## 🔁 2. `StateFlow`

> Un `Flow` **chaud** (il émet dès qu’il y a un changement) avec **une valeur actuelle** (comme `LiveData`).

```kotlin
val allStudents: StateFlow<List<Student>> = ...
```

### ✔️ Avantages :
- Toujours une **valeur par défaut**
- Idéal pour Compose : `collectAsState()`
- Peut être partagé et observé par plusieurs consommateurs

### ✅ Utilisation typique :
Utilise `stateIn()` pour **convertir un Flow en StateFlow** dans le ViewModel.

---

## 🔄 3. `stateIn()`

> Fonction qui **transforme un Flow en StateFlow** dans un `ViewModel`.

```kotlin
val allStudents: StateFlow<List<Student>> = repository.getStudents()
    .stateIn(
        viewModelScope,
        SharingStarted.WhileSubscribed(5000),
        emptyList()
    )
```

### ⚙️ Paramètres :
- `scope` → Généralement `viewModelScope`
- `SharingStarted` → Stratégie de partage
- `initialValue` → Valeur de départ du `StateFlow`

---

## 📡 4. `LiveData`

> Ancienne solution observée par les `Activity` / `Fragment`. Gère automatiquement le **cycle de vie**.

```kotlin
val students: LiveData<List<Student>> = flow.asLiveData()
```

### ✔️ Avantages :
- Simple à utiliser avec XML / Fragments
- Gère les lifecycles automatiquement

### ❌ Limites :
- Moins flexible que `Flow`
- Pas optimal avec Compose

---

## 🆚 Comparatif résumé

| 🔍 Aspect                   | `Flow`          | `StateFlow`      | `LiveData`      |
|----------------------------|------------------|------------------|------------------|
| Nature                     | Froid            | Chaud            | Chaud            |
| A une valeur actuelle      | ❌ Non            | ✅ Oui            | ✅ Oui            |
| Cycle de vie               | ❌ Manuelle       | ❌ Manuelle       | ✅ Automatique    |
| Recommandé avec Compose    | ✅ Oui            | ✅ Oui            | ⚠️ Convertir      |
| Convertit avec             | —                | `stateIn()`      | `asLiveData()`   |

---

## 📝 En résumé

- Utilise **`Flow` dans le DAO** avec Room
- Utilise **`stateIn()` dans le ViewModel** pour obtenir un `StateFlow`
- Dans Compose, consomme avec **`collectAsState()`**
- N’utilise `LiveData` que si tu es encore dans un vieux code XML/Fragment

---

## 📚 Liens utiles

- [Kotlin Flow Docs](https://kotlinlang.org/docs/flow.html)
- [StateFlow vs SharedFlow](https://developer.android.com/kotlin/flow/stateflow-and-sharedflow)
- [Jetpack Compose + Flow](https://developer.android.com/jetpack/compose/state)
