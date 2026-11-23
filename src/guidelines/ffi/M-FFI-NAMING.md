<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## FFI crates follow established naming conventions (M-FFI-NAMING) { #M-FFI-NAMING }

<span class="ru">Крейты FFI следуют устоявшимся соглашениям об именовании (M-FFI-NAMING)</span>

<why>immediately recognizable crate roles across projects.</why>

<span class="ru">мгновенно узнаваемые роли крейтов в разных проектах.</span>

Crates used for FFI should follow established naming practices:

<span class="ru">Крейты, используемые для FFI, должны следовать устоявшейся практике именования:</span>

- `-sys` for crates defining items to call into existing (C-style) libraries
  - <span class="ru">`-sys` для крейтов, определяющих элементы для вызова существующих библиотек (в стиле C)</span>
- `-ffi` for crates defining (C-style) items when called from existing applications
  - <span class="ru">`-ffi` для крейтов, определяющих элементы (в стиле C) для вызова из существующих приложений</span>

There are slight variations of this scheme (e.g., `-sys2` when a previous `-sys` crate was abandoned and using `-` vs `_`), but overall `-ffi` clearly defines 'export' libraries, and `-sys` 'import' ones.

<span class="ru">Существуют небольшие вариации этой схемы (например, `-sys2`, когда предыдущий крейт `-sys` был заброшен, и использование `-` вместо `_`), но в целом `-ffi` чётко обозначает библиотеки «экспорта», а `-sys` — библиотеки «импорта».</span>
