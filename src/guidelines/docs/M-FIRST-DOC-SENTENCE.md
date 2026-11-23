<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## First sentence is one line; approx. 15 words (M-FIRST-DOC-SENTENCE) { #M-FIRST-DOC-SENTENCE }

<span class="ru">Первое предложение — в одну строку; примерно 15 слов (M-FIRST-DOC-SENTENCE)</span>

<why>easily skimmable API docs.</why>

<span class="ru">легко просматриваемая документация API.</span>

When you document your item, the first sentence becomes the "summary sentence" that is extracted and shown in the module summary:

<span class="ru">Когда вы документируете элемент, первое предложение становится «итоговым предложением» (summary sentence), которое извлекается и показывается в сводке модуля:</span>

```rust
/// This is the summary sentence, shown in the module summary.
///
/// This is other documentation. It is only shown in that item's detail view.
/// Sentences here can be as long as you like and it won't cause any issues.
fn some_item() { }
```

Since Rust API documentation is rendered with a fixed max width, there is a naturally preferred sentence length you should not
exceed to keep things tidy on most screens.

<span class="ru">Поскольку документация API Rust отображается с фиксированной максимальной шириной, существует естественно предпочтительная длина предложения, которую не следует превышать, чтобы на большинстве экранов всё выглядело аккуратно.</span>

If you keep things in a line, your docs will become easily skimmable. Compare, for example, the standard library:

<span class="ru">Если уложиться в одну строку, документацию будет легко просматривать. Для сравнения возьмите, например, стандартную библиотеку:</span>

![TEXT](M-FIRST-DOC-SENTENCE_GOOD.png)

Otherwise, you might end up with _widows_ and a generally unpleasant reading flow:

<span class="ru">Иначе могут появиться _висячие строки_ (_widows_) и в целом неудобный поток чтения:</span>

![TEXT](M-FIRST-DOC-SENTENCE_BAD.png)

As a rule of thumb, the first sentence should not exceed **15 words**.

<span class="ru">Как правило, первое предложение не должно превышать **15 слов**.</span>
