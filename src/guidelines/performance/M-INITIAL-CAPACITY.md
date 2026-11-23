<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Collections are created with sufficient initial capacity (M-INITIAL-CAPACITY) { #M-INITIAL-CAPACITY }

<span class="ru">Коллекции создаются с достаточной начальной ёмкостью (M-INITIAL-CAPACITY)</span>

<why>efficient collection creation.</why>

<span class="ru">эффективное создание коллекций.</span>

Where the final or approximate size of a collection (`Vec`, `String`, `HashMap`, `HashSet`, etc.) is known at construction time, it should be created via   `with_capacity` rather than `new` or `default`.

<span class="ru">Если конечный или примерный размер коллекции (`Vec`, `String`, `HashMap`, `HashSet` и т.д.) известен на момент построения, её следует создавать через `with_capacity`, а не через `new` или `default`.</span>

Collections created without capacity may be re-allocated multiple times during their initialization, which also includes copying their content. Creating them with sufficient capacity can entirely avoid this needless overhead.

<span class="ru">Коллекции, созданные без указания ёмкости, могут перевыделять память несколько раз во время инициализации, что также включает копирование их содержимого. Создание с достаточной ёмкостью позволяет полностью избежать этих лишних накладных расходов.</span>

```rust,ignore
// Bad, probably re-allocates and copies content over multiple times.
let mut rval = Vec::new();
for x in &other {
    rval.push(convert(x));
}

// Better, creates collection with sufficient capacity upfront.
let mut rval = Vec::with_capacity(other.len());
for x in &other {
    rval.push(convert(x));
}
```

Iterator-driven construction (`collect`) inherits this behavior via `size_hint` and should be preferred over manual `push` loops when possible:

<span class="ru">Построение через итераторы (`collect`) наследует это поведение через `size_hint`, и при возможности его следует предпочитать ручным циклам с `push`:</span>

```rust,ignore
// Ideal, looks nicer and is performant
let rval: Vec<_> = other.iter().map(convert).collect();
```
