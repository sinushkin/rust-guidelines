<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Applications may use Anyhow or derivatives (M-APP-ERROR) { #M-APP-ERROR }

<span class="ru">Приложения могут использовать Anyhow или его производные (M-APP-ERROR)</span>

<why>simple application-level error handling.</why>

<span class="ru">простая обработка ошибок на уровне приложения.</span>

> Note, this guideline is primarily a relaxation and clarification of [M-ERRORS-CANONICAL-STRUCTS].
>
> <span class="ru">Обратите внимание: это руководство в первую очередь смягчает и уточняет [M-ERRORS-CANONICAL-STRUCTS].</span>

Applications, and crates in your own repository exclusively used from your application, may use [ohno::AppError](https://docs.rs/crate/ohno/latest#structs), [anyhow](https://github.com/dtolnay/anyhow),
[eyre](https://github.com/eyre-rs/eyre) or similar application-level error crates instead of implementing their own types.

<span class="ru">Приложения, а также крейты в вашем репозитории, которые используются исключительно вашим приложением, могут использовать [ohno::AppError](https://docs.rs/crate/ohno/latest#structs), [anyhow](https://github.com/dtolnay/anyhow), [eyre](https://github.com/eyre-rs/eyre) или похожие крейты обработки ошибок уровня приложения вместо реализации собственных типов.</span>

For example, in your application crates you may just re-export and use eyre's common `Result` type, which should be able to automatically
handle all third party library errors, in particular the ones following
[M-ERRORS-CANONICAL-STRUCTS].

<span class="ru">Например, в крейтах вашего приложения можно просто реэкспортировать и использовать общий тип `Result` из eyre, который должен автоматически обрабатывать все ошибки сторонних библиотек, в особенности те, что следуют [M-ERRORS-CANONICAL-STRUCTS].</span>

```rust,ignore
use ohno::AppError;

fn start_application() -> Result<(), AppError> {
    start_server()?;
    Ok(())
}
```

Once you selected your application error crate you should switch all application-level errors to that type, and you should not mix multiple
application-level error types.

<span class="ru">Выбрав крейт для ошибок приложения, переведите все ошибки уровня приложения на этот тип и не смешивайте несколько типов ошибок уровня приложения.</span>

Libraries (crates used by more than one crate) should always follow [M-ERRORS-CANONICAL-STRUCTS] instead.

<span class="ru">Библиотеки (крейты, которые используются более чем одним крейтом) должны вместо этого всегда следовать [M-ERRORS-CANONICAL-STRUCTS].</span>

[M-ERRORS-CANONICAL-STRUCTS]: ../libs/ux/#M-ERRORS-CANONICAL-STRUCTS
