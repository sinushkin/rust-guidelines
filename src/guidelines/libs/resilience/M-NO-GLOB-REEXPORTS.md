<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Don't glob re-export items (M-NO-GLOB-REEXPORTS) { #M-NO-GLOB-REEXPORTS }

<span class="ru">Не реэкспортируйте элементы через glob</span>

<why>a deliberate public surface.</why>

<span class="ru">продуманная публичная поверхность.</span>

Don't `pub use foo::*` from other modules, especially not from other crates. You might accidentally export more than you want,
and globs are hard to review in PRs. Re-export items individually instead:

<span class="ru">Не используйте `pub use foo::*` в других модулях, тем более в других крейтах. Можно случайно экспортировать больше, чем нужно, а glob-экспорты трудно проверять в PR. Реэкспортируйте элементы по отдельности:</span>

```rust,ignore
pub use foo::{A, B, C};
```

Glob exports are permissible for technical reasons, like doing platform specific re-exports from a set of HAL (hardware abstraction layer) modules:

<span class="ru">Glob-экспорты допустимы по техническим причинам, например при платформенно-специфичных реэкспортах из набора модулей HAL (hardware abstraction layer):</span>

```rust,ignore
#[cfg(target_os = "windows")]
mod windows { /* ... */ }

#[cfg(target_os = "linux")]
mod linux { /* ... */ }

// Acceptable use of glob re-exports, this is a common pattern
// and it is clear everything is just forwarded from a single 
// platform.

#[cfg(target_os = "windows")]
pub use windows::*;

#[cfg(target_os = "linux")]
pub use linux::*;
```
