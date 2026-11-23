<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Prefer 'macros by example' over proc macros (M-EXAMPLE-OVER-PROC) { #M-EXAMPLE-OVER-PROC }

<span class="ru">Предпочитайте «macros by example» перед процедурными макросами</span>

<why>easy macro inspection and fast compilation.</why>

<span class="ru">простая проверка макросов и быстрая компиляция.</span>

When a 'macro by example' can do the job, it should be preferred over proc macros.

<span class="ru">Когда задачу может решить «macro by example», следует предпочесть его процедурным макросам.</span>

Proc macros are more powerful, but their expansion can't easily be inspected. Where this versatility isn't needed, a simple 'macro by example' is the better option.

<span class="ru">Процедурные макросы мощнее, но их раскрытие сложно проверить. Там, где такая универсальность не нужна, лучшим вариантом будет простой «macro by example».</span>

```rust,ignore
// Bad, attribute macro requires proc macro machinery, can be hard to 
// inspect in some IDEs, and isn't needed here.
#[make_new_id]
struct MyId;

// Good, easier to write, maintain and inspect, faster compilation speed.
make_new_id!(MyId);
```
