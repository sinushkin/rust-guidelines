<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Third party items come from hidden `_private` module (M-MACRO-HELPERS) { #M-MACRO-HELPERS }

<span class="ru">Элементы сторонних крейтов приходят из скрытого модуля `_private` (M-MACRO-HELPERS)</span>

<why>predictable compilation.</why>

<span class="ru">предсказуемая компиляция.</span>

When a macro expansion needs to refer to third-party items, the host crate should re-export those from a hidden module, and the macro should emit fully-qualified paths through that module rather than expecting the user's crate to depend on the third-party crate directly.

<span class="ru">Когда раскрытию макроса нужны элементы сторонних крейтов, основной крейт должен реэкспортировать их из скрытого модуля, а макрос должен генерировать полные пути через этот модуль, а не полагаться на то, что крейт пользователя напрямую зависит от стороннего крейта.</span>

For example, a crate `foo` requiring `bar` traits would do:

<span class="ru">Например, крейт `foo`, которому нужны трейты из `bar`, сделает так:</span>

```rust,ignore
#[doc(hidden)]
pub mod _private {
    pub use ::bar::Bar;
}

pub use foo_proc::my_macro;
```

The `my_macro!` implementation would then rely on its presence in its emitted code:

<span class="ru">Реализация `my_macro!` затем будет опираться на наличие этого пути в генерируемом коде:</span>

```rust,ignore
impl ::foo::_private::Bar for MyType { ... }
```
