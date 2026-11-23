<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Prefer types over generics, generics over dyn traits (M-DI-HIERARCHY) { #M-DI-HIERARCHY }

<span class="ru">Предпочитайте типы обобщениям, а обобщения — трейтам dyn (M-DI-HIERARCHY)</span>

<why>composable patterns and freedom from design lock-in.</why>

<span class="ru">компонуемые шаблоны и свобода от привязки к проектным решениям.</span>

When asking for async dependencies, prefer concrete types over generics, and generics over `dyn Trait`.

<span class="ru">Когда запрашиваете асинхронные зависимости, предпочитайте конкретные типы обобщениям, а обобщения — `dyn Trait`.</span>

It is easy to accidentally deviate from this pattern when porting code from languages like C# that heavily rely on interfaces.
Consider you are porting a service called `Database` from C# to Rust and, inspired by the original `IDatabase` interface, you naively translate it into:

<span class="ru">Легко невольно отойти от этого шаблона при переносе кода из языков вроде C#, которые активно используют интерфейсы. Представьте, что вы переносите сервис `Database` с C# на Rust и, вдохновившись исходным интерфейсом `IDatabase`, наивно переводите его в следующее:</span>

```rust,ignore
trait Database {
    async fn update_config(&self, file: PathBuf);
    async fn store_object(&self, id: Id, obj: Object);
    async fn load_object(&self, id: Id) -> Object;
}

impl Database for MyDatabase { ... }

// Intended to be used like this:
async fn start_service(b: Rc<dyn Database>) { ... }
```

Apart from not feeling idiomatic, this approach precludes other Rust constructs that conflict with object safety,
can cause issues with asynchronous code, and exposes wrappers (compare [M-AVOID-WRAPPERS]).

<span class="ru">Помимо того, что такой подход не выглядит идиоматичным, он исключает другие конструкции Rust, которые конфликтуют с объектной безопасностью, может вызывать проблемы с асинхронным кодом и раскрывает обёртки (сравните с [M-AVOID-WRAPPERS]).</span>

Instead, when more than one implementation is needed, this _design escalation ladder_ should be followed:

<span class="ru">Вместо этого, когда нужна более чем одна реализация, следует придерживаться этой _лестницы эскалации проектирования_:</span>

If the other implementation is only concerned with providing a _sans-io_ implementation for testing, implement your type as an
enum, following [M-MOCKABLE-SYSCALLS] instead.

<span class="ru">Если другая реализация нужна только для _sans-io_ реализации в целях тестирования, реализуйте свой тип как `enum`, следуя вместо этого [M-MOCKABLE-SYSCALLS].</span>

If users are expected to provide custom implementations, you should introduce one or more traits, and implement them for your own types
_on top_ of your inherent functions. Each trait should be relatively narrow, e.g., `StoreObject`, `LoadObject`. If eventually a single
trait is needed it should be made a subtrait, e.g., `trait DataAccess: StoreObject + LoadObject {}`.

<span class="ru">Если ожидается, что пользователи будут предоставлять собственные реализации, следует ввести один или несколько трейтов и реализовать их для своих типов _поверх_ собственных (inherent) функций. Каждый трейт должен быть относительно узким, например `StoreObject`, `LoadObject`. Если в итоге понадобится один трейт, его следует сделать надтрейтом, например `trait DataAccess: StoreObject + LoadObject {}`.</span>

Code working with these traits should ideally accept them as generic type parameters as long as their use does not contribute to significant nesting
(compare [M-ABSTRACTIONS-DONT-NEST]).

<span class="ru">Код, работающий с этими трейтами, в идеале должен принимать их как обобщённые параметры типа, пока их использование не создаёт значительной вложенности (сравните с [M-ABSTRACTIONS-DONT-NEST]).</span>

```rust,ignore
// Good, generic does not have infectious impact, uses only most specific trait
async fn read_database(x: impl LoadObject) { ... }

// Acceptable, unless further nesting makes this excessive.
struct MyService<T: DataAccess> {
    db: T,
}
```

Once generics become a nesting problem, `dyn Trait` can be considered. Even in this case, visible wrapping should be avoided, and custom wrappers should be preferred.

<span class="ru">Как только обобщения начинают создавать проблемы с вложенностью, можно рассмотреть `dyn Trait`. Даже в этом случае видимых обёрток следует избегать и предпочитать собственные обёртки.</span>

```rust
# use std::sync::Arc;
# trait DataAccess {
#     fn foo(&self);
# }
// This allows you to expand or change `DynamicDataAccess` later. You can also
// implement `DataAccess` for `DynamicDataAccess` if needed, and use it with
// regular generic functions.
struct DynamicDataAccess(Arc<dyn DataAccess>);

impl DynamicDataAccess {
    fn new<T: DataAccess + 'static>(db: T) -> Self {
        Self(Arc::new(db))
    }
}

struct MyService {
    db: DynamicDataAccess,
}
```

The generic wrapper can also be combined with the enum approach from [M-MOCKABLE-SYSCALLS]:

<span class="ru">Обобщённую обёртку также можно комбинировать с подходом на основе `enum` из [M-MOCKABLE-SYSCALLS]:</span>

```rust,ignore
enum DataAccess {
    MyDatabase(MyDatabase),
    Mock(mock::MockCtrl),
    Dynamic(DynamicDataAccess)
}

async fn read_database(x: &DataAccess) { ... }
```

[M-AVOID-WRAPPERS]: ./#M-AVOID-WRAPPERS
[M-MOCKABLE-SYSCALLS]: ../resilience/#M-MOCKABLE-SYSCALLS
[M-ABSTRACTIONS-DONT-NEST]: ./#M-ABSTRACTIONS-DONT-NEST
