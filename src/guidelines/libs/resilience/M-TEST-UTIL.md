<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Test utilities are feature gated (M-TEST-UTIL) { #M-TEST-UTIL }

<span class="ru">Утилиты тестирования закрываются feature-флагом</span>

<why>production builds that cannot bypass safety checks.</why>

<span class="ru">продакшн-сборки, которые не могут обойти проверки безопасности.</span>

Testing functionality must be guarded behind a feature flag. This includes

<span class="ru">Функциональность тестирования должна быть закрыта feature-флагом. Сюда относятся:</span>

- mocking functionality ([M-MOCKABLE-SYSCALLS]),
  - <span class="ru">функциональность мокирования ([M-MOCKABLE-SYSCALLS]),</span>
- the ability to inspect sensitive data,
  - <span class="ru">возможность просматривать чувствительные данные,</span>
- safety check overrides,
  - <span class="ru">переопределение проверок безопасности,</span>
- fake data generation.
  - <span class="ru">генерация фейковых данных.</span>

We recommend you use a single flag only, named `test-util`. In any case, the feature(s) must clearly communicate they are for testing purposes.

<span class="ru">Рекомендуется использовать только один флаг с именем `test-util`. В любом случае feature(ы) должны ясно показывать, что они предназначены для тестирования.</span>

```rust, ignore
impl HttpClient {
    pub fn get() { ... }

    #[cfg(feature = "test-util")]
    pub fn bypass_certificate_checks() { ... }
}
```

[M-MOCKABLE-SYSCALLS]: ./#M-MOCKABLE-SYSCALLS
