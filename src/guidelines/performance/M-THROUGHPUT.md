<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Optimize for throughput, avoid empty cycles (M-THROUGHPUT) { #M-THROUGHPUT }

<span class="ru">Оптимизируйте под пропускную способность, избегайте пустых циклов (M-THROUGHPUT)</span>

<why>COGS savings at scale.</why>

<span class="ru">экономия COGS в масштабе.</span>

You should optimize your library for throughput, and one of your key metrics should be _items per CPU cycle_.

<span class="ru">Вам следует оптимизировать библиотеку под пропускную способность, и одной из ключевых метрик должно быть _количество элементов за такт CPU_.</span>

This does not mean to neglect latency&mdash;after all you can scale for throughput, but not for latency. However,
in most cases you should not pay for latency with _empty cycles_ that come with single-item processing, contended locks and frequent task switching.

<span class="ru">Это не означает пренебрежения задержкой&mdash;ведь масштабировать можно под пропускную способность, но не под задержку. Однако в большинстве случаев не следует платить за задержку _пустыми циклами_, которые возникают при обработке по одному элементу, при конкурентных блокировках и частом переключении задач.</span>

Ideally, you should

<span class="ru">В идеале вам следует:</span>

- partition reasonable chunks of work ahead of time,
  - <span class="ru">заранее разбивать работу на разумные куски,</span>
- let individual threads and tasks deal with their slice of work independently,
  - <span class="ru">позволять отдельным потокам и задачам независимо обрабатывать свою часть работы,</span>
- sleep or yield when no work is present,
  - <span class="ru">засыпать или уступать управление, когда работы нет,</span>
- design your own APIs for batched operations,
  - <span class="ru">проектировать собственные API для пакетных операций,</span>
- perform work via batched APIs where available,
  - <span class="ru">выполнять работу через пакетные API там, где они доступны,</span>
- yield within long individual items, or between chunks of batches (see [M-YIELD-POINTS]),
  - <span class="ru">уступать управление внутри долгих отдельных элементов или между частями пакетов (см. [M-YIELD-POINTS]),</span>
- exploit CPU caches, temporal and spatial locality.
  - <span class="ru">использовать кэши CPU, временную и пространственную локальность.</span>

You should not:

<span class="ru">Вам не следует:</span>

- hot spin to receive individual items faster,
  - <span class="ru">активно крутить цикл (hot spin), чтобы быстрее получать отдельные элементы,</span>
- perform work on individual items if batching is possible,
  - <span class="ru">обрабатывать отдельные элементы поштучно, если возможна пакетная обработка,</span>
- do work stealing or similar to balance individual items.
  - <span class="ru">использовать work stealing или подобное для балансировки отдельных элементов.</span>

Shared state should only be used if the cost of sharing is less than the cost of re-computation.

<span class="ru">Разделяемое состояние следует использовать только тогда, когда стоимость совместного использования меньше стоимости повторного вычисления.</span>

[M-YIELD-POINTS]: ./#M-YIELD-POINTS
