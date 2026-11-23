<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Unsafe implies undefined behavior (M-UNSAFE-IMPLIES-UB) { #M-UNSAFE-IMPLIES-UB }

<span class="ru">Unsafe подразумевает неопределённое поведение (M-UNSAFE-IMPLIES-UB)</span>

<why>semantic consistency without warning fatigue.</why>

<span class="ru">семантическая согласованность без усталости от предупреждений.</span>

The marker `unsafe` may only be applied to functions and traits if misuse implies the risk of undefined behavior (UB).
It must not be used to mark functions that are dangerous to call for other reasons.

<span class="ru">Маркер `unsafe` допустимо применять к функциям и трейтам только в том случае, если неправильное использование влечёт риск неопределённого поведения (UB). Его не следует использовать для пометки функций, опасных для вызова по другим причинам.</span>

```rust
// Valid use of unsafe
unsafe fn print_string(x: *const String) { }

// Invalid use of unsafe
unsafe fn delete_database() { }
```
