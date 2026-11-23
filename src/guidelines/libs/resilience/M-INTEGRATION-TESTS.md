<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Integration tests live under `tests/` (M-INTEGRATION-TESTS) { #M-INTEGRATION-TESTS }

<span class="ru">Интеграционные тесты располагаются в `tests/`</span>

<why>clean code files.</why>

<span class="ru">чистые файлы с кодом.</span>

Tests that only touch public API surface are _integration tests_ and belong under `tests/`, not `mod tests {}`.

<span class="ru">Тесты, которые затрагивают только публичную поверхность API, являются _интеграционными тестами_ и должны располагаться в `tests/`, а не в `mod tests {}`.</span>

In projects with coverage targets, it is not uncommon for `src/` files to contain more testing code than actual business logic. This can make browsing and understanding the code harder both in IDEs and PRs. Likewise, if a testing goal can be achieved through either an integration test or a unit test, the former is always preferred.

<span class="ru">В проектах с целевым покрытием нередко файлы в `src/` содержат больше тестового кода, чем бизнес-логики. Это затрудняет просмотр и понимание кода как в IDE, так и в PR. Аналогично, если цель тестирования можно достичь либо интеграционным, либо модульным тестом, предпочтительнее всегда первый.</span>
