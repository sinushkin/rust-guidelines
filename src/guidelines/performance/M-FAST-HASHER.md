<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Use a fast hasher where possible (M-FAST-HASHER) { #M-FAST-HASHER }

<span class="ru">Используйте быстрый хешер там, где это возможно (M-FAST-HASHER)</span>

<why>hashing performance.</why>

<span class="ru">производительность хеширования.</span>

When hashing trusted, internal keys, prefer a fast non-cryptographic hasher (e.g., `foldhash`, `FxHash`) over the standard library default.

<span class="ru">При хешировании доверенных внутренних ключей предпочитайте быстрый некриптографический хешер (например, `foldhash`, `FxHash`) стандартному хешеру из библиотеки по умолчанию.</span>

Rust's default hasher is reasonably DoS safe on untrusted user input, but this comes at a performance penalty. If you can trust that keys are not maliciously crafted to overflow individual buckets, a custom fast hasher can yield significant performance gains.

<span class="ru">Стандартный хешер Rust достаточно устойчив к DoS-атакам на недоверенный пользовательский ввод, но это стоит производительности. Если вы можете быть уверены, что ключи не сформированы намеренно так, чтобы переполнять отдельные корзины, пользовательский быстрый хешер может дать существенный прирост производительности.</span>

```rust,ignore
// Bad, uses default hasher for keys we control.
let lookup = HashMap::<UserID, Data>::with_capacity(1024);

// Good, uses faster foldhash for internal keys.
let lookup = foldhash::HashMap<UserID, Data>::with_capacity(1024);
```
