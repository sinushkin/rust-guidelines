<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## The workspace lists and versions all crates (M-CRATES-IN-WORKSPACE) { #M-CRATES-IN-WORKSPACE }

<span class="ru">Рабочее пространство перечисляет и версионирует все крейты (M-CRATES-IN-WORKSPACE)</span>

<why>simple inter-crate dependencies and debugging.</why>

<span class="ru">простые зависимости между крейтами и отладка.</span>

Every crate produced by the project should be listed as a workspace member, and its version should be declared in `[workspace.dependencies]` so that intra-workspace dependencies resolve to a single canonical version.

<span class="ru">Каждый крейт, создаваемый проектом, должен быть перечислен как член рабочего пространства, а его версия должна быть объявлена в `[workspace.dependencies]`, чтобы внутренние зависимости рабочего пространства разрешались в одну каноническую версию.</span>

```toml
# Bad, crate links its sibling directly
[dependencies]
sibling.path = "../sibling"


# Good, going through workspace
[dependencies]
sibling.workspace = true

[workspace.dependencies]
sibling = { path = "crates/sibling", version = "0.5.2" }
```
