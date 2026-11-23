<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Names of items are short (M-SHORT-NAMES) { #M-SHORT-NAMES }

<span class="ru">Имена элементов короткие (M-SHORT-NAMES)</span>

<why>idiomatic code.</why>

<span class="ru">идиоматичный код.</span>

The Rust convention that item identifiers are short should be followed:

<span class="ru">Следует соблюдать принятое в Rust соглашение, что идентификаторы элементов короткие:</span>

- identifiers should not compound more than 2 short words (`AppConfig` over `GlobalApplicationConfig`),
  - <span class="ru">идентификаторы не должны составляться из более чем двух коротких слов (`AppConfig` вместо `GlobalApplicationConfig`),</span>
- module or crate information shouldn't be baked into prefixes (`foo::Id` over `foo::FooId`), in particular when the direct 'super' item is sufficiently descriptive - in these cases users are expected to disambiguate items locally via qualifiers where needed (`fn convert(foo::Id) -> bar::Id`).
  - <span class="ru">информация о модуле или крейте не должна вшиваться в префиксы (`foo::Id` вместо `foo::FooId`), в особенности когда прямой «родительский» элемент достаточно описателен; в таких случаях пользователи должны при необходимости различать элементы локально с помощью квалификаторов (`fn convert(foo::Id) -> bar::Id`).</span>
- abbreviations are preferred (`CallbackFn` over `CallbackFunction`),
  - <span class="ru">предпочтительны сокращения (`CallbackFn` вместо `CallbackFunction`),</span>

Any of these rules can be broken where it makes local sense, but on a per-crate bases these exceptions should be _exceptional_ and well motivated.

<span class="ru">Любое из этих правил можно нарушить там, где это разумно в конкретном месте, но в рамках каждого крейта такие исключения должны быть _исключительными_ и хорошо обоснованными.</span>
