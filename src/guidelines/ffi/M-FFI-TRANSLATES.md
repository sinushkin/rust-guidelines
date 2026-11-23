<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Business logic belongs in core crates, FFI only translates (M-FFI-TRANSLATES) { #M-FFI-TRANSLATES }

<span class="ru">Бизнес-логика принадлежит базовым крейтам, FFI только переводит (M-FFI-TRANSLATES)</span>

<why>maximal safe code and a clean separation of concerns.</why>

<span class="ru">максимально безопасный код и чистое разделение ответственности.</span>

When Rust is used to create FFI libraries, there should be a clear separation of concerns between the core _business logic_ crate `foo` and the glue crate `foo-ffi`.

<span class="ru">При создании FFI-библиотек на Rust должно быть чёткое разделение ответственности между базовым крейтом с _бизнес-логикой_ `foo` и связующим крейтом `foo-ffi`.</span>

Any operational functionality belongs in the core crate and should be expressed as idiomatic, safe, testable Rust. The FFI crate exists only to translate between native Rust and C constructs, and the core crate must not be infected with interop concerns, even if this means repeating, and slightly adjusting, type and function signatures. For example, given the following type in the core crate `foo`:

<span class="ru">Любая операционная функциональность принадлежит базовому крейту и должна быть выражена как идиоматичный, безопасный и тестируемый Rust. Крейт FFI существует только для перевода между нативными конструкциями Rust и C, и базовый крейт не должен загрязняться задачами взаимодействия, даже если ради этого придётся повторять и немного адаптировать сигнатуры типов и функций. Например, для следующего типа в базовом крейте `foo`:</span>

```rust,ignore
pub struct Message {
    destination: [u8; 8],
    data: Vec<u8>,
}

impl Message {
    pub fn new(destination: [u8; 8], data: Vec<u8>) -> Self { /* ... */ }
    pub fn transmit(&self) -> Result<(), TransmitError> { /* ... */ }
}
```

A proper separation of concerns might collapse construction and transmission into a single FFI entry point in `foo-ffi`:

<span class="ru">Правильное разделение ответственности может свести конструирование и передачу к одной точке входа FFI в `foo-ffi`:</span>

```rust,ignore
#[no_mangle]
pub unsafe extern "C" fn transmit_message(
    destination: *const [u8; 8],
    data: *const u8,
    data_len: usize,
) -> u8 {
    let data = std::slice::from_raw_parts(data, data_len).to_vec();
    match Message::new(*destination, data).transmit() {
        Ok(()) => 0,
        Err(_) => 1,
    }
}
```

However, it would be improper to leak FFI requirements into `foo` itself: ownership, data models and signatures do not translate seamlessly between the two worlds. Any time _saved_ by skipping a clean split will have to be paid back many times over during refactorings down the line.

<span class="ru">Однако было бы неправильно переносить требования FFI в сам `foo`: владение, модели данных и сигнатуры не переходят между двумя мирами без потерь. Любое время, _сэкономленное_ пропуском чистого разделения, придётся многократно возвращать при последующих рефакторингах.</span>

```rust
#[repr(C)]
pub struct Message {
    pub destination: [u8; 8],
    pub data_ptr: *mut u8,
    pub data_len: usize,
    pub data_cap: usize,
}
```
