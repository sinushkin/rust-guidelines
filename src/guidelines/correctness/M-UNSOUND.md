<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## All code must be sound (M-UNSOUND) { #M-UNSOUND }

<span class="ru">Весь код должен быть корректным (sound) (M-UNSOUND)</span>

<why>predictable runtime behavior free of bugs and incompatibilities.</why>

<span class="ru">предсказуемое поведение во время выполнения без ошибок и несовместимостей.</span>

Unsound code is seemingly _safe_ code that may produce undefined behavior when called from other safe code, or on its own accord.

<span class="ru">Unsound-код — это якобы _безопасный_ код, который может приводить к неопределённому поведению при вызове из другого безопасного кода или сам по себе.</span>

> ### <tip></tip> Meaning of 'Safe'
>
> <span class="ru">Смысл слова «safe»</span>
>
> The terms _safe_ and `unsafe` are technical terms in Rust.
>
> <span class="ru">Термины _safe_ и `unsafe` — технические термины Rust.</span>
>
> A function is _safe_, if its signature does not mark it `unsafe`. That said, _safe_ functions can still be dangerous
> (e.g., `delete_database()`), and `unsafe` ones are, when properly used, usually quite benign (e.g.,`vec.get_unchecked()`).
>
> <span class="ru">Функция _safe_, если её сигнатура не помечена `unsafe`. Однако _безопасные_ функции всё равно могут быть опасными (например, `delete_database()`), а `unsafe`-функции при правильном использовании обычно вполне безобидны (например, `vec.get_unchecked()`).</span>
>
> A function is therefore _unsound_ if it appears _safe_ (i.e., it is not marked `unsafe`), but if _any_ of its calling
> modes would cause undefined behavior. This is to be interpreted in the strictest sense. Even if causing undefined
> behavior is only a 'remote, theoretical possibility' requiring 'weird code', the function is unsound.
>
> <span class="ru">Поэтому функция _unsound_, если она выглядит _safe_ (то есть не помечена `unsafe`), но любой из режимов её вызова приводит к неопределённому поведению. Это следует понимать в самом строгом смысле. Даже если неопределённое поведение возможно лишь как «удалённая, теоретическая возможность», требующая «странного кода», функция всё равно unsound.</span>
>
> Also see [Unsafe, Unsound, Undefined](https://cheats.rs/#unsafe-unsound-undefined).
>
> <span class="ru">См. также [Unsafe, Unsound, Undefined](https://cheats.rs/#unsafe-unsound-undefined).</span>

```rust
// "Safely" converts types
fn unsound_ref<T>(x: &T) -> &u128 {
    unsafe { std::mem::transmute(x) }
}

// "Clever trick" to work around missing `Send` bounds.
struct AlwaysSend<T>(T);
unsafe impl<T> Send for AlwaysSend<T> {}
unsafe impl<T> Sync for AlwaysSend<T> {}
```

Unsound abstractions are never permissible. If you cannot safely encapsulate something, you must expose `unsafe` functions instead, and document proper behavior.

<span class="ru">Unsound-абстракции никогда не допустимы. Если вы не можете безопасно инкапсулировать что-либо, вместо этого вы должны открывать `unsafe`-функции и документировать корректное поведение.</span>

<div class="warning">

No Exceptions

<span class="ru">Исключений нет</span>

While you may break most guidelines if you have a good enough reason, there are no exceptions in this case: unsound code is never acceptable.

<span class="ru">Хотя большинство руководств можно нарушить при достаточно веской причине, в этом случае исключений нет: unsound-код недопустим никогда.</span>

</div>

> ### <tip></tip> It's the Module Boundaries
>
> <span class="ru">Границы корректности совпадают с границами модулей</span>
>
> Note that soundness boundaries equal module boundaries! It is perfectly fine, in an otherwise safe abstraction,
> to have safe functions that rely on behavior guaranteed elsewhere **in the same module**.
>
> <span class="ru">Обратите внимание: границы корректности совпадают с границами модулей! Вполне допустимо, в иначе безопасной абстракции, иметь безопасные функции, которые опираются на поведение, гарантированное в другом месте **в том же модуле**.</span>
>
> ```rust
> struct MyDevice(*const u8);
>
> impl MyDevice {
>     fn new() -> Self {
>        // Properly initializes instance ...
>        # todo!()
>     }
>
>     fn get(&self) -> u8 {
>         // It is perfectly fine to rely on `self.0` being valid, despite this
>         // function in-and-by itself being unable to validate that.
>         unsafe { *self.0 }
>     }
> }
>
> ```
