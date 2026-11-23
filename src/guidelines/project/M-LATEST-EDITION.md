<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## New crates target latest edition (M-LATEST-EDITION) { #M-LATEST-EDITION }

<span class="ru">Новые крейты ориентируются на последнюю редакцию (M-LATEST-EDITION)</span>

<why>access to the latest Rust features.</why>

<span class="ru">доступ к последним возможностям Rust.</span>

When creating a new crate or workspace, set `edition` to the latest stable edition (at least `2024` at the time of writing); the `resolver` field is generally not needed.

<span class="ru">При создании нового крейта или рабочего пространства задавайте `edition` последней стабильной редакцией (на момент написания — не ниже `2024`); поле `resolver`, как правило, не нужно.</span>

Using an older edition generally has no upsides for new projects, but forces you to write 'old Rust' that is less idiomatic and has worse UX edge cases. Notably, using an older edition does _not_ grant any compatibility benefits with the rest of the ecosystem. An application based on `2015` can use libraries written for `2024` just fine.

<span class="ru">Использование старой редакции, как правило, не даёт новым проектам никаких преимуществ, но вынуждает писать «старый Rust», который менее идиоматичен и имеет худший UX в пограничных случаях. Важно, что старая редакция _не_ даёт никаких преимуществ по совместимости с остальной экосистемой: приложение на основе `2015` вполне может использовать библиотеки, написанные для `2024`.</span>
