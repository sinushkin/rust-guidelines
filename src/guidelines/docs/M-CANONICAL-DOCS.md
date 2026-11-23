<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Documentation has canonical sections (M-CANONICAL-DOCS) { #M-CANONICAL-DOCS }

<span class="ru">Документация содержит канонические разделы (M-CANONICAL-DOCS)</span>

<why>established Rust documentation practices.</why>

<span class="ru">устоявшиеся практики документирования в Rust.</span>

Public library items must contain the canonical doc sections. The summary sentence must always be present. Extended documentation and examples
are strongly encouraged. The other sections must be present when applicable.

<span class="ru">Публичные элементы библиотеки должны содержать канонические разделы документации. Итоговое предложение должно присутствовать всегда. Развёрнутая документация и примеры настоятельно рекомендуются. Остальные разделы должны присутствовать там, где это применимо.</span>

```rust
/// Summary sentence < 15 words.
///
/// Extended documentation in free form.
///
/// # Examples
/// One or more examples that show API usage like so.
///
/// # Errors
/// If fn returns `Result`, list known error conditions
///
/// # Panics
/// If fn may panic, list when this may happen
///
/// # Safety
/// If fn is `unsafe` or may otherwise cause UB, this section must list
/// all conditions a caller must uphold.
///
/// # Abort
/// If fn may abort the process, list when this may happen.
pub fn foo() {}
```

In contrast to other languages, you should not create a table of parameters. Instead parameter use is explained in plain text. In other words, do not

<span class="ru">В отличие от других языков, не следует создавать таблицу параметров. Вместо этого использование параметров объясняется обычным текстом. Другими словами, не делайте так:</span>

```rust,ignore
/// Copies a file.
///
/// # Parameters
/// - src: The source.
/// - dst: The destination.
fn copy(src: File, dst: File) {}
```

but instead:

<span class="ru">а вместо этого:</span>

```rust,ignore
/// Copies a file from `src` to `dst`.
fn copy(src: File, dst: File) {}
```

### Related Reading

<span class="ru">Связанное чтение</span>

- Function docs include error, panic, and safety considerations ([C-FAILURE](https://rust-lang.github.io/api-guidelines/documentation.html#c-failure))
  - <span class="ru">Документация функций включает соображения об ошибках, паниках и безопасности ([C-FAILURE](https://rust-lang.github.io/api-guidelines/documentation.html#c-failure))</span>
