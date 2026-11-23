<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Avoid smart pointers and wrappers in APIs (M-AVOID-WRAPPERS) { #M-AVOID-WRAPPERS }

<span class="ru">Избегайте умных указателей и обёрток в API (M-AVOID-WRAPPERS)</span>

<why>low cognitive load and ergonomic APIs.</why>

<span class="ru">низкая когнитивная нагрузка и удобные API.</span>

As a specialization of [M-ABSTRACTIONS-DONT-NEST], generic wrappers and smart pointers like
`Rc<T>`, `Arc<T>`, `Box<T>`, or `RefCell<T>` should be avoided in public APIs.

<span class="ru">Как частный случай [M-ABSTRACTIONS-DONT-NEST], обобщённые обёртки и умные указатели, такие как `Rc<T>`, `Arc<T>`, `Box<T>` или `RefCell<T>`, следует избегать в публичных API.</span>

From a user perspective these are mostly implementation details, and introduce infectious complexity that users have to
resolve. In fact, these might even be impossible to resolve once multiple crates disagree about the required type of wrapper.

<span class="ru">С точки зрения пользователя это в основном детали реализации, которые вносят «заразную» сложность, и пользователям приходится с ней разбираться. Более того, эту сложность может быть невозможно устранить, если несколько крейтов расходятся в том, какой тип обёртки требуется.</span>

If wrappers are needed internally, they should be hidden behind a clean API that uses simple types like `&T`, `&mut T`, or `T` directly. Compare:

<span class="ru">Если обёртки нужны внутри, их следует скрыть за простым API, которое напрямую использует простые типы вроде `&T`, `&mut T` или `T`. Сравните:</span>

```rust,ignore
// Good: simple API
pub fn process_data(data: &Data) -> State { ... }
pub fn store_config(config: Config) -> Result<(), Error> { ... }

// Bad: Exposing implementation details
pub fn process_shared(data: Arc<Mutex<Shared>>) -> Box<Processed> { ... }
pub fn initialize(config: Rc<RefCell<Config>>) -> Arc<Server> { ... }
```

Smart pointers in APIs are acceptable when:

<span class="ru">Умные указатели в API допустимы, когда:</span>

- The smart pointer is fundamental to the API's purpose (e.g., a new container lib)
  - <span class="ru">умный указатель является основой назначения API (например, новой библиотеки контейнеров)</span>

- The smart pointer, based on benchmarks, significantly improves performance and the complexity is justified.
  - <span class="ru">по результатам бенчмарков умный указатель заметно повышает производительность, и сложность оправдана.</span>

[M-ABSTRACTIONS-DONT-NEST]: ./#M-ABSTRACTIONS-DONT-NEST
