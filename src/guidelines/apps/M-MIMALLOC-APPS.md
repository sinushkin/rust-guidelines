<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Use mimalloc for apps (M-MIMALLOC-APPS) { #M-MIMALLOC-APPS }

<span class="ru">Использовать mimalloc в приложениях (M-MIMALLOC-APPS)</span>

<why>significant performance at no cost.</why>

<span class="ru">значительный прирост производительности без дополнительных затрат.</span>

Applications should set [mimalloc](https://crates.io/crates/mimalloc) as their global allocator. This usually results in notable performance
increases along allocating hot paths; we have seen up to 25% benchmark improvements.

<span class="ru">Приложения должны устанавливать [mimalloc](https://crates.io/crates/mimalloc) в качестве глобального аллокатора. Обычно это заметно повышает производительность на горячих путях выделения памяти; мы наблюдали улучшение бенчмарков до 25%.</span>

Changing the allocator only takes a few lines of code. Add mimalloc to your `Cargo.toml` like so:

<span class="ru">Замена аллокатора занимает всего несколько строк кода. Добавьте mimalloc в `Cargo.toml` так:</span>

```toml
[dependencies]
mimalloc = { version = "0.1" } # Or later version if available
```

Then use it from your `main.rs`:

```rust,ignore
use mimalloc::MiMalloc;

#[global_allocator]
static GLOBAL: MiMalloc = MiMalloc;
```
