<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Long-running tasks should have yield points (M-YIELD-POINTS) { #M-YIELD-POINTS }

<span class="ru">Долгие задачи должны иметь точки уступки управления (M-YIELD-POINTS)</span>

<why>fair CPU time for all tasks.</why>

<span class="ru">справедливое распределение времени CPU между всеми задачами.</span>

If you perform long running computations, they should contain `yield_now().await` points.

<span class="ru">Если вы выполняете долгие вычисления, они должны содержать точки `yield_now().await`.</span>

Your future might be executed in a runtime that cannot work around blocking or long-running tasks. Even then, such tasks are
considered bad design and cause runtime overhead. If your complex task performs I/O regularly it will simply utilize these await points to preempt itself:

<span class="ru">Ваш future может выполняться в рантайме, который не способен обойти блокирующие или долгие задачи. Даже в этом случае такие задачи считаются плохим дизайном и создают накладные расходы рантайма. Если ваша сложная задача регулярно выполняет I/O, она просто будет использовать эти точки await, чтобы вытеснять себя:</span>

```rust, ignore
async fn process_items(items: &[items]) {
    // Keep processing items, the runtime will preempt you automatically.
    for i in items {
        read_item(i).await;
    }
}
```

If your task performs long-running CPU operations without intermixed I/O, it should instead cooperatively yield at regular intervals, to not starve concurrent operations:

<span class="ru">Если ваша задача выполняет долгие операции на CPU без перемежающегося I/O, она должна вместо этого кооперативно уступать управление через регулярные промежутки, чтобы не лишать ресурсов одновременные операции:</span>

```rust, ignore
async fn process_items(zip_file: File) {
    let items = zip_file.read().async;
    for i in items {
        decompress(i);
        yield_now().await;
    }
}
```

If the number and duration of your individual operations are unpredictable you should use APIs such as `has_budget_remaining()` and
related APIs to query your hosting runtime.

<span class="ru">Если количество и длительность ваших отдельных операций непредсказуемы, следует использовать API вроде `has_budget_remaining()` и связанные с ним API, чтобы опрашивать хостящий вас рантайм.</span>

> ### <tip></tip> Yield how often?
>
> <span class="ru">Как часто уступать управление?</span>
>
> In a thread-per-core model the overhead of task switching must be balanced against the systemic effects of starving unrelated tasks.
>
> <span class="ru">В модели «поток на ядро» накладные расходы на переключение задач должны быть уравновешены системными эффектами голодания несвязанных задач.</span>
>
> Under the assumption that runtime task switching takes 100's of ns, in addition to the overhead of lost CPU caches,
> continuous execution in between should be long enough that the switching cost becomes negligible (<1%).
>
> <span class="ru">В предположении, что переключение задач в рантайме занимает сотни нс, плюс накладные расходы из-за потерянных кэшей CPU, непрерывное выполнение между переключениями должно быть достаточно долгим, чтобы стоимость переключения стала пренебрежимо малой (<1%).</span>
>
> Thus, performing 10 - 100μs of CPU-bound work between yield points would be a good starting point.
>
> <span class="ru">Поэтому 10–100 мкс работы, ограниченной CPU, между точками уступки управления будет хорошей отправной точкой.</span>
