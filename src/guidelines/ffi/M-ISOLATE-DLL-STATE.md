<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Isolate DLL state between FFI libraries (M-ISOLATE-DLL-STATE) { #M-ISOLATE-DLL-STATE }

<span class="ru">Изолируйте состояние DLL между FFI-библиотеками (M-ISOLATE-DLL-STATE)</span>

<why>data integrity and defined behavior across DLL boundaries.</why>

<span class="ru">целостность данных и определённое поведение на границах DLL.</span>

When loading multiple Rust-based dynamic libraries (DLLs) within one application, you may only share 'portable' state between these libraries.
Likewise, when authoring such libraries, you must only accept or provide 'portable' data from foreign DLLs.

<span class="ru">При загрузке нескольких динамических библиотек (DLL) на основе Rust в одном приложении между этими библиотеками можно разделять только «переносимое» состояние. Аналогично, при создании таких библиотек вы должны принимать или передавать только «переносимые» данные из чужих DLL.</span>

Portable here means data that is safe and consistent to process regardless of its origin. By definition, this is a subset of FFI-safe types.
A type is portable if it is `#[repr(C)]` (or similarly well-defined), and _all_ of the following:

<span class="ru">Переносимость здесь означает данные, которые безопасно и согласованно обрабатывать независимо от их происхождения. По определению это подмножество FFI-безопасных типов. Тип переносим, если он помечен `#[repr(C)]` (или аналогично хорошо определён) и _все_ следующие условия выполняются:</span>

- It must not have any interaction with any `static` or thread local.
  - <span class="ru">Он не должен взаимодействовать ни с каким `static` или thread-local значением.</span>
- It must not have any interaction with any `TypeId`.
  - <span class="ru">Он не должен взаимодействовать ни с каким `TypeId`.</span>
- It must not contain any value, pointer or reference to any non-portable data (it is valid to point into portable data within non-portable data, such as
  sharing a reference to an ASCII string held in a `Box`).
  - <span class="ru">Он не должен содержать никаких значений, указателей или ссылок на непереносимые данные (допустимо указывать на переносимые данные внутри непереносимых, например разделять ссылку на ASCII-строку, хранящуюся в `Box`).</span>

_Interaction_ means any computational relationship, and therefore also relates to how the type is used. Sending a `u128` between DLLs is OK, using it to
exchange a transmuted `TypeId` isn't.

<span class="ru">_Взаимодействие_ означает любую вычислительную связь и, следовательно, также то, как тип используется. Отправлять `u128` между DLL допустимо, а использовать его для обмена преобразованным (transmuted) `TypeId` нельзя.</span>

The underlying issue stems from the Rust compiler treating each DLL as an entirely new compilation artifact, akin to a standalone application. This means each DLL:

<span class="ru">Причина в том, что компилятор Rust рассматривает каждую DLL как совершенно новый артефакт сборки, подобно самостоятельному приложению. Это означает, что каждая DLL:</span>

- has its own set of `static` and thread-local variables,
  - <span class="ru">имеет собственный набор переменных `static` и thread-local,</span>
- the type layout of any `#[repr(Rust)]` type (the default) can differ between compilations,
  - <span class="ru">раскладка в памяти любого типа `#[repr(Rust)]` (по умолчанию) может различаться между сборками,</span>
- has its own set of unique type IDs, differing from any other DLL.
  - <span class="ru">имеет собственный набор уникальных идентификаторов типов, отличных от идентификаторов любой другой DLL.</span>

Notably, this affects:

<span class="ru">В частности, это затрагивает:</span>

- ⚠️ any allocated instance, e.g., `String`, `Vec<u8>`, `Box<Foo>`, ...
  - <span class="ru">⚠️ любой выделенный экземпляр, например `String`, `Vec<u8>`, `Box<Foo>`, ...</span>
- ⚠️ any library relying on other statics, e.g., `tokio`, `log`,
  - <span class="ru">⚠️ любая библиотека, опирающаяся на другие статики, например `tokio`, `log`,</span>
- ⚠️ any struct not `#[repr(C)]`,
  - <span class="ru">⚠️ любая структура, не помеченная `#[repr(C)]`,</span>
- ⚠️ any data structure relying on consistent `TypeId`.
  - <span class="ru">⚠️ любая структура данных, опирающаяся на согласованный `TypeId`.</span>

In practice, transferring any of the above between libraries leads to data loss, state corruption, and usually undefined behavior.

<span class="ru">На практике передача любого из перечисленного между библиотеками приводит к потере данных, повреждению состояния и, как правило, к неопределённому поведению.</span>

Take particular note that this may also apply to types and methods that are invisible at the FFI boundary:

<span class="ru">Обратите особое внимание: это может относиться также к типам и методам, невидимым на границе FFI:</span>

```rust,ignore
/// A method in DLL1 that wants to use a common service from DLL2
#[ffi_function]
fn use_common_service(common: &CommonService) {
    // This has at least two issues:
    // - `CommonService`, or ANY type nested deep within might have
    //   a different type layout in DLL2, leading to immediate
    //   undefined behavior (UB) ⚠️
    // - `do_work()` here looks like it will be invoked in DLL2, but
    //   the code executed will actually come from DLL1. This means that
    //   `do_work()` invoked here will see a data structure coming from
    //   DLL2, but will use statics from DLL1 ⚠️
    common.do_work();
}
```
