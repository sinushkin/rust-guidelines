<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Accept `impl RangeBounds<>` where feasible (M-IMPL-RANGEBOUNDS) { #M-IMPL-RANGEBOUNDS }

<span class="ru">Принимайте `impl RangeBounds<>`, где это возможно (M-IMPL-RANGEBOUNDS)</span>

<why>flexibility and clarity when specifying ranges.</why>

<span class="ru">гибкость и ясность при задании диапазонов.</span>

Functions that accept a range of numbers must use a `Range` type or trait over hand-rolled parameters:

<span class="ru">Функции, принимающие диапазон чисел, должны использовать тип или трейт `Range` вместо самописных параметров:</span>

```rust,ignore
// Bad
fn select_range(low: usize, high: usize) {}
fn select_range(range: (usize, usize)) {}
```

In addition, functions that can work on arbitrary ranges, should accept `impl RangeBounds<T>` rather than `Range<T>`.

<span class="ru">Кроме того, функции, которые могут работать с произвольными диапазонами, следует принимать `impl RangeBounds<T>`, а не `Range<T>`.</span>

```rust
# use std::ops::{RangeBounds, Range};
// Callers must call with `select_range(1..3)`
fn select_range(r: Range<usize>) {}

// Callers may call as
//     select_any(1..3)
//     select_any(1..)
//     select_any(..)
fn select_any(r: impl RangeBounds<usize>) {}
```
