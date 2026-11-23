<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Don't define preludes (M-NO-PRELUDE) { #M-NO-PRELUDE }

<span class="ru">Не определяйте прелюдии (M-NO-PRELUDE)</span>

<why>a clean namespace and reliable downstream builds.</why>

<span class="ru">чистое пространство имён и надёжная сборка у downstream-потребителей.</span>

Crates must not define a `prelude` or any namespace intended to be imported as `use foo::*`.

<span class="ru">Крейты не должны определять `prelude` или любое пространство имён, предназначенное для импорта как `use foo::*`.</span>

While the Rust Standard Library successfully uses [preludes](https://doc.rust-lang.org/std/prelude/index.html) to define edition items, preludes in crates cause more harm than good. Given today's IDE support they are not needed, and once multiple preludes are used from different crates there is potential for conflicts:

<span class="ru">Хотя стандартная библиотека Rust успешно использует [прелюдии](https://doc.rust-lang.org/std/prelude/index.html) для определения элементов редакции, прелюдии в крейтах приносят больше вреда, чем пользы. С учётом современной поддержки в IDE они не нужны, а при использовании нескольких прелюдий из разных крейтов возникает потенциал для конфликтов:</span>

```rust,ignore
use foo::prelude::*;
use bar::prelude::*;
use baz::prelude::*;

_ = Client::new();

// error[E0659]: `Client` is ambiguous
//   --> src/lib.rs:17:13
//    |
// 17 |     _ = Client; 
//    |         ^^^^^^ ambiguous name
//    |
//    = note: ambiguous because of multiple glob imports of a name in the same module
```

Preludes in particular do not resolve bad module design. If it looks like a prelude would make the crate easier to use or understand, this is almost always an indication that the existing module system needs restructuring, see [M-BALANCED-MODULES](./#M-BALANCED-MODULES).

<span class="ru">В частности, прелюдии не решают проблему плохого проектирования модулей. Если кажется, что прелюдия сделала бы крейт проще в использовании или понимании, это почти всегда признак того, что существующую систему модулей нужно перестроить, см. [M-BALANCED-MODULES](./#M-BALANCED-MODULES).</span>
