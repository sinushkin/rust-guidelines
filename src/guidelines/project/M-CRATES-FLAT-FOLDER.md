<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## All crates are siblings in one folder (M-CRATES-FLAT-FOLDER) { #M-CRATES-FLAT-FOLDER }

<span class="ru">Все крейты — соседи в одной папке (M-CRATES-FLAT-FOLDER)</span>

<why>simple project navigation and a standard Rust layout.</why>

<span class="ru">простая навигация по проекту и стандартная структура Rust-проекта.</span>

A repository should contain a single workspace `Cargo.toml`, and all Rust crates should be subordinate to it. All crates should then live in a single, direct subdirectory (e.g., `crates/`) below the workspace (for up to 1-2 dozen of crates), beyond that some folder hierarchy should be used (e.g., `common/`, `server/`, `client/`) to organize siblings.

<span class="ru">Репозиторий должен содержать один рабочий `Cargo.toml`, и все Rust-крейты должны ему подчиняться. Все крейты должны располагаться в одной непосредственной подпапке (например, `crates/`) под рабочим пространством (для до 1–2 десятков крейтов); при большем количестве следует использовать некоторую иерархию папок (например, `common/`, `server/`, `client/`) для упорядочивания соседних крейтов.</span>

```bash
# Ideal for most workspaces
Cargo.toml
crates/
  foo/Cargo.toml 
  foo_core/Cargo.toml 
  foo_proc/Cargo.toml 
  foo_tests/Cargo.toml 
  bar/Cargo.toml
  baz/Cargo.toml


# Ok for large workspaces
Cargo.toml
crates/
  server/
    main/Cargo.toml 
    routes/Cargo.toml 
  client/
    foo/Cargo.toml
    bar/Cargo.toml
  common/
    error/Cargo.toml
```

Placing crates inside other crates (at or below their `Cargo.toml`), or even inside their `src/` folder is never acceptable. If a crate relationship should be expressed, this is done via common prefixes instead (e.g., `foo`, `foo_util`, `foo_build`).

```bash
# Never acceptable, crates inside `src/` folder
Cargo.toml
crates/
  foo/Cargo.toml 
    src/lib.rs
       deps/bar/Cargo.toml 
```

Rare exceptions to this rule can occur if your crate is in the business of processing workspaces and has a collection of UI tests or similar it relies on; but even then these are usually dummy crates in nature.
