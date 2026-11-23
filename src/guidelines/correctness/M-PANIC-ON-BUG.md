<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Detected programming bugs are panics, not errors (M-PANIC-ON-BUG) { #M-PANIC-ON-BUG }

<span class="ru">Обнаруженные ошибки программирования — это паники, а не ошибки (M-PANIC-ON-BUG)</span>

<why>tractable error handling and runtime consistency.</why>

<span class="ru">понятная обработка ошибок и согласованность во время выполнения.</span>

As an extension of [M-PANIC-IS-STOP] above, when an unrecoverable programming error has been
detected, libraries and applications must panic, i.e., request program termination.

<span class="ru">Как развитие [M-PANIC-IS-STOP] выше, когда обнаружена неустранимая ошибка программирования, библиотеки и приложения должны паниковать, то есть запрашивать завершение программы.</span>

In these cases, no `Error` type should be introduced or returned, as any such error could not be acted upon at runtime.

<span class="ru">В таких случаях не следует вводить или возвращать тип `Error`, поскольку такую ошибку во время выполнения нельзя было бы обработать.</span>

Contract violations, i.e., the breaking of invariants either within a library or by a caller, are programming errors and must therefore panic.

<span class="ru">Нарушения контрактов, то есть нарушения инвариантов либо внутри библиотеки, либо вызывающей стороной, являются ошибками программирования и поэтому должны приводить к панике.</span>

However, what constitutes a violation is situational. APIs are not expected to go out of their way to detect them, as such
checks can be impossible or expensive. Encountering `must_be_even == 3` during an already existing check clearly warrants
a panic, while a function `parse(&str)` clearly must return a `Result`. If in doubt, we recommend you take inspiration from the standard library.

<span class="ru">Однако то, что считается нарушением, зависит от ситуации. От API не ожидается, что они будут специально искать такие нарушения, так как проверки могут быть невозможны или дороги. Встретив `must_be_even == 3` во время уже существующей проверки, очевидно, следует паниковать, тогда как функция `parse(&str)` очевидно должна возвращать `Result`. В случае сомнений рекомендуем ориентироваться на стандартную библиотеку.</span>

```rust, ignore
// Generally, a function with bad parameters must either
// - Ignore a parameter and/or return the wrong result
// - Signal an issue via Result or similar
// - Panic
// If in this `divide_by` we see that y == 0, panicking is
// the correct approach.
fn divide_by(x: u32, y: u32) -> u32 { ... }

// However, it can also be permissible to omit such checks
// and return an unspecified (but not an undefined) result.
fn divide_by_fast(x: u32, y: u32) -> u32 { ... }

// Here, passing an invalid URI is not a contract violation.
// Since parsing is inherently fallible, a Result must be returned.
fn parse_uri(s: &str) -> Result<Uri, ParseError> { };

```

> ### <tip></tip> Make it 'Correct by Construction'
>
> <span class="ru">Сделайте код «корректным по построению»</span>
>
> While panicking on a detected programming error is the 'least bad option', your panic might still ruin someone's day.
> For any user input or calling sequence that would otherwise panic, you should also explore if you can use the type
> system to avoid panicking code paths altogether.
>
> <span class="ru">Хотя паника при обнаруженной ошибке программирования — «наименее плохой вариант», ваша паника всё равно может испортить кому-то день. Для любого пользовательского ввода или последовательности вызовов, которые иначе привели бы к панике, также стоит изучить, можно ли с помощью системы типов полностью избежать путей кода, ведущих к панике.</span>

[M-PANIC-IS-STOP]: ./#M-PANIC-IS-STOP
