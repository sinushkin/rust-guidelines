<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Avoid statics (M-AVOID-STATICS) { #M-AVOID-STATICS }

<span class="ru">Избегайте статических элементов (`static`)</span>

<why>consistency and correctness across crate versions.</why>

<span class="ru">согласованность и корректность между версиями крейтов.</span>

Libraries should avoid `static` and thread-local items, if a consistent view of the item is relevant for correctness.
Essentially, any code that would be incorrect if the static _magically_ had another value must not use them. Statics
only used for performance optimizations are ok.

<span class="ru">Библиотеки должны избегать `static` и thread-local элементов, если для корректности важен согласованный вид элемента. По сути, любой код, который стал бы некорректным, если бы static _магическим образом_ имел другое значение, не должен их использовать. Статики, применяемые только для оптимизации производительности, допустимы.</span>

The fundamental issue with statics in Rust is the secret duplication of state.

<span class="ru">Фундаментальная проблема статических элементов в Rust — скрытое дублирование состояния.</span>

Consider a crate `core` with the following function:

<span class="ru">Рассмотрим крейт `core` со следующей функцией:</span>

```rust
# use std::sync::atomic::AtomicUsize;
# use std::sync::atomic::Ordering;
static GLOBAL_COUNTER: AtomicUsize = AtomicUsize::new(0);

pub fn increase_counter() -> usize {
    GLOBAL_COUNTER.fetch_add(1, Ordering::Relaxed)
}
```

Now assume you have a crate `main`, calling two libraries `library_a` and `library_b`, each invoking that counter:

<span class="ru">Теперь предположим, что есть крейт `main`, который вызывает две библиотеки `library_a` и `library_b`, каждая из которых обращается к этому счётчику:</span>

```rust,ignore
// Increase global static counter 2 times
library_a::count_up();
library_a::count_up();

// Increase global static counter 3 more times
library_b::count_up();
library_b::count_up();
library_b::count_up();
```

They eventually report their result:

<span class="ru">В итоге они сообщают свой результат:</span>

```rust,ignore
library_a::print_counter();
library_b::print_counter();
main::print_counter();
```

At this point, what is _the_ value of said counter; `0`, `2`, `3` or `5`?

<span class="ru">Так каково же значение этого счётчика: `0`, `2`, `3` или `5`?</span>

The answer is, possibly any  (even multiple!) of the above, depending on the crate's version resolution!

<span class="ru">Ответ: возможно любое (даже несколько) из них, в зависимости от разрешения версий крейтов!</span>

Under the hood Rust may link to multiple versions of the same crate, independently instantiated, to satisfy declared
dependencies. This is especially observable during a crate's `0.x` version timeline, where each `x` constitutes a separate _major_ version.

<span class="ru">Под капотом Rust может линковать несколько версий одного и того же крейта, независимо инстанцированных, чтобы удовлетворить объявленные зависимости. Это особенно заметно на протяжении линии версий `0.x`, где каждый `x` является отдельной _основной_ версией.</span>

If `main`,  `library_a` and `library_b` all declared the same version of `core`, e.g. `0.5`, then the reported result will be `5`, since all
crates actually _see_ the same version of `GLOBAL_COUNTER`.

<span class="ru">Если `main`, `library_a` и `library_b` все объявляют одну и ту же версию `core`, например `0.5`, то выведенный результат будет `5`, поскольку все крейты фактически _видят_ одну и ту же версию `GLOBAL_COUNTER`.</span>

However, if `library_a` declared `0.4` instead, then it would be linked against a separate version of `core`; thus `main` and `library_b` would
agree on a value of `3`, while `library_a` reported `2`.

<span class="ru">Если же `library_a` объявит `0.4`, то она будет слинкована с отдельной версией `core`; тогда `main` и `library_b` сойдутся на значении `3`, а `library_a` выведет `2`.</span>

Although `static` items can be useful, they are particularly dangerous before a library's stabilization, and for any state where _secret duplication_ would
cause consistency issues when static and non-static variable use interacts. In addition, statics interfere with unit testing, and are a contention point in
thread-per-core designs.

<span class="ru">Хотя элементы `static` могут быть полезны, они особенно опасны до стабилизации библиотеки и для любого состояния, где _скрытое дублирование_ приведёт к проблемам согласованности при взаимодействии статических и нестатических переменных. Кроме того, статики мешают модульному тестированию и становятся точкой конкуренции в архитектурах thread-per-core.</span>
