<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Macros don't lie about signatures (M-MACROS-DONT-LIE) { #M-MACROS-DONT-LIE }

<span class="ru">Макросы не лгут о сигнатурах (M-MACROS-DONT-LIE)</span>

<why>clarity for users and LLMs.</why>

<span class="ru">ясность для пользователей и LLM.</span>

Macros must not (make users) misrepresent signatures or the shape of items.

<span class="ru">Макросы не должны (вводить пользователей в заблуждение относительно) сигнатур или формы элементов.</span>

Macros have the ability to arbitrarily rewrite token streams. They could convert structs to enums, traits to functions, or perform any other transformation imaginable. They should, however, do none of that, as the resulting code will be highly confusing and virtually impossible to predict or reason about.

<span class="ru">Макросы могут произвольно переписывать потоки токенов. Они могли бы превращать структуры в перечисления, трейты в функции или выполнять любые другие мыслимые преобразования. Однако им не следует делать ничего из этого, поскольку получившийся код будет крайне запутанным и практически невозможным для предсказания или анализа.</span>

Among others, macros must not

<span class="ru">В частности, макросы не должны:</span>

- visibly convert the nature of data types (e.g., structs to enums, ...),
  - <span class="ru">явно менять природу типов данных (например, структуры в перечисления, ...),</span>
- alter function signatures,
  - <span class="ru">изменять сигнатуры функций,</span>
- convert the `async`-ness of items,
  - <span class="ru">менять `async`-природу элементов,</span>
- do anything else that materially detaches _what's written_ from _what's happening_.
  - <span class="ru">делать что-либо ещё, что существенно отделяет _написанное_ от _происходящего_.</span>

```rust,ignore
// Bad: Adds extra parameter and marks function `async`. Impossible to 
// predict from reading code. 
#[magic_transform]
fn foo() { }

foo(token).await
```
