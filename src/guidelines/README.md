<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

# About

A collection of pragmatic design guidelines helping application and library developers to produce idiomatic Rust that scales.

<span class="ru">Набор прагматичных руководств по проектированию, помогающих разработчикам приложений и библиотек писать идиоматичный Rust, который масштабируется.</span>

Version: 2026.6

<span class="ru">Версия: 2026.6</span>

## Meta Design Principles

We build on existing high-quality guidelines, most notably the [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/checklist.html),
addressing topics often encountered by Rust developers. For a guideline to make it into this book, we expect it to meet the following criteria:

<span class="ru">Мы опираемся на существующие качественные руководства, в первую очередь на [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/checklist.html), и затрагиваем темы, с которыми Rust-разработчики сталкиваются часто. Чтобы руководство попало в эту книгу, оно должно соответствовать следующим критериям:</span>

- [ ] It positively affects { safety, COGs, maintenance }; i.e., it must where applicable
  - <span class="ru">Оно положительно влияет на { безопасность, COGS, сопровождение }; то есть, где применимо, должно:</span>
  - [ ] promote **safety best-practices** and prevent sources of risk
    - <span class="ru">способствовать **лучшим практикам безопасности** и предотвращать источники риска</span>
  - [ ] lead to **high throughput**, **low latency**, and **low memory usage**
    - <span class="ru">обеспечивать **высокую пропускную способность**, **низкую задержку** и **малое потребление памяти**</span>
  - [ ] make code **readable and understandable**
    - <span class="ru">делать код **читаемым и понятным**</span>
- [ ] A majority of experienced (3+ years) **Rust developers would agree with the guideline**.
  - <span class="ru">Большинство опытных (3+ года) **Rust-разработчиков согласилось бы с руководством**.</span>
- [ ] The guideline is **reasonably comprehensible** to Rust novices (4+ weeks).
  - <span class="ru">Руководство **достаточно понятно** новичкам в Rust (4+ недели).</span>
- [ ] It is **pragmatic**, as unrealistic guidelines won't be followed.
  - <span class="ru">Оно **прагматично**, так как нереалистичным руководствам никто не следует.</span>

## Applicability

Guidelines declared _must_ are supposed to always hold, where _should_ guidelines indicate more flexibility.

<span class="ru">Руководства со словом _must_ («должно») должны выполняться всегда, а _should_ («следует») допускают больше гибкости.</span>

That said, teams are free to adopt these guidelines as they see fit, and you occasionally might have good reasons to do things differently.

<span class="ru">Тем не менее команды вправе применять эти руководства по своему усмотрению, и порой у вас могут быть веские причины действовать иначе.</span>

We recommend you try to apply all items to your project. If an item does not make sense, get in touch. Either the item has
issues and we should update it, or it does not apply (and we might update it anyway to point these edge cases out).

<span class="ru">Мы рекомендуем применять все пункты к вашему проекту. Если пункт не имеет смысла, свяжитесь с нами: либо в пункте есть проблемы и его надо обновить, либо он неприменим (и мы всё равно можем обновить его, чтобы указать на такие крайние случаи).</span>

> ### <tip></tip> The Golden Rule
>
> <span class="ru">Золотое правило</span>
>
> Each item here exists for a reason; and it is the spirit that counts, not the letter.
>
> <span class="ru">Каждый пункт здесь существует не просто так; важен дух, а не буква.</span>
>
> Before attempting to work around a guideline, you should understand why it exists and what it tries to safeguard.
> Likewise, you should not blindly follow a guideline if it becomes apparent that doing so would violate its underlying motivation!
>
> <span class="ru">Прежде чем обходить руководство, поймите, зачем оно существует и что защищает. И не следуйте руководству слепо, если очевидно, что это нарушит его первоначальную цель!</span>

## Submitting New Guidelines

Do you have a practical guideline that leads to better safety, COGS or maintainability? We'd love to hear from you!

<span class="ru">Есть практичное руководство, улучшающее безопасность, COGS или сопровождаемость? Мы будем рады его услышать!</span>

Here is the process you should follow:

<span class="ru">Порядок действий:</span>

- Check if your guideline follows the [meta design principles](#meta-design-principles) above.
  - <span class="ru">Проверьте, что руководство соответствует [мета-принципам проектирования](#meta-design-principles) выше.</span>
- Check if your suggestion is not already covered by the [API Guidelines](https://rust-lang.github.io/api-guidelines/checklist.html) or [Clippy](https://rust-lang.github.io/rust-clippy/master/?groups).
  - <span class="ru">Проверьте, что предложение ещё не покрыто [API Guidelines](https://rust-lang.github.io/api-guidelines/checklist.html) или [Clippy](https://rust-lang.github.io/rust-clippy/master/?groups).</span>
- File a [PR or issue](https://github.com/microsoft/rust-guidelines).
  - <span class="ru">Создайте [PR или issue](https://github.com/microsoft/rust-guidelines).</span>

<div id="build-date">This book was last generated on: BUILD_DATE</div>
