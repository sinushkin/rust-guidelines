<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Items are only visible through one path (M-SINGLE-ITEM-PATH) { #M-SINGLE-ITEM-PATH }

<span class="ru">Элементы видны только по одному пути (M-SINGLE-ITEM-PATH)</span>

<why>a single, clutter-free path to each type.</why>

<span class="ru">один понятный путь к каждому типу без лишнего.</span>

Public items within a crate should be reachable only through one path. For example some `crate::db::Connection` should not also be visible as `crate::Connection`:

<span class="ru">Публичные элементы внутри крейта должны быть доступны только по одному пути. Например, `crate::db::Connection` не должен также быть виден как `crate::Connection`:</span>

```rust,ignore
// Not OK
pub mod db {
    pub struct Connection;
}

pub use db::Connection;
```

This rule is often violated by agents creating or refactoring large code bases over several iterations. In an attempt to _simplify_ their task, they re-export items under multiple paths, often previous ones from before some change, instead of cleanly redesigning structures where it makes sense.

<span class="ru">Это правило часто нарушают агенты, создающие или рефакторящие большие кодовые базы в несколько итераций. Пытаясь _упростить_ себе задачу, они реэкспортируют элементы под несколькими путями, часто старыми, оставшимися от прежней версии, вместо того чтобы аккуратно перепроектировать структуры там, где это имеет смысл.</span>

Note this only targets the duplication of user-facing items. Within a crate it is acceptable (and often unavoidable) to see the same item multiple times as export trees are constructed:

<span class="ru">Обратите внимание: правило касается только дублирования пользовательских элементов. Внутри крейта допустимо (а часто и неизбежно) видеть один и тот же элемент несколько раз при построении дерева экспорта:</span>

```rust,ignore
// OK
pub(crate) mod db {
    pub struct Connection;
}

pub use db::Connection;
```

Similarly, re-exports of foreign items are not covered by this rule, although they should follow [M-FOREIGN-REEXPORTS](../libs/interop/#M-FOREIGN-REEXPORTS).

<span class="ru">Аналогично, реэкспорты чужих элементов этим правилом не охватываются, хотя им следует соответствовать [M-FOREIGN-REEXPORTS](../libs/interop/#M-FOREIGN-REEXPORTS).</span>

Likewise, this rule also does not apply to public-but-hidden `_private` modules needed by macros, compare [M-MACRO-HELPERS](../macros/#M-MACRO-HELPERS).

<span class="ru">Также это правило не распространяется на публичные, но скрытые модули `_private`, нужные макросам; см. [M-MACRO-HELPERS](../macros/#M-MACRO-HELPERS).</span>
