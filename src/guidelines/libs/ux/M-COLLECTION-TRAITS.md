<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Collections implement the appropriate iter traits (M-COLLECTION-TRAITS) { #M-COLLECTION-TRAITS }

<span class="ru">Коллекции реализуют подходящие трейты итераторов (M-COLLECTION-TRAITS)</span>

<why>composable collections.</why>

<span class="ru">компонуемые коллекции.</span>

Custom collections should implement the iterator-facing traits the standard library offers.

<span class="ru">Пользовательские коллекции должны реализовывать предоставляемые стандартной библиотекой трейты, связанные с итераторами.</span>

Whenever you define a new collection type `Collection<T>` for consumption by third parties, the following traits and types should also be implemented, [see here](https://cheats.rs/#iterators) for more details:

<span class="ru">Всякий раз, когда вы определяете новый тип коллекции `Collection<T>` для использования сторонними разработчиками, также следует реализовать следующие трейты и типы; подробности [см. здесь](https://cheats.rs/#iterators):</span>

- the structs `IntoIter<T>`, `Iter<T>` and `IterMut<T>`,
  - <span class="ru">структуры `IntoIter<T>`, `Iter<T>` и `IterMut<T>`,</span>
- an `impl Iterator` for all of them,
  - <span class="ru">реализацию `Iterator` для всех них,</span>
- the methods `c.iter()` and `c.iter_mut()`,
  - <span class="ru">методы `c.iter()` и `c.iter_mut()`,</span>
- an `impl IntoIterator` for `Collection<T>`, `&Collection<T>` and `&mut Collection<T>`,
  - <span class="ru">реализацию `IntoIterator` для `Collection<T>`, `&Collection<T>` и `&mut Collection<T>`,</span>
- an `impl FromIterator` for `Collection<T>`,
  - <span class="ru">реализацию `FromIterator` для `Collection<T>`,</span>
- an `Extend` for `Collection<T>`,
  - <span class="ru">реализацию `Extend` для `Collection<T>`,</span>
- `DoubleEndedIterator`, `ExactSizeIterator`, ... as applicable
  - <span class="ru">`DoubleEndedIterator`, `ExactSizeIterator`, ... по мере применимости</span>

In addition, make sure you implement `size_hint()` on all iterators and do so truthfully.

<span class="ru">Кроме того, убедитесь, что вы реализуете `size_hint()` для всех итераторов, и делаете это честно (корректно).</span>
