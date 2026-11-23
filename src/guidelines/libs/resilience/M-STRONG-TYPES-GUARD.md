<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Newtypes guard their invariants (M-STRONG-TYPES-GUARD) { #M-STRONG-TYPES-GUARD }

<span class="ru">Ньютайпы защищают свои инварианты</span>

<why>centralized correctness invariants.</why>

<span class="ru">централизованные инварианты корректности.</span>

When introducing a strong type or newtype that exists to encode an invariant (a non-empty string, a percentage, a port number, a sanitized path, ...), the type itself must enforce that invariant where applicable.

<span class="ru">При введении сильного типа или ньютайпа, который существует для кодирования инварианта (непустая строка, процент, номер порта, очищенный путь, ...), сам тип должен обеспечивать этот инвариант, где это применимо.</span>

Construction should be fallible, returning a proper error when the invariant cannot be upheld, rather than handing the responsibility off to every user:

<span class="ru">Конструирование должно быть способно завершиться ошибкой (fallible): возвращать соответствующую ошибку, если инвариант невозможно соблюсти, а не перекладывать ответственность на каждого пользователя:</span>

```rust,ignore
// Bad, creates a new type but enforces nothing. Every caller now has to
// re-check that the value is actually a valid month, defeating the point of
// having a dedicated type.
pub struct Month(pub u8);

impl Month {
    pub fn new(value: u8) -> Self { ... }
}


// Good, the invariant (1..=12) is checked once, at the boundary, and
// every later use of `Month` can rely on it.
pub struct Month(u8);

impl Month {
    pub fn from_u8(value: u8) -> Result<Self, DateError> { ... }
}
```

This means for any newtype that is non-total:

<span class="ru">Это означает, что для любого ньютайпа, который не является тотальным:</span>

- It must have at least one fallible constructor (e.g., `fn from_foo(...) -> Result<Self, _>`).
  - <span class="ru">Он должен иметь хотя бы один fallible-конструктор (например, `fn from_foo(...) -> Result<Self, _>`).</span>
- Additional panicking constructors are allowed (e.g., `new`), and should preferably be `const`.
  - <span class="ru">Дополнительные паникующие конструкторы допустимы (например, `new`), и предпочтительно делать их `const`.</span>
- Conversions from weaker types into the newtype must be fallible (`TryFrom`/`FromStr`).
  - <span class="ru">Преобразования из более слабых типов в ньютайп должны быть fallible (`TryFrom`/`FromStr`).</span>
- Infallible `From` implementations may not be offered.
  - <span class="ru">Инфаллибельные реализации `From` предлагать нельзя.</span>

> ### <tip></tip> Why `const`?
>
> <span class="ru">Зачем `const`?</span>
>
> Const constructors allows them to be used inside `const {}` blocks, which surfaces these violations as errors. This enables
> users to do `let month_due = const { Month::new(14) }` and avoids hitting these paths during runtime.
>
> <span class="ru">Константные конструкторы можно использовать внутри блоков `const {}`, и тогда такие нарушения выявляются как ошибки компиляции. Это позволяет пользователям писать `let month_due = const { Month::new(14) }` и не попадать на эти пути во время выполнения.</span>
