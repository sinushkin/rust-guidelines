<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Modules are balanced in size and scope (M-BALANCED-MODULES) { #M-BALANCED-MODULES }

<span class="ru">Модули сбалансированы по размеру и области ответственности (M-BALANCED-MODULES)</span>

<why>discoverable functionality and clear API usage.</why>

<span class="ru">обнаруживаемая функциональность и понятное использование API.</span>

Your module design should approximately follow established UX practices of menu design: A _reasonable_ number of your most important items should be placed in the crate root, and a comprehensible grouping of the remaining functionality into subordinate modules.

<span class="ru">Структура модулей должна примерно следовать устоявшимся UX-практикам проектирования меню: _разумное_ количество самых важных элементов размещается в корне крейта, а остальная функциональность понятно группируется во вложенные модули.</span>

Two violations of that rule are encountered most frequently: flat module roots containing dozens of items without clear ordering, or the excessive use of submodules without items in the crate root. While there are crates where this makes sense (e.g., automatically generated `-sys` crates defining 100s of C items, or umbrella crates like `std` and `tokio`), the majority of library crates are not among them.

<span class="ru">Чаще всего встречаются два нарушения этого правила: плоский корень модуля с десятками элементов без понятного порядка или чрезмерное использование подмодулей при отсутствии элементов в корне крейта. Есть крейты, для которых это оправдано (например, автоматически генерируемые `-sys`-крейты с сотнями элементов C или зонтичные крейты вроде `std` и `tokio`), но большинство библиотечных крейтов к ним не относятся.</span>

When designing your module layout, consider these factors:

<span class="ru">При проектировании структуры модулей учитывайте следующие факторы:</span>

- Essential items users must find in order to use a crate should go into its root. For example, a `foo_client` crate should probably have its main `Client` struct inside the root.
  - <span class="ru">Ключевые элементы, которые пользователь должен найти, чтобы использовать крейт, должны находиться в его корне. Например, в крейте `foo_client`, вероятно, основная структура `Client` должна быть в корне.</span>

- Other items should be grouped semantically by use case. Modules named `traits` and `errors` don't help anyone, but `account`, `network` and `status` do.
  - <span class="ru">Остальные элементы следует группировать семантически, по сценарию использования. Модули с именами `traits` и `errors` никому не помогают, а `account`, `network` и `status` помогают.</span>

- Also take into account that modules are the perfect place for module-level documentation that further explains the respective subsystem.
  - <span class="ru">Также учитывайте, что модули — идеальное место для документации уровня модуля, которая подробнее объясняет соответствующую подсистему.</span>
