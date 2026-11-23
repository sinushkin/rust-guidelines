<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Lint overrides should use `#[expect]` (M-LINT-OVERRIDE-EXPECT) { #M-LINT-OVERRIDE-EXPECT }

<span class="ru">Переопределения линтов следует делать через `#[expect]` (M-LINT-OVERRIDE-EXPECT)</span>

<why>a current, tidy lint set.</why>

<span class="ru">актуальный и аккуратный набор линтов.</span>

When overriding project-global lints inside a submodule or item, you should do so via `#[expect]`, not `#[allow]`.

<span class="ru">При переопределении глобальных для проекта линтов внутри подмодуля или элемента следует делать это через `#[expect]`, а не `#[allow]`.</span>

Expected lints emit a warning if the marked warning was not encountered, thus preventing the accumulation of stale lints.
That said, `#[allow]` lints are still useful when applied to generated code, and can appear in macros.

<span class="ru">Ожидаемые линты выдают предупреждение, если помеченное предупреждение не встретилось, тем самым предотвращая накопление устаревших линтов. Впрочем, `#[allow]` по-прежнему полезен для сгенерированного кода и может появляться в макросах.</span>

Overrides should be accompanied by a `reason`:

<span class="ru">Переопределения должны сопровождаться `reason`:</span>

```rust,edition2021
#[expect(clippy::unused_async, reason = "API fixed, will use I/O later")]
pub async fn ping_server() {
  // Stubbed out for now
}
```
