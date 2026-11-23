<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Custom panics have a helpful message (M-PANIC-MESSAGE) { #M-PANIC-MESSAGE }

<span class="ru">Пользовательские паники содержат понятное сообщение (M-PANIC-MESSAGE)</span>

<why>faster bug diagnosis.</why>

<span class="ru">более быстрая диагностика ошибок.</span>

When code panics intentionally (via `panic!`, `assert!`, `unreachable!`, `todo!`, or similar), a message must be present to clearly state what went wrong and, where applicable, include relevant values.

<span class="ru">Когда код намеренно паникует (через `panic!`, `assert!`, `unreachable!`, `todo!` или аналогичные средства), сообщение обязательно должно присутствовать и ясно указывать, что пошло не так, а также, где это применимо, включать релевантные значения.</span>

```rust,ignore
// Bad, the panic gives the developer little to act on.
assert!(buffer.len() >= HEADER_SIZE);

// Good, message contains reason and actual values.
assert!(buffer.len() >= HEADER_SIZE, "buffer too small for header: got {} bytes, need {HEADER_SIZE}", buffer.len());
```

Messages related to API misuse should be useful to the end user. Messages indicating bugs should be helpful to you-as-the-author, or whoever maintains the project after you, to quickly identify the underlying cause.

<span class="ru">Сообщения, связанные с неправильным использованием API, должны быть полезны конечному пользователю. Сообщения о багах должны помогать вам как автору или тому, кто будет поддерживать проект после вас, быстро определить первопричину.</span>

Panic messages in tests are not generally needed.

<span class="ru">Сообщения о паниках в тестах, как правило, не нужны.</span>
