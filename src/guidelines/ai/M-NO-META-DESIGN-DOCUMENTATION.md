<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Avoid meta design documentation (M-NO-META-DESIGN-DOCUMENTATION) { #M-NO-META-DESIGN-DOCUMENTATION }

<span class="ru">Избегайте мета-документации о проектировании (M-NO-META-DESIGN-DOCUMENTATION)</span>

<why>docs focused on what is relevant to users.</why>

<span class="ru">документация, сосредоточенная на том, что важно пользователям.</span>

Crate and module documentation must be free of meta design narratives that were only relevant during the creation of a crate. In other words, it is the end-state that is to be documented, not the design journey.

<span class="ru">Документация крейтов и модулей должна быть свободна от мета-повествований о проектировании, которые были важны только при создании крейта. Иными словами, документируется конечное состояние, а не путь проектирования.</span>

Agents frequently produce sections that describe how a change was designed, "why we picked X over Y" essays, and design journals inside user-facing documentation. These artifacts might be interesting diagnostics while working on the project, but they are mostly meaningless to end users.

<span class="ru">Агенты часто создают разделы, описывающие, как был спроектирован тот или иной вариант изменения, эссе в духе «почему мы выбрали X, а не Y» и дневники проектирования прямо в пользовательской документации. Такие артефакты могут быть интересной диагностикой во время работы над проектом, но конечным пользователям они почти бесполезны.</span>

For example, an agent might append a self-report like this, summarizing which guidelines it claims to have followed:

<span class="ru">Например, агент может добавить такой самоотчёт, суммирующий, каким руководствам, по его утверждению, он следовал:</span>

```text
| Rule | Applied | Where |
| --- | :---: | --- |
| M-SHORT-NAMES | ✅ | Shortened method names across the data-access and HTTP handler layers. |
| M-WEASEL-WORDS | ✅ | Removed weasel words from type and field names throughout the public API. |
| M-PUBLIC-DISPLAY | ✅ | Added `Display` impls for all user-facing identifier and error types. |
| M-ASYNC-FN | ✅ | Migrated I/O-facing APIs from `impl Future` returns to `async fn`. |
```

This kind of content describes process, not behaviour, and goes stale over time. That said, it is of course perfectly reasonable to have a _Design Principles_ or similar section in the project's README, that on a high level describes the enduring architectural goals that are relevant to end users (e.g., a crate being allocation free, having an OSI architecture, or being designed with `#[no_std]` in mind).

<span class="ru">Такое содержимое описывает процесс, а не поведение, и со временем устаревает. Тем не менее вполне разумно иметь в README проекта раздел _Design Principles_ или подобный, который на высоком уровне описывает устойчивые архитектурные цели, важные для конечных пользователей (например, что крейт не выделяет память, имеет архитектуру OSI или спроектирован с расчётом на `#[no_std]`).</span>
