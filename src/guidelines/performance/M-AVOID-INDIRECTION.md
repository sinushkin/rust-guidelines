<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Nested type hierarchies should avoid needless indirection (M-AVOID-INDIRECTION) { #M-AVOID-INDIRECTION }

<span class="ru">Вложенные иерархии типов должны избегать ненужной косвенности (M-AVOID-INDIRECTION)</span>

<why>fast, cache-friendly memory access.</why>

<span class="ru">быстрый доступ к памяти, дружественный к кэшу.</span>

Hot types should avoid nested heap indirection and consider lifting hot, cacheable deep fields to improve cache utilization.  

<span class="ru">Горячие типы должны избегать вложенной косвенности через кучу и рассматривать вынос часто кэшируемых глубоких полей наверх для улучшения использования кэша.</span>

While the gold standard is to benchmark, a pattern that emerges repeatedly when porting C# code to Rust is to reflexively `Arc` nested types, often multiple layers deep. Although this can make sense on very wide or heavyweight types that genuinely need to be shared by multiple owners, this pattern can ruin access latency when multiple rounds of DRAM lookup have to be performed sequentially.

<span class="ru">Золотой стандарт — измерять производительность с помощью бенчмарков. Но при переносе кода с C# на Rust часто повторяется паттерн: рефлекторно оборачивать вложенные типы в `Arc`, зачастую в несколько слоёв. Хотя это может иметь смысл для очень широких или тяжёлых типов, которые действительно нужно разделять между несколькими владельцами, этот паттерн может сильно ухудшить задержку доступа, когда несколько раундов поиска в DRAM приходится выполнять последовательно.</span>

Where nested, shared ownership isn't strictly needed, it is usually better to start with local, embedded data, and lift cacheable fields.

<span class="ru">Там, где вложенное разделяемое владение строго не требуется, обычно лучше начать с локальных встроенных данных и вынести наверх кэшируемые поля.</span>

```rust,ignore
// Bad, `print` (assuming it is reasonably hot) needs 2 indirections 
// to query whether it is enabled. 
struct Item {
    config: Arc<Config>,
    payload: Payload,
}

struct Config {
    feature: Arc<Feature>
}

impl Item {
    fn print(&self) {
        if self.config.feature.is_enabled() { ... }
    }
}

// Better: `enabled` resides nearby and is likely immediately available 
// once `print` is called.
struct Item {
    config: Arc<Config>,
    payload: Payload,
    enabled: bool,
}

impl Item {
    fn print(&self) {
        if self.enabled { ... }
    }
}

```
