<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Mark `pub use` items with `#[doc(inline)]` (M-DOC-INLINE) { #M-DOC-INLINE }

<span class="ru">Помечайте элементы `pub use` атрибутом `#[doc(inline)]` (M-DOC-INLINE)</span>

<why>re-exported items that fit in with their siblings.</why>

<span class="ru">реэкспортированные элементы, которые вписываются в ряд соседних элементов.</span>

When publicly re-exporting crate items via `pub use foo::Foo` or `pub use foo::*`, they show up in an opaque re-export block. In most cases, this is not
helpful to the reader:

<span class="ru">При публичном реэкспорте элементов крейта через `pub use foo::Foo` или `pub use foo::*` они попадают в непрозрачный блок реэкспорта. В большинстве случаев это не помогает читателю:</span>

![TEXT](M-DOC-INLINE_BAD.png)

Instead, you should annotate them with `#[doc(inline)]` at the `use` site, for them to be inlined organically:

<span class="ru">Вместо этого следует пометить их `#[doc(inline)]` в месте `use`, чтобы они органично встраивались в документацию:</span>

```rust,edition2021,ignore
# pub(crate) mod foo { pub struct Foo; }
#[doc(inline)]
pub use foo::*;

// or

#[doc(inline)]
pub use foo::Foo;
```

![TEXT](M-DOC-INLINE_GOOD.png)

This does not apply to `std` or 3rd party types; these should always be re-exported without inlining to make it clear they are external.

<span class="ru">Это не относится к типам `std` и сторонних крейтов: их следует всегда реэкспортировать без инлайнинга, чтобы было ясно, что они внешние.</span>

> ### <alert></alert> Still avoid glob exports
>
> <span class="ru">Всё равно избегайте glob-экспортов</span>
>
> The `#[doc(inline)]` trick above does not change [M-NO-GLOB-REEXPORTS]; you generally should not re-export items via wildcards.
>
> <span class="ru">Приём `#[doc(inline)]` выше не меняет [M-NO-GLOB-REEXPORTS]: в целом не следует реэкспортировать элементы через подстановочные знаки.</span>

[M-NO-GLOB-REEXPORTS]: ../libs/resilience/#M-NO-GLOB-REEXPORTS
