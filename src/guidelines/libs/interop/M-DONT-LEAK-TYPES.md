<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Don't leak external types (M-DONT-LEAK-TYPES) { #M-DONT-LEAK-TYPES }

<span class="ru">Не раскрывайте внешние типы (M-DONT-LEAK-TYPES)</span>

<why>stable APIs and low long-term maintenance cost.</why>

<span class="ru">стабильные API и низкая долгосрочная стоимость сопровождения.</span>

Where possible, you should prefer `std`<sup>1</sup> types in public APIs over types coming from external crates. Exceptions should be carefully considered.

<span class="ru">По возможности предпочитайте типы `std`<sup>1</sup> в публичных API типам из внешних крейтов. Исключения следует тщательно обдумывать.</span>

Any type in any public API will become part of that API's contract. Since `std` and constituents are the only crates
shipped by default, and since they come with a permanent stability guarantee, their types are the only ones that come without an interoperability risk.

<span class="ru">Любой тип в любом публичном API становится частью контракта этого API. Поскольку `std` и её составляющие — единственные крейты, поставляемые по умолчанию, и у них есть постоянная гарантия стабильности, их типы — единственные, которые не несут риска совместимости.</span>

A crate that exposes another crate's type is said to _leak_ that type.

<span class="ru">Говорят, что крейт _раскрывает_ тип другого крейта (_leak_).</span>

For maximal long term stability your crate should, theoretically, not leak any types. Practically, some leakage
is unavoidable, sometimes even beneficial. We recommend you follow this heuristic:

<span class="ru">Для максимальной долгосрочной стабильности крейт теоретически не должен раскрывать никаких типов. На практике некоторое раскрытие неизбежно, а иногда даже полезно. Рекомендуем следовать такой эвристике:</span>

- [ ] if you can avoid it, do not leak third-party types
  - <span class="ru">если можете этого избежать, не раскрывайте типы сторонних крейтов</span>
- [ ] if you are part of an umbrella crate,<sup>2</sup> you may freely leak types from sibling crates.
  - <span class="ru">если вы являетесь частью зонтичного крейта (umbrella crate)<sup>2</sup>, можно свободно раскрывать типы соседних крейтов.</span>
- [ ] behind a relevant feature flag, types may be leaked (e.g., `serde`)
  - <span class="ru">за соответствующим feature-флагом типы можно раскрывать (например, `serde`)</span>
- [ ] without a feature _only_ if they give a _substantial benefit_. Most commonly that is interoperability with significant
      other parts of the Rust ecosystem based around these types.
  - <span class="ru">без feature-флага — _только_ если это даёт _существенную выгоду_. Обычно это совместимость со значимыми другими частями экосистемы Rust, построенными вокруг этих типов.</span>

<footnotes>

<sup>1</sup> In rare instances, e.g., high performance libraries used from embedded, you might even want to limit yourself to `core` only.

<span class="ru"><sup>1</sup> В редких случаях, например для высокопроизводительных библиотек во встраиваемых системах, можно даже ограничиться только `core`.</span>

<sup>2</sup> For example, a `runtime` crate might be the umbrella of `runtime_rt`, `runtime_app` and `runtime_clock` As users are
expected to only interact with the umbrella, siblings may leak each others types.

<span class="ru"><sup>2</sup> Например, крейт `runtime` может быть зонтичным для `runtime_rt`, `runtime_app` и `runtime_clock`. Поскольку пользователи должны взаимодействовать только с зонтичным крейтом, соседние крейты могут раскрывать типы друг друга.</span>

</footnotes>
