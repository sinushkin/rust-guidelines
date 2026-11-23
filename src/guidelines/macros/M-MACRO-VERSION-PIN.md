<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Pin supporting proc macro crates (M-MACRO-VERSION-PIN) { #M-MACRO-VERSION-PIN }

<span class="ru">Закрепляйте вспомогательные крейты процедурных макросов (M-MACRO-VERSION-PIN)</span>

<why>keep generated code compatible with its library</why>

<span class="ru">сохранять сгенерированный код совместимым с его библиотекой</span>

A crate that re-exports macros from companion proc macro crates must pin them to its own exact
version via `=x.y.z`, and each macro crate must pin its own companion crates the same way. When
publishing, those crates are counted as one crate and must be published together with the same
exact version.

<span class="ru">Крейт, который реэкспортирует макросы из вспомогательных крейтов процедурных макросов, должен закрепить их на свою точную версию через `=x.y.z`, а каждый крейт макросов должен так же закрепить своих вспомогательных спутников. При публикации эти крейты считаются одним крейтом и должны публиковаться вместе с одной и той же точной версией.</span>

Without exact pins, a newer macro may generate code incompatible with an older version of the
crate, causing unexpected and hard-to-diagnose compilation failures in the generated code.

<span class="ru">Без точного закрепления более новый макрос может генерировать код, несовместимый со старой версией крейта, что приводит к неожиданным и трудно диагностируемым ошибкам компиляции сгенерированного кода.</span>

M-MACRO-VERSION-PIN does not apply to independently consumed macro libraries.

<span class="ru">M-MACRO-VERSION-PIN не применяется к макроблиотекам, которые используются независимо.</span>

Example:

<span class="ru">Пример:</span>

```toml
# foo/Cargo.toml
[dependencies]
foo_macros = "=1.2.3"
```
