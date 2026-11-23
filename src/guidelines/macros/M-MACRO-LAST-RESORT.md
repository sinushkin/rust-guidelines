<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Macros are a last resort (M-MACRO-LAST-RESORT) { #M-MACRO-LAST-RESORT }

<span class="ru">Макросы — крайняя мера (M-MACRO-LAST-RESORT)</span>

<why>minimal complexity.</why>

<span class="ru">минимальная сложность.</span>

Macros should only be used if no other viable solution exists, compare this adage:

<span class="ru">Макросы следует использовать, только если нет другого жизнеспособного решения. Сравните с этим присловьем:</span>

> As @littlecalculist always told me, “macros are for when you run out of language”. If you still have language left — and Rust gives you a lot of language — use the language first.
>
> <span class="ru">Как всегда говорил мне @littlecalculist, «макросы нужны, когда у вас заканчивается язык». Если язык у вас ещё остался — а в Rust его очень много — сначала используйте язык.</span>
>
> @pcwalton

Macros are powerful, but come with several downsides. They

<span class="ru">Макросы мощны, но у них есть несколько недостатков. Они</span>

- are magic, and it can be impossible to predict what they do, or how they do it,
  - <span class="ru">работают как магия, и предсказать, что они делают и как именно, может быть невозможно,</span>
- disproportionally increase compilation time in projects that otherwise don't rely on them,
  - <span class="ru">несоразмерно увеличивают время компиляции в проектах, которые без них бы обошлись,</span>
- can cause subtle breakage at edition boundaries where Rust syntax and semantics can change.
  - <span class="ru">могут вызывать тонкие поломки на границах редакций, где синтаксис и семантика Rust могут меняться.</span>

Counterintuitively, the more structurally complex the result of a macro expansion is, the worse an idea it is to use macros for that in the first place. The ideal macro makes your users go "_I know exactly what this will generate, but I don't want to write all of that by hand_".

<span class="ru">Парадоксально, но чем структурно сложнее результат раскрытия макроса, тем хуже сама идея использовать для этого макрос. Идеальный макрос заставляет пользователей подумать: «_Я точно знаю, что он сгенерирует, но не хочу писать всё это вручную_».</span>
