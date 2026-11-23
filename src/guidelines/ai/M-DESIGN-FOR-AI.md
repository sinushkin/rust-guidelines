<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Design with AI use in mind (M-DESIGN-FOR-AI) { #M-DESIGN-FOR-AI }

<span class="ru">Проектирование с учётом использования ИИ (M-DESIGN-FOR-AI)</span>

<why>maximum utility from agents working in your codebase.</why>

<span class="ru">максимальная польза от агентов, работающих в вашей кодовой базе.</span>

As a general rule, making APIs easier to use for humans also makes them easier to use by AI.
If you follow the guidelines in this book, you should be in good shape.

<span class="ru">Как правило, то, что упрощает использование API для людей, упрощает его и для ИИ. Если вы следуете руководствам этой книги, всё должно быть в порядке.</span>

Rust's strong type system is a boon for agents, as their lack of genuine understanding can often be
counterbalanced by comprehensive compiler checks, which Rust provides in abundance.

<span class="ru">Сильная система типов Rust — большое благо для агентов: их отсутствие подлинного понимания часто компенсируется обширными проверками компилятора, которых в Rust в избытке.</span>

With that said, there are a few guidelines which are particularly important to help make AI coding in Rust more effective:

<span class="ru">При этом есть несколько руководств, которые особенно важны для того, чтобы написание кода с помощью ИИ на Rust было эффективнее:</span>

* **Create Idiomatic Rust API Patterns**. The more your APIs, whether public or internal, look and feel like the majority of
Rust code in the world, the better it is for AI. Follow the [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/checklist.html)
along with the guidelines from [Library / UX](../libs/ux).
  - <span class="ru">**Создавайте идиоматичные шаблоны API Rust**. Чем больше ваши API, публичные или внутренние, выглядят и ощущаются как большинство Rust-кода в мире, тем лучше для ИИ. Следуйте [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/checklist.html) вместе с руководствами из [Library / UX](../libs/ux).</span>

* **Provide Thorough Docs**. Agents love good detailed docs. Include docs for all of your modules and public items in your crate.
Assume the reader has a solid, but not expert, level of understanding of Rust, and that the reader understands the standard library.
Follow
[C-CRATE-DOC](https://rust-lang.github.io/api-guidelines/checklist.html#c-crate-doc),
[C-FAILURE](https://rust-lang.github.io/api-guidelines/checklist.html#c-failure),
[C-LINK](https://rust-lang.github.io/api-guidelines/checklist.html#c-link), and
[M-MODULE-DOCS](../docs/#M-MODULE-DOCS)
[M-CANONICAL-DOCS](../docs/#M-CANONICAL-DOCS).
  - <span class="ru">**Пишите подробную документацию**. Агенты любят хорошую подробную документацию. Включайте документацию для всех модулей и публичных элементов вашего крейта. Считайте, что читатель обладает уверенным, но не экспертным знанием Rust и понимает стандартную библиотеку. Следуйте [C-CRATE-DOC](https://rust-lang.github.io/api-guidelines/checklist.html#c-crate-doc), [C-FAILURE](https://rust-lang.github.io/api-guidelines/checklist.html#c-failure), [C-LINK](https://rust-lang.github.io/api-guidelines/checklist.html#c-link), а также [M-MODULE-DOCS](../docs/#M-MODULE-DOCS) и [M-CANONICAL-DOCS](../docs/#M-CANONICAL-DOCS).</span>

* **Provide Thorough Examples**. Your documentation should have directly usable examples, the repository should include more elaborate ones.
Follow
[C-EXAMPLE](https://rust-lang.github.io/api-guidelines/checklist.html#c-example)
[C-QUESTION-MARK](https://rust-lang.github.io/api-guidelines/checklist.html#c-question-mark).
  - <span class="ru">**Приводите подробные примеры**. Документация должна содержать готовые к непосредственному использованию примеры, а в репозитории должны быть более развёрнутые. Следуйте [C-EXAMPLE](https://rust-lang.github.io/api-guidelines/checklist.html#c-example) и [C-QUESTION-MARK](https://rust-lang.github.io/api-guidelines/checklist.html#c-question-mark).</span>

* **Use Strong Types**. Avoid [primitive obsession](https://refactoring.guru/smells/primitive-obsession) by using strong types with strict well-documented semantics.
Follow
[C-NEWTYPE](https://rust-lang.github.io/api-guidelines/checklist.html#c-newtype).
  - <span class="ru">**Используйте строгие типы**. Избегайте [primitive obsession](https://refactoring.guru/smells/primitive-obsession), применяя строгие типы с точно задокументированной семантикой. Следуйте [C-NEWTYPE](https://rust-lang.github.io/api-guidelines/checklist.html#c-newtype).</span>

* **Make Your APIs Testable**. Design APIs which allow your customers to test their use of your API in unit tests. This might involve introducing some mocks, fakes,
or cargo features. AI agents need to be able to iterate quickly to prove that the code they are writing that calls your API is working
correctly.
  - <span class="ru">**Сделайте API тестируемыми**. Проектируйте API так, чтобы пользователи могли тестировать своё использование вашего API в модульных тестах. Для этого может понадобиться вводить моки, фейки или features cargo. Агентам ИИ нужно быстро итерировать, чтобы доказать, что написанный ими код, вызывающий ваш API, работает правильно.</span>

* **Ensure Test Coverage**. Your own code should have good test coverage over observable behavior.
This enables agents to work in a mostly hands-off mode when refactoring.
  - <span class="ru">**Обеспечьте покрытие тестами**. Ваш собственный код должен иметь хорошее покрытие тестами наблюдаемого поведения. Это позволяет агентам работать почти без присмотра при рефакторинге.</span>
