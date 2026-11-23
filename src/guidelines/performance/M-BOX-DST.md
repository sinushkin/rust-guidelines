<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Use boxed slices and strings for immutable owned sequences (M-BOX-DST) { #M-BOX-DST }

<span class="ru">Используйте boxed-срезы и строки для неизменяемых владеющих последовательностей (M-BOX-DST)</span>

<why>low memory consumption and good cache utilization.</why>

<span class="ru">низкое потребление памяти и хорошее использование кэша.</span>

Frequently used, internal, immutable sequences that will not be resized after construction should be stored as `Box<[T]>`, `Arc<str>` or similar, rather than their original  `Vec<T>` or `String` counterparts.

<span class="ru">Часто используемые внутренние неизменяемые последовательности, которые не будут изменять размер после создания, следует хранить как `Box<[T]>`, `Arc<str>` или подобные типы, а не как их исходные аналоги `Vec<T>` или `String`.</span>

Regular growable collections consist of a `(ptr, len, capacity)` triple. Converting them to boxed slices makes them immutable, executes a [shrink-to-fit](./#M-SHRINK-TO-FIT), and drops the `capacity` bit, reducing their handle size by 1/3.  For this pattern to be useful, the following preconditions should apply:

<span class="ru">Обычные растущие коллекции состоят из тройки `(ptr, len, capacity)`. Их преобразование в boxed-срезы делает их неизменяемыми, выполняет [shrink-to-fit](./#M-SHRINK-TO-FIT) и отбрасывает `capacity`, уменьшая размер дескриптора на 1/3. Чтобы этот паттерн был полезен, должны выполняться следующие условия:</span>

- the sequence should be frequently instantiated (e.g., >1000's of instances),
  - <span class="ru">последовательность должна часто создаваться (например, >1000 экземпляров),</span>
- it must be immutable,
  - <span class="ru">она должна быть неизменяемой,</span>
- it should not be user-visible, i.e., regular users would just deal with `&str` or similar.
  - <span class="ru">она не должна быть видна пользователю, то есть обычные пользователи должны работать с `&str` или подобным.</span>

Some collections provide dedicated methods for this, e.g., `String::into_boxed_str`.

<span class="ru">Некоторые коллекции предоставляют для этого специальные методы, например `String::into_boxed_str`.</span>

```rust,ignore
// Bad, with many entries this wastes space and makes
// traversal ultimately slower. 
struct Data {
    ids: Vec<String>
}

// Good, reduces memory consumption and fits more elements 
// into cache.
struct Data {
    ids: Vec<Box<str>>
}
```
