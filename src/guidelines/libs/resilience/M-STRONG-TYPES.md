<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Use the proper type family (M-STRONG-TYPES) { #M-STRONG-TYPES }

<span class="ru">Используйте подходящее семейство типов</span>

<why>the right data and safety invariants, at the right time.</why>

<span class="ru">правильные данные и инварианты безопасности в нужный момент.</span>

Use the appropriate `std` type for your task. In general you should use the strongest type available, as early as possible in your API flow. Common offenders are

<span class="ru">Используйте подходящий тип из `std` для своей задачи. В общем случае следует использовать самый сильный доступный тип как можно раньше в потоке API. Типичные ошибки:</span>

| Do not use ... | use instead ... | Explanation |
| --- | --- | --- |
| `String`* | `PathBuf`* | Anything dealing with the OS should be `Path`-like |

| <span class="ru">Не используйте ...</span> | <span class="ru">используйте вместо этого ...</span> | <span class="ru">Всё, что связано с ОС, должно быть похоже на `Path`</span> |
| --- | --- | --- |
| `String`* | `PathBuf`* | <span class="ru">Всё, что связано с ОС, должно быть похоже на `Path`</span> |

That said, you should also follow common Rust `std` conventions. Purely numeric types at public API boundaries (e.g., `window_size()`) are expected to
be regular numbers, not `Saturating<usize>`, `NonZero<usize>`, or similar.

<span class="ru">Впрочем, следует также придерживаться общепринятых соглашений Rust `std`. Чисто числовые типы на публичных границах API (например, `window_size()`) должны быть обычными числами, а не `Saturating<usize>`, `NonZero<usize>` или подобными.</span>

<footnotes>

<sup>*</sup> Including their siblings, e.g., `&str`, `Path`, ...

<span class="ru"><sup>*</sup> Включая их аналоги, например `&str`, `Path`, ...</span>

</footnotes>
