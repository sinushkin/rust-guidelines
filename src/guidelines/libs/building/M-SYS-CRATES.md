<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Native `-sys` crates compile without dependencies (M-SYS-CRATES) { #M-SYS-CRATES }

<span class="ru">Нативные `-sys` крейты собираются без зависимостей (M-SYS-CRATES)</span>

<why>libraries that just work on all platforms.</why>

<span class="ru">библиотеки, которые просто работают на всех платформах.</span>

If you author a pair of `foo` and `foo-sys` crates wrapping a native `foo.lib`, you are likely to run into the issues described
in [M-OOBE].

<span class="ru">Если вы создаёте пару крейтов `foo` и `foo-sys`, оборачивающих нативную `foo.lib`, вероятно, вы столкнётесь с проблемами, описанными в [M-OOBE].</span>

Follow these steps to produce a crate that _just works_ across platforms:

<span class="ru">Чтобы получить крейт, который _просто работает_ на всех платформах, выполните следующие шаги:</span>

- [ ] fully govern the build of `foo.lib` from `build.rs` inside `foo-sys`. Only use hand-crafted compilation via the
  [cc](https://crates.io/crates/cc) crate, do _not_ run Makefiles or external build scripts, as that will require the installation of external dependencies,
  - <span class="ru">полностью управлять сборкой `foo.lib` из `build.rs` внутри `foo-sys`. Используйте только собственноручно написанную сборку через крейт [cc](https://crates.io/crates/cc), а _не_ запускайте Makefile или внешние скрипты сборки, так как это потребует установки внешних зависимостей;</span>
- [ ] make all external tools optional, such as `nasm`,
  - <span class="ru">сделать все внешние инструменты необязательными, например `nasm`;</span>
- [ ] embed the upstream source code in your crate,
  - <span class="ru">встроить исходный код upstream в свой крейт;</span>
- [ ] make the embedded sources verifiable (e.g., include Git URL + hash),
  - <span class="ru">сделать встроенные исходники проверяемыми (например, указав URL Git + хеш);</span>
- [ ] pre-generate `bindgen` glue if possible,
  - <span class="ru">по возможности заранее генерировать обвязку `bindgen`;</span>
- [ ] support both static linking, and dynamic linking via [libloading](https://crates.io/crates/libloading).
  - <span class="ru">поддерживать как статическую линковку, так и динамическую через [libloading](https://crates.io/crates/libloading).</span>

Deviations from these points can work, and can be considered on a case-by-case basis:

<span class="ru">Отступления от этих пунктов могут работать и рассматриваются в каждом случае отдельно:</span>

If the native build system is available as an _OOBE_ crate, that can be used instead of `cc` invocations. The same applies to external tools.

<span class="ru">Если нативная система сборки доступна в виде крейта _OOBE_, его можно использовать вместо вызовов `cc`. То же относится и к внешним инструментам.</span>

Source code might have to be downloaded if it does not fit crates.io size limitations. In any case, only servers with an availability
comparable to crates.io should be used. In addition, the specific hashes of acceptable downloads should be stored in the crate and verified.

<span class="ru">Исходный код может потребоваться загружать, если он не помещается в ограничения размера crates.io. В любом случае следует использовать только серверы с доступностью, сопоставимой с crates.io. Кроме того, конкретные хеши допустимых загрузок должны храниться в крейте и проверяться.</span>

Downloading sources can fail on hermetic build environments, therefore alternative source roots should also be specifiable (e.g., via environment variables).

<span class="ru">Загрузка исходников может не работать в герметичных средах сборки, поэтому также должна быть возможность указать альтернативные корни исходников (например, через переменные окружения).</span>

[M-OOBE]: ./#M-OOBE
