<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Macros assume main crate (M-MACRO-MAIN-CRATE) { #M-MACRO-MAIN-CRATE }

<span class="ru">Макросы предполагают основной крейт (M-MACRO-MAIN-CRATE)</span>

<why>simple macro logic.</why>

<span class="ru">простая логика макросов.</span>

Procedural macros can (and should) assume they are used through their main crate and emit paths for that.

<span class="ru">Процедурные макросы могут (и должны) исходить из того, что их используют через основной крейт, и генерировать пути для этого.</span>

For crates including proc macros it is common to ship them split in 3 for technical reasons:

<span class="ru">Для крейтов с процедурными макросами обычно по техническим причинам их разделяют на 3 части:</span>

- `foo` - the main crate that re-exports macros from `foo_proc`, along with extra traits or types,
  - <span class="ru">`foo` — основной крейт, который реэкспортирует макросы из `foo_proc`, а также дополнительные трейты или типы,</span>
- `foo_proc` - facade re-exporting macros from `foo_proc_impl` with `proc-macro = true`,
  - <span class="ru">`foo_proc` — фасад, реэкспортирующий макросы из `foo_proc_impl`, с `proc-macro = true`,</span>
- `foo_proc_impl` - the actual macro implementation and unit tests.
  - <span class="ru">`foo_proc_impl` — собственно реализация макросов и модульные тесты.</span>

In some cases there can be additional crates involved. Authors might be tempted to make `foo`, `foo_proc`, and siblings all work, resulting in complex re-export hierarchies or the use of 3rd party helpers. In reality, the minimal UX gain is usually not worth the added complexity (or compile time overhead), given the ecosystem precedent of mostly not supporting these usage modes in the first place.

<span class="ru">В некоторых случаях могут участвовать и другие крейты. Авторы могут соблазниться сделать так, чтобы работали `foo`, `foo_proc` и их «соседи», что ведёт к сложной иерархии реэкспортов или использованию сторонних вспомогательных крейтов. На деле минимальный выигрыш в удобстве обычно не стоит добавленной сложности (или накладных расходов на время компиляции), учитывая, что в экосистеме такие режимы использования в основном изначально не поддерживаются.</span>

This also implies you should not attempt to support use cases where your crate is imported under a different name.

<span class="ru">Это также означает, что не следует пытаться поддерживать сценарии, в которых ваш крейт импортируется под другим именем.</span>
