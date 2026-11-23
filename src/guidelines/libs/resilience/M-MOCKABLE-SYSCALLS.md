<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## I/O and system calls are mockable (M-MOCKABLE-SYSCALLS) { #M-MOCKABLE-SYSCALLS }

<span class="ru">Ввод-вывод и системные вызовы поддаются мокированию</span>

<why>testable edge cases that are otherwise hard to evoke.</why>

<span class="ru">тестируемые граничные случаи, которые иначе трудно воспроизвести.</span>

Any user-facing type doing I/O, or sys calls with side effects, should be mockable to these effects. This includes file and
network access, clocks, entropy sources and seeds, and similar. More generally, any operation that is

<span class="ru">Любой пользовательский тип, выполняющий ввод-вывод или системные вызовы с побочными эффектами, должен поддаваться мокированию этих эффектов. Сюда относятся доступ к файлам и сети, часы, источники энтропии и seed-значения и т.п. В более общем виде любая операция, которая</span>

- non-deterministic,
  - <span class="ru">недетерминированная,</span>
- reliant on external state,
  - <span class="ru">зависящая от внешнего состояния,</span>
- depending on the hardware or the environment,
  - <span class="ru">зависящая от оборудования или окружения,</span>
- is otherwise fragile or not universally reproducible
  - <span class="ru">в остальном хрупкая или не воспроизводимая универсально</span>

should be mockable.

<span class="ru">должна поддаваться мокированию.</span>

> ### <tip></tip> Mocking Allocations?
>
> <span class="ru">Мокировать выделения памяти?</span>
>
> Unless you write kernel code or similar, you can consider allocations to be deterministic, hardware independent and practically
> infallible, thus not covered by this guideline.
>
> <span class="ru">Если вы не пишете ядерный код или что-то подобное, можно считать, что выделения памяти детерминированы, не зависят от оборудования и практически не могут завершиться ошибкой, поэтому на них это руководство не распространяется.</span>
>
> However, this does _not_ mean you should expect there to be unlimited memory available. While it is ok to
> accept caller provided input as-is if your library has a _reasonable_ memory complexity, memory-hungry libraries
> and code handling external input should provide bounded and / or chunking operations.
>
> <span class="ru">Однако это _не_ означает, что можно рассчитывать на неограниченный объём памяти. Хотя допустимо принимать входные данные, переданные вызывающим кодом, как есть, если библиотека имеет _разумную_ сложность по памяти, библиотеки с высоким потреблением памяти и код, обрабатывающий внешние входные данные, должны предоставлять ограниченные и/или чанкованные операции.</span>

This guideline has several implications for libraries, they

<span class="ru">Это руководство имеет несколько следствий для библиотек, а именно они</span>

- should not perform ad-hoc I/O, i.e., call `read("foo.txt")`
  - <span class="ru">не должны выполнять ad-hoc ввод-вывод, то есть вызывать `read("foo.txt")`</span>
- should not rely on non-mockable I/O and sys calls
  - <span class="ru">не должны полагаться на немокируемый ввод-вывод и системные вызовы</span>
- should not create their own I/O or sys call _core_ themselves
  - <span class="ru">не должны самостоятельно создавать собственное _ядро_ ввода-вывода или системных вызовов</span>
- should not offer `MyIoLibrary::default()` constructors
  - <span class="ru">не должны предоставлять конструкторы `MyIoLibrary::default()`</span>

Instead, libraries performing I/O and sys calls should either accept some I/O _core_ that is mockable already, or provide mocking functionality themselves:

<span class="ru">Вместо этого библиотеки, выполняющие ввод-вывод и системные вызовы, должны либо принимать уже мокируемое ядро ввода-вывода, либо сами предоставлять функциональность мокирования:</span>

```rust, ignore
let lib = Library::new_runtime(runtime_io); // mockable I/O functionality passed in
let (lib, mock) = Library::new_mocked(); // supports inherent mocking
```

Libraries supporting inherent mocking should implement it as follows:

```rust, ignore
pub struct Library {
    some_core: LibraryCore // Encapsulates syscalls, I/O, ... compare below.
}

impl Library {
    pub fn new() -> Self { ... }
    pub fn new_mocked() -> (Self, MockCtrl) { ... }
}
```

Behind the scenes, `LibraryCore` is a non-public enum, similar to [M-RUNTIME-ABSTRACTED], that either dispatches
calls to the respective sys call, or to an mocking controller.

<span class="ru">За кулисами `LibraryCore` — это непубличный enum, похожий на [M-RUNTIME-ABSTRACTED], который либо передаёт вызовы соответствующему системному вызову, либо контроллеру мокирования.</span>

```rust, ignore
// Dispatches calls either to the operating system, or to a
// mocking controller.
enum LibraryCore {
    Native,

    #[cfg(feature = "test-util")]
    Mocked(mock::MockCtrl)
}

impl LibraryCore {
    // Some function you'd forward to the operating system.
    fn random_u32(&self) {
        match self {
            Self::Native => unsafe { os_random_u32() }
            Self::Mocked(m) => m.random_u32()
        }
    }
}


#[cfg(feature = "test-util")]
mod mock {
    // This follows the M-SERVICES-CLONE pattern, so both `LibraryCore` and
    // the user can hold on to the same `MockCtrl` instance.
    pub struct MockCtrl {
        inner: Arc<MockCtrlInner>
    }

    // Implement required logic accordingly, usually forwarding to
    // `MockCtrlInner` below.
    impl MockCtrl {
        pub fn set_next_u32(&self, x: u32) { ... }
        pub fn random_u32(&self) { ... }
    }

    // Contains actual logic, e.g., the next random number we should return.
    struct MockCtrlInner {
        next_call: u32
    }
}
```

Runtime-aware libraries already build on top of the [M-RUNTIME-ABSTRACTED] pattern should extend their runtime enum instead:

<span class="ru">Библиотеки, учитывающие рантайм и уже построенные на шаблоне [M-RUNTIME-ABSTRACTED], должны вместо этого расширять свой enum рантайма:</span>

```rust, ignore
enum Runtime {
    #[cfg(feature="tokio")]
    Tokio(tokio::Tokio),

    #[cfg(feature="smol")]
    Smol(smol::Smol)

    #[cfg(feature="test-util")]
    Mock(mock::MockCtrl)
}
```

As indicated above, most libraries supporting mocking should not accept mock controllers, but return them via parameter tuples,
with the first parameter being the library instance, the second the mock controller. This is to prevent state ambiguity if multiple
instances shared a single controller:

<span class="ru">Как указано выше, большинство библиотек с поддержкой мокирования не должны принимать контроллеры мока, а возвращать их через кортежи параметров: первым параметром — экземпляр библиотеки, вторым — контроллер мока. Это нужно, чтобы избежать неоднозначности состояния, если несколько экземпляров разделяли бы один контроллер:</span>

```rust, ignore
impl Library {
    pub fn new_mocked() -> (Self, MockCtrl) { ... } // good
    pub fn new_mocked_bad(&mut MockCtrl) -> Self { ... } // prone to misuse
}
```

[M-RUNTIME-ABSTRACTED]: ../ux/#M-RUNTIME-ABSTRACTED
