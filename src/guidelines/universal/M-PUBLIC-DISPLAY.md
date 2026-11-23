<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Public types meant to be read are Display (M-PUBLIC-DISPLAY) { #M-PUBLIC-DISPLAY }

<span class="ru">Публичные типы, предназначенные для чтения, реализуют Display (M-PUBLIC-DISPLAY)</span>

<why>usability.</why>

<span class="ru">удобство использования.</span>

If your type is expected to be read by upstream consumers, be it developers or end users, it should implement `Display`. This in particular includes:

<span class="ru">Если ваш тип предполагается читать внешним потребителям — разработчикам или конечным пользователям, — он должен реализовывать `Display`. В частности, сюда относятся:</span>

- Error types, which are mandated by `std::error::Error` to implement `Display`
  - <span class="ru">типы ошибок, которые `std::error::Error` обязывает реализовывать `Display`</span>
- Wrappers around string-like data
  - <span class="ru">обёртки вокруг строкоподобных данных</span>

Implementations of `Display` should follow Rust customs; this includes rendering newlines and escape sequences.
The handling of sensitive data outlined in [M-PUBLIC-DEBUG] applies analogously.

<span class="ru">Реализации `Display` должны следовать соглашениям Rust, включая обработку переводов строк и escape-последовательностей. Обращение с чувствительными данными, описанное в [M-PUBLIC-DEBUG], применяется аналогично.</span>

[M-PUBLIC-DEBUG]: ./#M-PUBLIC-DEBUG
