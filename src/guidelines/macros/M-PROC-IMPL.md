<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Proc macros should have separate impl crate incl. tests (M-PROC-IMPL) { #M-PROC-IMPL }

<span class="ru">Процедурные макросы должны иметь отдельный крейт реализации, включая тесты (M-PROC-IMPL)</span>

<why>thoroughly testable proc macros.</why>

<span class="ru">тщательно тестируемые процедурные макросы.</span>

Proc macros should be thin shims inside some `foo_proc` crate that delegate to a separate, regular library crate, usually called `foo_proc_impl`, which contains the actual token-stream transformation logic and its tests.

<span class="ru">Процедурные макросы должны быть тонкими обёртками внутри крейта `foo_proc`, которые делегируют работу отдельному обычному библиотечному крейту, обычно называемому `foo_proc_impl`, содержащему собственно логику преобразования потоков токенов и её тесты.</span>

As proc macro crates are special, testing them from `foo_proc` usually requires workarounds for unit and snapshot tests. Instead, consider having a `foo_proc_impl` crate:

<span class="ru">Поскольку крейты процедурных макросов особенные, тестирование их из `foo_proc` обычно требует обходных путей для модульных и снапшот-тестов. Вместо этого рассмотрите вариант с отдельным крейтом `foo_proc_impl`:</span>

```rust,ignore
use proc_macro2::TokenStream;

pub fn my_macro(attr: TokenStream, item: TokenStream) -> TokenStream { ... }
```

These can come with regular [insta](https://insta.rs/) or similar snapshot tests, and are then exported as genuine proc macros via a `foo_proc` crate like so:

<span class="ru">К ним можно добавить обычные снапшот-тесты [insta](https://insta.rs/) или похожие, а затем экспортировать как настоящие процедурные макросы через крейт `foo_proc` следующим образом:</span>

```rust,ignore
#[proc_macro_attribute]
pub fn my_macro(attr: TokenStream, item: TokenStream) -> TokenStream {
    foo_proc_impl::my_macro(attr.into(), item.into()).into()
}
```

The macros are then re-exported from the core crate:

<span class="ru">Затем макросы реэкспортируются из основного крейта:</span>

```rust,ignore
pub use foo_proc::my_macro;
```

Inside the core crate, we also recommend adding [trybuild](https://docs.rs/trybuild/latest/trybuild/) UI tests with negative examples to ensure consistent error messages.

<span class="ru">Внутри основного крейта мы также рекомендуем добавить UI-тесты [trybuild](https://docs.rs/trybuild/latest/trybuild/) с негативными примерами, чтобы обеспечить стабильность сообщений об ошибках.</span>
