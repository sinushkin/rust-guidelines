<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Magic values are documented (M-DOCUMENTED-MAGIC) { #M-DOCUMENTED-MAGIC }

<span class="ru">Магические значения документируются (M-DOCUMENTED-MAGIC)</span>

<why>maintainability and safe refactoring.</why>

<span class="ru">сопровождаемость и безопасный рефакторинг.</span>

Hardcoded _magic_ values in production code must be accompanied by a comment. The comment should outline:

<span class="ru">Жёстко заданные _магические_ значения в продакшн-коде должны сопровождаться комментарием. Комментарий должен описывать:</span>

- why this value was chosen,
  - <span class="ru">почему выбрано это значение,</span>
- non-obvious side effects if that value is changed,
  - <span class="ru">неочевидные побочные эффекты при изменении этого значения,</span>
- external systems that interact with this constant.
  - <span class="ru">внешние системы, которые взаимодействуют с этой константой.</span>

You should prefer named constants over inline values.

<span class="ru">Предпочитайте именованные константы встроенным значениям.</span>

```rust, ignore
// Bad: it's relatively obvious that this waits for a day, but not why
wait_timeout(60 * 60 * 24).await // Wait at most a day

// Better
wait_timeout(60 * 60 * 24).await // Large enough value to ensure the server
                                 // can finish. Setting this too low might
                                 // make us abort a valid request. Based on
                                 // `api.foo.com` timeout policies.

// Best

/// How long we wait for the server.
///
/// Large enough value to ensure the server
/// can finish. Setting this too low might
/// make us abort a valid request. Based on
/// `api.foo.com` timeout policies.
const UPSTREAM_SERVER_TIMEOUT: Duration = Duration::from_secs(60 * 60 * 24);
```
