<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Rust code solves Rust problems (M-RUST-SHAPED) { #M-RUST-SHAPED }

<span class="ru">Код на Rust решает задачи Rust (M-RUST-SHAPED)</span>

<why>idiomatic code.</why>

<span class="ru">идиоматичный код.</span>

When (automatically) porting C#, Java, C++, or similar code to Rust, technical constructs must not be copied 1-on-1.

<span class="ru">При (автоматическом) портировании кода на C#, Java, C++ или аналогичных языках технические конструкции нельзя копировать один в один.</span>

It is prudent to separate domain aspects from language aspects. Domain aspects address business problems. An algorithm to compute prime numbers or logic for processing a customer table can (and should) work the same when translating between languages.

<span class="ru">Разумно отделять предметные аспекты от языковых. Предметные аспекты относятся к бизнес-задачам. Алгоритм вычисления простых чисел или логика обработки таблицы клиентов могут (и должны) работать одинаково при переводе между языками.</span>

However, many patterns exist to solve problems particular to the ecosystem they stem from. The Rust ecosystem has its own problems, and these need to be addressed by idioms that work for Rust. These include

<span class="ru">Однако многие шаблоны существуют для решения проблем, специфичных для экосистемы, из которой они происходят. У экосистемы Rust свои проблемы, и их нужно решать идиомами, которые работают для Rust. К ним относятся:</span>

- error handling,
  - <span class="ru">обработка ошибок,</span>
- management of tasks and threads,
  - <span class="ru">управление задачами и потоками,</span>
- component abstractions and their lifetimes,
  - <span class="ru">абстракции компонентов и их времена жизни,</span>
- ownership of parameters,
  - <span class="ru">владение параметрами,</span>
- the absence of 'object-oriented' programming,
  - <span class="ru">отсутствие «объектно-ориентированного» программирования,</span>
- structural differences between interfaces and traits,
  - <span class="ru">структурные различия между интерфейсами и трейтами,</span>
- and many others.
  - <span class="ru">и многие другие.</span>

While some language constructs simply don't translate at all (e.g., compared to C#, Rust does not have any meaningful reflection), others are deceptively similar and might only bite months down the line (e.g., statics, compare [M-AVOID-STATICS](../libs/resilience/#M-AVOID-STATICS)).

<span class="ru">Некоторые языковые конструкции вообще не переносятся (например, по сравнению с C# в Rust нет осмысленной рефлексии), а другие обманчиво похожи и могут проявиться лишь через месяцы (например, статики, см. [M-AVOID-STATICS](../libs/resilience/#M-AVOID-STATICS)).</span>

As a rule of thumb, structs and their methods can have vaguely similar names, flows, inputs and outputs, as far as their business functionality is concerned. However, any striking technical similarity between Rust and { C#, Java, Python, ... } implementations is indicative of deeper architectural problems; a `throw_if_null()` never makes sense.

<span class="ru">Как эмпирическое правило: с точки зрения бизнес-функциональности структуры и их методы могут иметь отдалённо похожие имена, потоки, входы и выходы. Однако любое явное техническое сходство между реализациями на Rust и { C#, Java, Python, ... } указывает на более глубокие архитектурные проблемы; `throw_if_null()` никогда не имеет смысла.</span>
