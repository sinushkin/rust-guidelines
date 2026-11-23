<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Has comprehensive module documentation (M-MODULE-DOCS) { #M-MODULE-DOCS }

<span class="ru">Имеет развёрнутую документацию модуля (M-MODULE-DOCS)</span>

<why>easy API docs navigation.</why>

<span class="ru">удобная навигация по документации API.</span>

Any public library module must have `//!` module documentation, and the first sentence must follow [M-DOC-FIRST-SENTENCE].

<span class="ru">Любой публичный модуль библиотеки должен иметь документацию модуля `//!`, а первое предложение должно соответствовать [M-DOC-FIRST-SENTENCE].</span>

```rust,edition2021,ignore
pub mod ffi {
    //! Contains FFI abstractions.

    pub struct String {};
}
```

The rest of the module documentation should be comprehensive, i.e., cover the most relevant technical aspects of the contained items, including

<span class="ru">Остальная часть документации модуля должна быть развёрнутой, то есть охватывать наиболее значимые технические аспекты содержащихся в нём элементов, включая:</span>

- what the module contains
  - <span class="ru">что содержит модуль</span>
- when it should be used, possibly when not
  - <span class="ru">когда его следует использовать и, возможно, когда не следует</span>
- examples
  - <span class="ru">примеры</span>
- subsystem specifications (e.g., `std::fmt` [also describes its formatting language](https://doc.rust-lang.org/stable/std/fmt/index.html#formatting-parameters))
  - <span class="ru">спецификации подсистем (например, `std::fmt` [также описывает свой язык форматирования](https://doc.rust-lang.org/stable/std/fmt/index.html#formatting-parameters))</span>
- observable side effects, including what guarantees are made about these, if any
  - <span class="ru">наблюдаемые побочные эффекты, включая гарантии, которые даются в отношении них, если таковые есть</span>
- relevant implementation details, e.g., the used system APIs
  - <span class="ru">значимые детали реализации, например, используемые системные API</span>

 Great examples include:

<span class="ru">Отличные примеры включают:</span>

- [`std::fmt`](https://doc.rust-lang.org/stable/std/fmt/index.html)
- [`std::pin`](https://doc.rust-lang.org/stable/std/pin/index.html)
- [`std::option`](https://doc.rust-lang.org/stable/std/option/index.html)

This does not mean every module should contain all of these items. But if there is something to say about the interaction of the contained types,
their module documentation is the right place.

<span class="ru">Это не значит, что каждый модуль должен содержать все эти элементы. Но если есть что сказать о взаимодействии содержащихся типов, то место для этого — их документация модуля.</span>

[M-DOC-FIRST-SENTENCE]: ./#M-DOC-FIRST-SENTENCE
