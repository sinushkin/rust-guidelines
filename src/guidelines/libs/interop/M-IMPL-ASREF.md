<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Accept `impl AsRef<>` where feasible (M-IMPL-ASREF) { #M-IMPL-ASREF }

<span class="ru">Принимайте `impl AsRef<>`, где это возможно (M-IMPL-ASREF)</span>

<why>flexibility for callers to use their own types.</why>

<span class="ru">гибкость для вызывающего кода при использовании собственных типов.</span>

In **function** signatures, accept `impl AsRef<T>` for types that have a
[clear reference hierarchy](https://doc.rust-lang.org/stable/std/convert/trait.AsRef.html#implementors), where you
do not need to take ownership, or where object creation is relatively cheap.

<span class="ru">В сигнатурах **функций** принимайте `impl AsRef<T>` для типов, у которых есть [понятная иерархия реализаций](https://doc.rust-lang.org/stable/std/convert/trait.AsRef.html#implementors), если не нужно забирать владение или создание объекта относительно дёшево.</span>

| Instead of ... | accept ... |
| --- | --- |
| `&str`, `String` | `impl AsRef<str>` |
| `&Path`, `PathBuf` | `impl AsRef<Path>` |
| `&[u8]`, `Vec<u8>` | `impl AsRef<[u8]>` |

```rust,ignore
# use std::path::Path;
// Definitely use `AsRef`, the function does not need ownership.
fn print(x: impl AsRef<str>) {}
fn read_file(x: impl AsRef<Path>) {}
fn send_network(x: impl AsRef<[u8]>) {}

// Further analysis needed. In these cases the function wants
// ownership of some `String` or `Vec<u8>`. If those are
// "low frequency, low volume" functions `AsRef` has better ergonomics,
// otherwise accepting a `String` or `Vec<u8>` will have better
// performance.
fn new_instance(x: impl AsRef<str>) -> HoldsString {}
fn send_to_other_thread(x: impl AsRef<[u8]>) {}
```

In contrast, **types** should generally not be infected by these bounds:

<span class="ru">Напротив, **типы** обычно не должны «заражаться» такими ограничениями:</span>

```rust,ignore
// Generally not ok. There might be exceptions for performance
// reasons, but those should not be user visible.
struct User<T: AsRef<str>> {
    name: T
}

// Better
struct User {
    name: String
}
```
