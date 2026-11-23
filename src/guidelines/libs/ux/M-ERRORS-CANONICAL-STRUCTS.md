<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Errors are canonical structs (M-ERRORS-CANONICAL-STRUCTS) { #M-ERRORS-CANONICAL-STRUCTS }

<span class="ru">Ошибки — это канонические структуры (M-ERRORS-CANONICAL-STRUCTS)</span>

<why>harmonized error types and consistent error handling.</why>

<span class="ru">согласованные типы ошибок и единообразная обработка ошибок.</span>

Errors should be a situation-specific `struct` that contain a [`Backtrace`](https://doc.rust-lang.org/stable/std/backtrace/struct.Backtrace.html),
a possible upstream error cause, and helper methods.

<span class="ru">Ошибка должна быть специфичной для ситуации структурой, которая содержит [`Backtrace`](https://doc.rust-lang.org/stable/std/backtrace/struct.Backtrace.html), возможную причину из нижележащего уровня и вспомогательные методы.</span>

Simple crates usually expose a single error type `Error`, complex crates may expose multiple types, for example
`AccessError` and `ConfigurationError`. Error types should provide helper methods for additional information that allows callers to handle the error.

<span class="ru">Простые крейты обычно экспортируют один тип ошибки `Error`, сложные могут экспортировать несколько типов, например `AccessError` и `ConfigurationError`. Типы ошибок должны предоставлять вспомогательные методы с дополнительной информацией, которая позволяет вызывающей стороне обработать ошибку.</span>

A simple error might look like so:

<span class="ru">Простая ошибка может выглядеть так:</span>

```rust
# use std::backtrace::Backtrace;
# use std::fmt::Display;
# use std::fmt::Formatter;
pub struct ConfigurationError {
    backtrace: Backtrace,
}

impl ConfigurationError {
    pub(crate) fn new() -> Self {
        Self { backtrace: Backtrace::capture() }
    }
}

// Impl Debug + Display
```

Where appropriate, error types should provide contextual error information, for example:

<span class="ru">Где это уместно, типы ошибок должны предоставлять контекстную информацию, например:</span>

```rust,ignore
# use std::backtrace::Backtrace;
# #[derive(Debug)]
# pub struct ConfigurationError {
#    backtrace: Backtrace,
# }
impl ConfigurationError {
    pub fn config_file(&self) -> &Path { }
}
```

If your API does mixed operations, or depends on various upstream libraries, store an `ErrorKind`.
Error kinds, and more generally enum-based errors, should not be used to avoid creating separate public error types when there is otherwise no error overlap:

<span class="ru">Если ваш API выполняет разнородные операции или зависит от различных нижележащих библиотек, храните `ErrorKind`. Виды ошибок и, в более общем смысле, ошибки на основе `enum` не следует использовать для того, чтобы избежать создания отдельных публичных типов ошибок, когда иначе нет пересечения ошибок:</span>

```rust, ignore
// Prefer this
fn download_iso() -> Result<(), DownloadError> {}
fn start_vm() -> Result<(), VmError> {}

// Over that
fn download_iso() -> Result<(), GlobalEverythingErrorEnum> {}
fn start_vm() -> Result<(), GlobalEverythingErrorEnum> {}

// However, not every function warrants a new error type. Errors
// should be general enough to be reused.
fn parse_json() -> Result<(), ParseError> {}
fn parse_toml() -> Result<(), ParseError> {}
```

If you do use an inner `ErrorKind`, that enum should not be exposed directly for future-proofing reasons,
as otherwise you would expose your callers to _all_ possible failure modes, even the ones you consider internal
and unhandleable. Instead, expose various `is_xxx()` methods as shown below:

<span class="ru">Если вы всё же используете внутренний `ErrorKind`, этот `enum` не следует экспортировать напрямую ради будущей совместимости, иначе вы откроете вызывающей стороне _все_ возможные режимы отказа, включая те, которые вы считаете внутренними и не поддающимися обработке. Вместо этого предоставьте различные методы `is_xxx()`, как показано ниже:</span>

```rust
# use std::backtrace::Backtrace;
# use std::fmt::Display;
# use std::fmt::Formatter;
#[derive(Debug)]
pub(crate) enum ErrorKind {
    Io(std::io::Error),
    Protocol
}

#[derive(Debug)]
pub struct HttpError {
    kind: ErrorKind,
    backtrace: Backtrace,
}

impl HttpError {
    pub fn is_io(&self) -> bool { matches!(self.kind, ErrorKind::Io(_)) }
    pub fn is_protocol(&self) -> bool { matches!(self.kind, ErrorKind::Protocol) }
}
```

Most upstream errors don't provide a backtrace. You should capture one when creating an `Error` instance, either via one of
your `Error::new()` flavors, or when implementing `From<UpstreamError> for Error {}`.

<span class="ru">Большинство ошибок нижележащего уровня не предоставляют трассировку стека. Её следует захватывать при создании экземпляра `Error`: либо через один из вариантов `Error::new()`, либо при реализации `From<UpstreamError> for Error {}`.</span>

Error structs must properly implement `Display` that renders as follows:

<span class="ru">Структуры ошибок должны корректно реализовывать `Display`, который выводит следующее:</span>

```rust,ignore
impl Display for MyError {
    // Print a summary sentence what happened.
    // Print `self.backtrace`.
    // Print any additional upstream 'cause' information you might have.
#   fn fmt(&self, f: &mut Formatter<'_>) -> std::fmt::Result {
#       todo!()
#   }
}
```

Errors must also implement `std::error::Error`:

<span class="ru">Ошибки также должны реализовывать `std::error::Error`:</span>

```rust,ignore
impl std::error::Error for MyError { }
```

Lastly, if you happen to emit lots of errors from your crate, consider creating a private `bail!()` helper macro to simplify error instantiation.

<span class="ru">Наконец, если ваш крейт выдаёт много ошибок, подумайте о создании приватного вспомогательного макроса `bail!()`, чтобы упростить создание ошибок.</span>

> ### <tip></tip> When You Get Backtraces
>
> <span class="ru">Когда появляются трассировки стека</span>
>
> Backtraces are an invaluable debug tool in complex or async code, since  errors might _travel_ far through a callstack before being surfaced.
>
> <span class="ru">Трассировки стека — незаменимый инструмент отладки в сложном или асинхронном коде, поскольку ошибки могут _проходить_ далеко по стеку вызовов, прежде чем всплыть.</span>
>
> That said, they are a _development_ tool, not a _runtime_ diagnostic, and by default `Backtrace::capture()` will **not** capture
> backtraces, as they have a large overhead, e.g., 4μs per capture on the author's PC.
>
> <span class="ru">При этом это инструмент _разработки_, а не _диагностики времени выполнения_, и по умолчанию `Backtrace::capture()` **не** захватывает трассировки, так как они дороги, например 4 мкс на захват на ПК автора.</span>
>
> Instead, Rust evaluates a [set of environment variables](https://doc.rust-lang.org/stable/std/backtrace/index.html#environment-variables), such as
> `RUST_BACKTRACE`, and only walks the call frame when explicitly asked. Otherwise it captures an empty trace, at the cost of only a few CPU instructions.
>
> <span class="ru">Вместо этого Rust учитывает [набор переменных окружения](https://doc.rust-lang.org/stable/std/backtrace/index.html#environment-variables), таких как `RUST_BACKTRACE`, и обходит кадры стека только по явному запросу. В противном случае захватывается пустая трассировка, что стоит всего нескольких инструкций процессора.</span>
