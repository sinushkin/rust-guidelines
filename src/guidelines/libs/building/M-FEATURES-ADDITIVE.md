<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Features are additive (M-FEATURES-ADDITIVE) { #M-FEATURES-ADDITIVE }

<span class="ru">Функции аддитивны (M-FEATURES-ADDITIVE)</span>

<why>reliable compilation in large, complex projects.</why>

<span class="ru">надёжная компиляция в больших и сложных проектах.</span>

All library features must be additive, and any combination must work, as long as the feature itself would work on the current platform. This implies:

<span class="ru">Все функции библиотеки должны быть аддитивными, и любая их комбинация должна работать, при условии что сама функция работала бы на текущей платформе. Это означает:</span>

- [ ] You must not introduce a `no-std` feature, use a `std` feature instead
  - <span class="ru">Не должно быть функции `no-std`; вместо неё используйте функцию `std`.</span>
- [ ] Adding any feature `foo` must not disable or modify any public item
  - <span class="ru">Добавление любой функции `foo` не должно отключать или изменять какой-либо публичный элемент</span>
  - Adding enum variants is fine if these enums are `#[non_exhaustive]`
    - <span class="ru">Добавление вариантов перечислений допустимо, если эти перечисления помечены `#[non_exhaustive]`.</span>
- [ ] Features must not rely on other features to be manually enabled
  - <span class="ru">Функции не должны зависеть от того, что другие функции включены вручную.</span>
- [ ] Features must not rely on their parent to skip-enable a feature in one of their children
  - <span class="ru">Функции не должны полагаться на то, что родительская функция пропустит включение функции в одном из своих дочерних элементов.</span>

Further Reading

<span class="ru">Дополнительная литература</span>

- [Feature Unification](https://doc.rust-lang.org/cargo/reference/features.html#feature-unification)
- [Mutually Exclusive Features](https://doc.rust-lang.org/cargo/reference/features.html#mutually-exclusive-features)
