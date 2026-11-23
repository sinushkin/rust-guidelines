<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## MSRV is conservatively updated (M-MSRV) { #M-MSRV }

<span class="ru">MSRV обновляется консервативно (M-MSRV)</span>

<why>modern features with stability for users.</why>

<span class="ru">современные возможности при стабильности для пользователей.</span>

A Minimum Supported Rust Version (MSRV) should be set when libraries are first created. It can be updated as new Rust features are needed, but should be kept a few versions behind the most recent compiler release.

<span class="ru">Минимальную поддерживаемую версию Rust (MSRV) следует задавать при первом создании библиотеки. Её можно обновлять по мере необходимости новых возможностей Rust, но следует держать на несколько версий позади самого свежего выпуска компилятора.</span>

The ecosystem expectation is that projects are compiled with a _reasonably modern_ Rust compiler.

<span class="ru">Экосистема ожидает, что проекты собираются _достаточно современным_ компилятором Rust.</span>

Bumping MSRV therefore does not require a major release, but can be handled through a minor update (e.g., `1.3` to `1.4`). In fact, any project depending on 3rd party crates is already inherently bound to this contract; forcing a major version bump will not confer any benefits, but could possibly bifurcate downstream dependencies.

<span class="ru">Поэтому повышение MSRV не требует мажорного релиза, его можно выпустить в рамках минорного обновления (например, `1.3` → `1.4`). Более того, любой проект, зависящий от сторонних крейтов, уже по сути связан этим соглашением; принудительное повышение мажорной версии не даст никаких преимуществ, но может расколоть зависящие от него крейты.</span>
