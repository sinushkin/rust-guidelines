<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Identify, profile, optimize the hot path early (M-HOTPATH) { #M-HOTPATH }

<span class="ru">Выявляйте, профилируйте и оптимизируйте горячий путь на раннем этапе (M-HOTPATH)</span>

<why>high-performance code.</why>

<span class="ru">высокопроизводительный код.</span>

You should, early in the development process, identify if your crate is performance or COGS relevant. If it is:

<span class="ru">На раннем этапе разработки вам следует определить, важен ли ваш крейт для производительности или COGS. Если да:</span>

- identify hot paths and create benchmarks around them,
  - <span class="ru">выявите горячие пути и создайте вокруг них бенчмарки,</span>
- regularly run a profiler collecting CPU and allocation insights,
  - <span class="ru">регулярно запускайте профилировщик, собирающий данные о CPU и аллокациях,</span>
- document or communicate the most performance sensitive areas.
  - <span class="ru">документируйте или сообщайте о наиболее чувствительных к производительности участках.</span>

For benchmarks we recommend [criterion](https://crates.io/crates/criterion) or [divan](https://crates.io/crates/divan).
If possible, benchmarks should not only measure elapsed wall time, but also used CPU time over all threads (this unfortunately
requires manual work and is not supported out of the box by the common benchmark utils).

<span class="ru">Для бенчмарков мы рекомендуем [criterion](https://crates.io/crates/criterion) или [divan](https://crates.io/crates/divan). По возможности бенчмарки должны измерять не только прошедшее время, но и использованное процессорное время по всем потокам (к сожалению, это требует ручной работы и не поддерживается из коробки common-утилитами для бенчмарков).</span>

Profiling Rust on Windows works out of the box with [Intel VTune](https://www.intel.com/content/www/us/en/developer/tools/oneapi/vtune-profiler.html)
and [Superluminal](https://superluminal.eu/). However, to gain meaningful CPU insights you should enable debug symbols for benchmarks in your `Cargo.toml`:

<span class="ru">Профилирование Rust в Windows работает из коробки с [Intel VTune](https://www.intel.com/content/www/us/en/developer/tools/oneapi/vtune-profiler.html) и [Superluminal](https://superluminal.eu/). Однако, чтобы получить содержательные данные о CPU, включите отладочные символы для бенчмарков в вашем `Cargo.toml`:</span>

```toml
[profile.bench]
debug = 1
```

Documenting the most performance sensitive areas helps other contributors take better decision. This can be as simple as
sharing screenshots of your latest profiling hot spots.

<span class="ru">Документирование наиболее чувствительных к производительности участков помогает другим участникам принимать более взвешенные решения. Это может быть так же просто, как публикация снимков последних горячих точек профилирования.</span>

### Further Reading

<span class="ru">Дополнительная литература</span>

- [Performance Tips](https://cheats.rs/#performance-tips)
  - <span class="ru">[Советы по производительности](https://cheats.rs/#performance-tips)</span>

> ### <tip></tip> How much faster?
>
> <span class="ru">Насколько быстрее?</span>
>
> Some of the most common 'language related' issues we have seen include:
>
> <span class="ru">Некоторые из самых распространённых проблем, связанных с языком, которые мы встречали:</span>
>
> - frequent re-allocations, esp. cloned, growing or `format!` assembled strings,
>   - <span class="ru">частые повторные аллокации, особенно при клонировании, росте или сборке строк через `format!`,</span>
> - short lived allocations over bump allocations or similar,
>   - <span class="ru">короткоживущие аллокации вместо bump-аллокаций или подобных,</span>
> - memory copy overhead that comes from cloning Strings and collections,
>   - <span class="ru">накладные расходы на копирование памяти при клонировании `String` и коллекций,</span>
> - repeated re-hashing of equal data structures
>   - <span class="ru">повторное хеширование одинаковых структур данных,</span>
> - the use of Rust's default hasher where collision resistance wasn't an issue
>   - <span class="ru">использование стандартного хешера Rust там, где устойчивость к коллизиям не требовалась.</span>
>
> Anecdotally, we have seen ~15% benchmark gains on hot paths where only some of these `String`  problems were
> addressed, and it appears that up to 50% could be achieved in highly optimized versions.
>
> <span class="ru">По наблюдениям, мы видели прирост ~15% в бенчмарках горячих путей, где устранили лишь часть проблем с `String`, и похоже, что в сильно оптимизированных версиях можно достичь до 50%.</span>
