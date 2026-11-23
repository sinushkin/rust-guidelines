<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Library telemetry does not tank performance (M-LOG-OVERHEAD) { #M-LOG-OVERHEAD }

<span class="ru">Телеметрия библиотеки не должна губить производительность (M-LOG-OVERHEAD)</span>

<why>low-overhead telemetry during diagnosis.</why>

<span class="ru">телеметрия с низкими накладными расходами во время диагностики.</span>

Library code that emits telemetry should ensure that doing so does not meaningfully impact throughput or latency on the hot path.

<span class="ru">Код библиотеки, испускающий телеметрию, должен гарантировать, что это не оказывает заметного влияния на пропускную способность или задержку на горячем пути.</span>

Crates offered to 3rd parties emitting logs or metrics should assume telemetry will be permanently enabled, or under load. Care should therefore be taken that the volume and overhead of emitted events is reasonable, and will not cause excessive performance degradation.

<span class="ru">Крейты, предлагаемые сторонним разработчикам и испускающие логи или метрики, должны исходить из того, что телеметрия будет постоянно включена или работать под нагрузкой. Поэтому следует следить за тем, чтобы объём и накладные расходы испускаемых событий были разумными и не приводили к чрезмерной деградации производительности.</span>

Hot, inner loops should preferably stay free of telemetry emission entirely. If it can't be avoided, the events emitted should be lightweight and avoid allocations (e.g., `format!` string concatenation).

<span class="ru">Горячие внутренние циклы желательно оставлять полностью свободными от телеметрии. Если этого нельзя избежать, испускаемые события должны быть легковесными и не выделять память (например, без конкатенации строк через `format!`).</span>

```rust,ignore
// Bad, logs each message and invokes allocation-based formatting.
for m in messages {
    log(format!("Emitting message {}", m.id()))
}

// Better, avoids per-message allocations.
for m in messages {
    log(("Emitting message", m.id()))
}

// Best: If possible, let telemetry users reconstruct what happened offline 
log(("Processing message batch", messages.batch_id()))
for m in messages { ... }
```
