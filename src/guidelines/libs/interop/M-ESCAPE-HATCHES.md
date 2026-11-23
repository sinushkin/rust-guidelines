<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Native escape hatches (M-ESCAPE-HATCHES) { #M-ESCAPE-HATCHES }

<span class="ru">Нативные «запасные выходы» (M-ESCAPE-HATCHES)</span>

<why>workarounds for unsupported use cases until alternatives exist.</why>

<span class="ru">обходные пути для неподдерживаемых сценариев, пока не появятся альтернативы.</span>

Types wrapping native handles should provide `unsafe` escape hatches. In interop scenarios your users might have gotten a native handle from somewhere
else, or they might have to pass your wrapped handle over FFI. To enable these use cases you should provide `unsafe` conversion methods.

<span class="ru">Типы, оборачивающие нативные дескрипторы, должны предоставлять `unsafe` «запасные выходы». В сценариях интероперабельности пользователи могут получить нативный дескриптор откуда-то ещё или им может понадобиться передать ваш обёрнутый дескриптор через FFI. Чтобы поддержать такие сценарии, следует предоставить `unsafe`-методы преобразования.</span>

```rust
# type HNATIVE = *const u8;
pub struct Handle(HNATIVE);

impl Handle {
    pub fn new() -> Self {
        // Safely creates handle via API calls
        # todo!()
    }

    // Constructs a new Handle from a native handle the user got elsewhere.
    // This method  should then also document all safety requirements that
    // must be fulfilled.
    pub unsafe fn from_native(native: HNATIVE) -> Self {
        Self(native)
    }

    // Various extra methods to permanently or temporarily obtain
    // a native handle.
    pub fn into_native(self) -> HNATIVE { self.0 }
    pub fn to_native(&self) -> HNATIVE { self.0 }
}
```
