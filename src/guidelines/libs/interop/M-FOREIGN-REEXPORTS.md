<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Items come from their original crate (M-FOREIGN-REEXPORTS) { #M-FOREIGN-REEXPORTS }

<span class="ru">Элементы берутся из своего исходного крейта (M-FOREIGN-REEXPORTS)</span>

<why>unambiguous type identity.</why>

<span class="ru">однозначная идентичность типов.</span>

Crates should generally not re-export items from other crates. For example, if your crate contains a method `foo::download(url: bar::Url)`, you should not do `pub use bar::Url` from inside `foo`. This avoids having possibly dozens of aliases in context, which can get confusing for both users and agents, in particular if these are mixed with genuinely different types of the same name from other crates.

<span class="ru">Крейты, как правило, не должны реэкспортировать элементы из других крейтов. Например, если ваш крейт содержит метод `foo::download(url: bar::Url)`, не следует делать `pub use bar::Url` внутри `foo`. Это позволяет избежать десятков псевдонимов в контексте, что может запутать и пользователей, и агентов, особенно если они смешаны с действительно различными типами с тем же именем из других крейтов.</span>

When a crate accepts or returns a type defined in some third-party crate, users are expected to depend on that third-party crate directly and import the type from there. That said, there are a few valid exceptions to this rule:

<span class="ru">Когда крейт принимает или возвращает тип, определённый в стороннем крейте, предполагается, что пользователи напрямую зависят от этого крейта и импортируют тип оттуда. Тем не менее есть несколько допустимых исключений из этого правила:</span>

- Umbrella crates (compare [M-DONT-LEAK-TYPES](./#M-DONT-LEAK-TYPES)) by definition re-export other types
  - <span class="ru">Зонтичные крейты (ср. [M-DONT-LEAK-TYPES](./#M-DONT-LEAK-TYPES)) по определению реэкспортируют другие типы</span>
- Crates split for technical reasons (e.g., exporting `foo_core::Url` from `foo`)
  - <span class="ru">Крейты, разделённые по техническим причинам (например, экспорт `foo_core::Url` из `foo`)</span>
- Macro use to provide stable paths, e.g., via some hidden `foo::__private::Url`
  - <span class="ru">Использование макросов для предоставления стабильных путей, например через скрытый `foo::__private::Url`</span>
