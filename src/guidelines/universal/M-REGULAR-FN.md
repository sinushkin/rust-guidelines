<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Prefer regular over associated functions (M-REGULAR-FN) { #M-REGULAR-FN }

<span class="ru">Предпочитайте обычные функции ассоциированным (M-REGULAR-FN)</span>

<why>readability.</why>

<span class="ru">читаемость.</span>

Associated functions should primarily be used for instance creation, not general purpose computation.

<span class="ru">Ассоциированные функции следует использовать в первую очередь для создания экземпляров, а не для универсальных вычислений.</span>

In contrast to some OO languages, regular functions are first-class citizens in Rust and need no module or _class_ to host them. Functionality that
does not clearly belong to a receiver should therefore not reside in a type's `impl` block:

<span class="ru">В отличие от некоторых ООП-языков, обычные функции в Rust — полноправные элементы языка, и им не нужны модуль или _класс_, чтобы их разместить. Поэтому функциональность, которая явно не относится к получателю (receiver), не должна находиться в блоке `impl` типа:</span>

```rust, ignore
struct Database {}

impl Database {
    // Ok, associated function creates an instance
    fn new() -> Self {}

    // Ok, regular method with `&self` as receiver
    fn query(&self) {}

    // Not ok, this function is not directly related to `Database`,
    // it should therefore not live under `Database` as an associated
    // function.
    fn check_parameters(p: &str) {}
}

// As a regular function this is fine
fn check_parameters(p: &str) {}
```

Regular functions are more idiomatic, and reduce unnecessary noise on the caller side. Associated trait functions are perfectly idiomatic though:

<span class="ru">Обычные функции более идиоматичны и уменьшают лишний шум на стороне вызова. Впрочем, ассоциированные функции трейтов тоже вполне идиоматичны:</span>

```rust
pub trait Default {
    fn default() -> Self;
}

struct Foo;

impl Default for Foo {
    fn default() -> Self { Self }
}
```
