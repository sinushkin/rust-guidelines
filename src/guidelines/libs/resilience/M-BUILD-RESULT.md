<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Builders validate in final `.build()` (M-BUILD-RESULT) { #M-BUILD-RESULT }

<span class="ru">Билдеры проверяют данные в финальном `.build()`</span>

<why>clean builder error handling.</why>

<span class="ru">чистая обработка ошибок билдера.</span>

A builder's per-field setters should accept input without failing, final validation should be done by `.build()`.

<span class="ru">Сеттеры билдера для отдельных полей должны принимать данные без ошибок, а финальная проверка должна выполняться в `.build()`.</span>

Fallible setters add noise, and still don't guard against interdependent error conditions. Where builders are fallible they should offer a `Result`-carrying `.build()` instead.

<span class="ru">Сеттеры, которые могут завершиться ошибкой, добавляют шум и всё равно не защищают от ошибок, зависящих от нескольких полей. Если билдеры могут завершаться ошибкой, им вместо этого следует предлагать `.build()`, возвращающий `Result`.</span>

```rust,ignore
// Bad, forces repeated error checks that provide no value.
Foo::builder()
    .name("Foo")?
    .distance(42)?
    .build();

// Good, consolidates sanity checking and allows for cross-checks 
// between properties.
Foo::builder()
    .name("Foo")
    .distance(42)
    .build()?;
```

That said, individual settings should prefer strong types carrying their own validation where applicable, compare M-STRONG-TYPES-GUARD.

<span class="ru">При этом отдельные настройки, где это уместно, должны предпочитать сильные типы, содержащие собственную проверку, см. M-STRONG-TYPES-GUARD.</span>
