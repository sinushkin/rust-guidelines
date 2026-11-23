<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Production code uses telemetry, not println (M-LOG-NOT-PRINT) { #M-LOG-NOT-PRINT }

<span class="ru">Продакшн-код использует телеметрию, а не println</span>

<why>diagnostics available where they are needed.</why>

<span class="ru">диагностика доступна там, где она нужна.</span>

Production code paths should emit diagnostics through the project's telemetry framework rather than via `println!` or `dbg!`. Console output is reserved for CLIs that intentionally write to stdout as their user interface.

<span class="ru">Продакшн-код должен выводить диагностику через фреймворк телеметрии проекта, а не через `println!` или `dbg!`. Вывод в консоль зарезервирован за CLI, которые намеренно пишут в stdout как в свой пользовательский интерфейс.</span>
