<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Common settings come from the workspace Cargo.toml (M-CARGO-WORKSPACE) { #M-CARGO-WORKSPACE }

<span class="ru">Общие настройки берутся из Cargo.toml рабочего пространства (M-CARGO-WORKSPACE)</span>

<why>consistent, maintainable project configuration.</why>

<span class="ru">согласованная, сопровождаемая конфигурация проекта.</span>

Any repo with two or more crates that somehow belong together should unify these crates with a workspace `Cargo.toml`. Members then inherit shared metadata and dependency versions from the workspace root via `[workspace.dependencies]`, `[workspace.lints]`, ... rather than duplicating these values in each crate.

<span class="ru">Любой репозиторий с двумя и более крейтами, которые каким-либо образом связаны между собой, должен объединять их с помощью рабочего `Cargo.toml`. Члены рабочего пространства затем наследуют общие метаданные и версии зависимостей из корня рабочего пространства через `[workspace.dependencies]`, `[workspace.lints]`, ... вместо дублирования этих значений в каждом крейте.</span>

Where a dependency is crate-specific, it should still be defined in the workspace. Workspace definitions should generally not enable dependency features (except basic ones such as `["std"]`), and should instead use `default-features = false`.

<span class="ru">Даже если зависимость нужна только одному крейту, её всё равно следует объявлять в рабочем пространстве. Определения в рабочем пространстве, как правило, не должны включать фичи зависимостей (кроме базовых, например `["std"]`); вместо этого следует использовать `default-features = false`.</span>
