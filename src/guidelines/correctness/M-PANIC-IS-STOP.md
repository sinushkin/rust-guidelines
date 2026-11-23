<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Panic means 'stop the program' (M-PANIC-IS-STOP) { #M-PANIC-IS-STOP }

<span class="ru">Паника означает «остановить программу» (M-PANIC-IS-STOP)</span>

<why>soundness and predictability.</why>

<span class="ru">корректность и предсказуемость.</span>

Panics are not exceptions. Instead, they suggest immediate program termination.

<span class="ru">Паники — не исключения. Вместо этого они означают немедленное завершение программы.</span>

Although your code must be [_minimally_ panic-safe](https://doc.rust-lang.org/nomicon/exception-safety.html) (i.e., a survived panic may not lead to
undefined state), invoking a panic means _this program should stop now_. It is not valid to:

<span class="ru">Хотя ваш код должен быть [_минимально_ устойчивым к панике](https://doc.rust-lang.org/nomicon/exception-safety.html) (то есть переживший панику код не должен приводить к неопределённому состоянию), вызов паники означает, что _эта программа должна остановиться сейчас_. Недопустимо:</span>

- use panics to communicate (errors) upstream,
  - <span class="ru">использовать паники для передачи (ошибок) вверх по стеку,</span>
- use panics to handle self-inflicted error conditions,
  - <span class="ru">использовать паники для обработки ошибочных условий, вызванных самим кодом,</span>
- assume panics will be caught, even by your own code.
  - <span class="ru">полагаться на то, что паника будет перехвачена, даже вашим собственным кодом.</span>

For example, if the application calling you is compiled with a `Cargo.toml` containing

<span class="ru">Например, если приложение, вызывающее ваш код, собрано с `Cargo.toml`, содержащим</span>

```toml
[profile.release]
panic = "abort"
```

then any invocation of panic will cause an otherwise functioning program to needlessly abort. Valid reasons to panic are:

<span class="ru">тогда любой вызов panic приведёт к ненужному аварийному завершению работающей программы. Допустимые причины для паники:</span>

- when encountering a programming error, e.g., `x.expect("must never happen")`,
  - <span class="ru">при обнаружении ошибки в программе, например, `x.expect("must never happen")`,</span>
- anything invoked from const contexts, e.g., `const { foo.unwrap() }`,
  - <span class="ru">всё, что вызывается из const-контекстов, например, `const { foo.unwrap() }`,</span>
- when user requested, e.g., providing an `unwrap()` method yourself,
  - <span class="ru">по запросу пользователя, например, при предоставлении собственного метода `unwrap()`,</span>
- when encountering a poison, e.g., by calling `unwrap()` on a lock result (a poisoned lock signals another thread has panicked already).
  - <span class="ru">при обнаружении «отравления» (poison), например, при вызове `unwrap()` на результате блокировки (отравленная блокировка сигнализирует, что другой поток уже запаниковал).</span>

Any of those are directly or indirectly linked to programming errors.

<span class="ru">Все они прямо или косвенно связаны с ошибками в программе.</span>
