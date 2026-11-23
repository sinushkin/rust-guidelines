<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Use structured logging with message templates (M-LOG-STRUCTURED) { #M-LOG-STRUCTURED }

<span class="ru">Используйте структурированное логирование с шаблонами сообщений (M-LOG-STRUCTURED)</span>

<why>low-cost logging with strong filtering.</why>

<span class="ru">недорогое логирование с сильной фильтрацией.</span>

Logging should use structured events with named properties and message templates following
the [message templates](https://messagetemplates.org/) specification.

<span class="ru">Логирование должно использовать структурированные события с именованными свойствами и шаблонами сообщений в соответствии со спецификацией [message templates](https://messagetemplates.org/).</span>

> **Note:** Examples use the [`tracing`](https://docs.rs/tracing/) crate's `event!` macro,
but these principles apply to any logging API that supports structured logging (e.g., `log`,
`slog`, custom telemetry systems).
>
> <span class="ru">**Примечание:** примеры используют макрос `event!` крейта [`tracing`](https://docs.rs/tracing/), но эти принципы применимы к любому API логирования с поддержкой структурированных логов (например, `log`, `slog`, собственные системы телеметрии).</span>

### Avoid String Formatting

<span class="ru">Избегайте форматирования строк</span>

String formatting allocates memory at runtime. Message templates defer formatting until viewing time.
We recommend that message template includes all named properties for easier inspection at viewing time.

<span class="ru">Форматирование строк выделяет память во время выполнения. Шаблоны сообщений откладывают форматирование до момента просмотра. Рекомендуем, чтобы шаблон сообщения включал все именованные свойства для удобства просмотра.</span>

```rust,ignore
// Bad: String formatting causes allocations
tracing::info!("file opened: {}", path);
tracing::info!(format!("file opened: {}", path));

// Good: Message templates with named properties
event!(
    name: "file.open.success",
    Level::INFO,
    file.path = path.display(),
    "file opened: {{file.path}}",
);
```

> **Note**: Use the `{{property}}` syntax in message templates which preserves the literal text
> while escaping Rust's format syntax. String formatting is deferred until logs are viewed.
>
> <span class="ru">**Примечание**: используйте синтаксис `{{property}}` в шаблонах сообщений: он сохраняет буквальный текст и экранирует синтаксис форматирования Rust. Форматирование строк откладывается до просмотра логов.</span>

### Name Your Events

<span class="ru">Называйте ваши события</span>

Use hierarchical dot-notation: `<component>.<operation>.<state>`

<span class="ru">Используйте иерархическую точечную нотацию: `<component>.<operation>.<state>`</span>

```rust,ignore
// Bad: Unnamed events
event!(
    Level::INFO,
    file.path = file_path,
    "file {{file.path}} processed successfully",
);

// Good: Named events
event!(
    name: "file.processing.success", // event identifier
    Level::INFO,
    file.path = file_path,
    "file {{file.path}} processed successfully",
);
```

Named events enable grouping and filtering across log entries.

<span class="ru">Именованные события позволяют группировать и фильтровать записи журнала.</span>

### Follow OpenTelemetry Semantic Conventions

<span class="ru">Следуйте семантическим соглашениям OpenTelemetry</span>

Use [OTel semantic conventions](https://opentelemetry.io/docs/specs/semconv/) for common attributes if needed.
This enables standardization and interoperability.

<span class="ru">Используйте [семантические соглашения OTel](https://opentelemetry.io/docs/specs/semconv/) для общих атрибутов при необходимости. Это обеспечивает стандартизацию и совместимость.</span>

```rust,ignore
event!(
    name: "file.write.success",
    Level::INFO,
    file.path = path.display(),         // Standard OTel name
    file.size = bytes_written,          // Standard OTel name
    file.directory = dir_path,          // Standard OTel name
    file.extension = extension,         // Standard OTel name
    file.operation = "write",           // Custom name
    "{{file.operation}} {{file.size}} bytes to {{file.path}} in {{file.directory}} extension={{file.extension}}",
);
```

Common conventions:

<span class="ru">Распространённые соглашения:</span>

- HTTP: `http.request.method`, `http.response.status_code`, `url.scheme`, `url.path`, `server.address`
  - <span class="ru">Для HTTP: `http.request.method`, `http.response.status_code`, `url.scheme`, `url.path`, `server.address`</span>
- File: `file.path`, `file.directory`, `file.name`, `file.extension`, `file.size`
  - <span class="ru">Для файлов: `file.path`, `file.directory`, `file.name`, `file.extension`, `file.size`</span>
- Database: `db.system.name`, `db.namespace`, `db.operation.name`, `db.query.text`
  - <span class="ru">Для баз данных: `db.system.name`, `db.namespace`, `db.operation.name`, `db.query.text`</span>
- Errors: `error.type`, `error.message`, `exception.type`, `exception.stacktrace`
  - <span class="ru">Для ошибок: `error.type`, `error.message`, `exception.type`, `exception.stacktrace`</span>

### Redact Sensitive Data

<span class="ru">Скрывайте чувствительные данные</span>

Do not log plain sensitive data as this might lead to privacy and security incidents.

<span class="ru">Не записывайте чувствительные данные в открытом виде в журнал: это может привести к инцидентам с приватностью и безопасностью.</span>

```rust,ignore
// Bad: Logs potentially sensitive data
event!(
    name: "file.operation.started",
    Level::INFO,
    user.email = user.email,  // Sensitive data
    file.name = "license.txt",
    "reading file {{file.name}} for user {{user.email}}",
);

// Good: Redact sensitive parts
event!(
    name: "file.operation.started",
    Level::INFO,
    user.email.redacted = redact_email(user.email),
    file.name = "license.txt",
    "reading file {{file.name}} for user {{user.email.redacted}}",
);
```

Sensitive data includes email addresses, file paths revealing user identity, filenames containing secrets or tokens,
file contents with PII, temporary file paths with session IDs and more. Consider using the [`data_privacy`](https://crates.io/crates/data_privacy) crate for consistent redaction.

<span class="ru">К чувствительным данным относятся адреса электронной почты, пути к файлам, раскрывающие личность пользователя, имена файлов с секретами или токенами, содержимое файлов с персональными данными (PII), временные пути с идентификаторами сессий и другое. Для единообразного обезличивания рассмотрите крейт [`data_privacy`](https://crates.io/crates/data_privacy).</span>

### Further Reading

<span class="ru">Дополнительная литература</span>

- [Message Templates Specification](https://messagetemplates.org/)
- [OpenTelemetry Semantic Conventions](https://opentelemetry.io/docs/specs/semconv/)
- [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
