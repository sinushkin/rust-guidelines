<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Shrink collections to fit after building (M-SHRINK-TO-FIT) { #M-SHRINK-TO-FIT }

<span class="ru">Уменьшайте коллекции до нужного размера после построения (M-SHRINK-TO-FIT)</span>

<why>a minimal memory footprint.</why>

<span class="ru">минимальный объём памяти.</span>

Where large, long-lived, growable collections such as `Vec` or `String` were built without an exact size reservation (compare [M-INITIAL-CAPACITY](./#M-INITIAL-CAPACITY)), the resulting collection should be shrunk via `shrink_to_fit` before storing it.

<span class="ru">Если большие, долгоживущие, растущие коллекции, такие как `Vec` или `String`, были построены без точного резервирования размера (сравните [M-INITIAL-CAPACITY](./#M-INITIAL-CAPACITY)), перед сохранением коллекцию следует уменьшить через `shrink_to_fit`.</span>

Many Rust collections grow by powers of two when iteratively adding elements. In the worst case a collection might therefore use ~2x of its needed memory.

<span class="ru">Многие коллекции Rust при последовательном добавлении элементов растут степенями двойки. В худшем случае коллекция может использовать ~2x от необходимого объёма памяти.</span>

```rust,ignore
// Bad, long lived object might end up using 2x needed memory.
let mut long_lived = Vec::new();
for x in large_iter {
    long_lived.push(x);
}

// Good, frees up extra memory.
long_lived.shrink_to_fit();
```

Note that this does not apply to conversions done via `into_boxed_*` and friends (compare [M-BOX-DST](./#M-BOX-DST)), as these generally shrink before converting already.

<span class="ru">Обратите внимание, что это не относится к преобразованиям через `into_boxed_*` и подобные (сравните [M-BOX-DST](./#M-BOX-DST)), так как они, как правило, уже уменьшают размер перед преобразованием.</span>
