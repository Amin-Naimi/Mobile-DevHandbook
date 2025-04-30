
# 🖼️ Jetpack Compose – `imageVector` vs `painter`

This guide explains when to use `imageVector` or `painter` in Jetpack Compose for displaying icons or images.

---

## ✅ When to Use `imageVector`

Use `imageVector` when working with **built-in Material Design icons** provided by Jetpack Compose.

- 📦 Example:
  ```kotlin
  Icon(
      imageVector = Icons.Default.Home,
      contentDescription = "Home"
  )
  ```

- 📚 Available sources:
  - `Icons.Default.*`
  - `Icons.Filled.*`
  - `Icons.Outlined.*`
  - `Icons.Rounded.*`
  - `Icons.Sharp.*`
  - `Icons.TwoTone.*`

---

## ✅ When to Use `painter`

Use `painter` when loading **custom icons** or **images** from the `res/drawable` directory.

- 📦 Example with a custom SVG/XML icon:
  ```kotlin
  Icon(
      painter = painterResource(R.drawable.ic_custom_icon),
      contentDescription = "Custom icon"
  )
  ```

- 📦 Example with a PNG image:
  ```kotlin
  Image(
      painter = painterResource(R.drawable.my_image),
      contentDescription = "Image"
  )
  ```

---

## 🔁 Summary Table

| Use Case                        | Use                | Example                                      |
|--------------------------------|---------------------|----------------------------------------------|
| Built-in Material icons         | `imageVector`       | `Icons.Default.Home`                         |
| Imported SVG/XML vector icons   | `painter`           | `painterResource(R.drawable.ic_custom_icon)` |
| PNG/JPEG/WebP images            | `painter`           | `painterResource(R.drawable.image_png)`      |

---

## 📎 Notes

- The `Icon()` composable accepts **either** `imageVector` **or** `painter`—not both.
- Use `imageVector` when you want icons that adapt to light/dark themes.
- Vector assets are scalable and preferred for icons due to their small size and clarity.
