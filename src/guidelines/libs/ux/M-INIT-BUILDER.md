<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Complex type construction has builders (M-INIT-BUILDER) { #M-INIT-BUILDER }

<span class="ru">Для сложного создания типов есть билдеры (M-INIT-BUILDER)</span>

<why>future-proof type construction in complex scenarios.</why>

<span class="ru">создание типов в сложных сценариях, устойчивое к будущим изменениям.</span>

Types that could support 4 or more arbitrary initialization permutations should provide builders. In other words, types with up to
2 optional initialization parameters can be constructed via inherent methods:

<span class="ru">Типы, которые могут поддерживать 4 и более произвольных вариантов инициализации, должны предоставлять билдеры. Другими словами, типы с не более чем 2 необязательными параметрами инициализации можно создавать через собственные (inherent) методы:</span>

```rust
# struct A;
# struct B;
struct Foo;

// Supports 2 optional construction parameters, inherent methods ok.
impl Foo {
    pub fn new() -> Self { Self }
    pub fn with_a(a: A) -> Self { Self }
    pub fn with_b(b: B) -> Self { Self }
    pub fn with_a_b(a: A, b: B) -> Self { Self }
}
```

Beyond that, types should provide a builder:

<span class="ru">Сверх этого типы должны предоставлять билдер:</span>

```rust, ignore
# struct A;
# struct B;
# struct C;
# struct Foo;
# struct FooBuilder;
impl Foo {
    pub fn new() -> Self { ... }
    pub fn builder() -> FooBuilder { ... }
}

impl FooBuilder {
    pub fn a(mut self, a: A) -> Self { ... }
    pub fn b(mut self, b: B) -> Self { ... }
    pub fn c(mut self, c: C) -> Self { ... }
    pub fn build(self) -> Foo { ... }
}

```

The proper name for a builder that builds `Foo` is `FooBuilder`. Its methods must be chainable, with the final method called
`.build()`. The buildable struct must have a shortcut `Foo::builder()`, while the builder itself should _not_ have a public
`FooBuilder::new()`. Builder methods that set a value `x` are called `x()`, not `set_x()` or similar.

<span class="ru">Правильное имя для билдера, который создаёт `Foo`, — `FooBuilder`. Его методы должны быть цепочечными, а последний метод называться `.build()`. У создаваемой структуры должен быть короткий путь `Foo::builder()`, а сам билдер не должен иметь публичного `FooBuilder::new()`. Методы билдера, которые задают значение `x`, называются `x()`, а не `set_x()` или похожим образом.</span>

### Builders and Required Parameters

<span class="ru">### Билдеры и обязательные параметры</span>

Required parameters should be passed when creating the builder, not as setter methods. For builders with multiple required
parameters, encapsulate them into a parameters struct and use the `deps: impl Into<Deps>` pattern to provide flexibility:

<span class="ru">Обязательные параметры следует передавать при создании билдера, а не через методы-сеттеры. Для билдеров с несколькими обязательными параметрами объедините их в структуру параметров и используйте шаблон `deps: impl Into<Deps>` для гибкости:</span>

> **Note:** A dedicated deps struct is not required if the builder has no required parameters or only a single simple parameter. However,
> for backward compatibility and API evolution, it's preferable to use a dedicated struct for deps even in simple cases, as it makes it
> easier to add new required parameters in the future without breaking existing code.
>
> <span class="ru">**Примечание:** Отдельная структура deps не обязательна, если у билдера нет обязательных параметров или есть только один простой параметр. Однако ради обратной совместимости и развития API предпочтительнее использовать отдельную структуру для deps даже в простых случаях, так как это упрощает добавление новых обязательных параметров в будущем без поломки существующего кода.</span>

```rust, ignore
#[derive(Debug, Clone)]
pub struct FooDeps {
    pub logger: Logger,
    pub config: Config,
}

impl From<(Logger, Config)> for FooDeps { ... }
impl From<Logger> for FooDeps { ... } // In case we could use default Config instance

impl Foo {
    pub fn builder(deps: impl Into<FooDeps>) -> FooBuilder { ... }
}
```

This pattern allows for convenient usage:

<span class="ru">Этот шаблон позволяет удобно использовать:</span>

- `Foo::builder(logger)` - when only the logger is needed
  - <span class="ru">`Foo::builder(logger)` — когда нужен только логгер</span>
- `Foo::builder((logger, config))` - when both parameters are needed
  - <span class="ru">`Foo::builder((logger, config))` — когда нужны оба параметра</span>
- `Foo::builder(FooDeps { logger, config })` - explicit struct construction
  - <span class="ru">`Foo::builder(FooDeps { logger, config })` — явное создание структуры</span>

Alternatively, you can use [`fundle`](https://docs.rs/fundle) to simplify the creation of `FooDeps`:

<span class="ru">Кроме того, для упрощения создания `FooDeps` можно использовать [`fundle`](https://docs.rs/fundle):</span>

```rust, ignore
#[derive(Debug, Clone)]
#[fundle::deps]
pub struct FooDeps {
    pub logger: Logger,
    pub config: Config,
}
```

This pattern enables "dependency injection", see [these docs](https://docs.rs/fundle/latest/fundle/attr.deps.html) for more details.

<span class="ru">Этот шаблон позволяет использовать «внедрение зависимостей», подробнее см. [в этой документации](https://docs.rs/fundle/latest/fundle/attr.deps.html).</span>

### Runtime-Specific Builders

<span class="ru">### Билдеры, специфичные для рантайма</span>

For types that are runtime-specific or require runtime-specific configuration, provide dedicated builder creation methods that accept the appropriate runtime parameters:

<span class="ru">Для типов, которые специфичны для рантайма или требуют конфигурации, зависящей от рантайма, предоставьте отдельные методы создания билдера, принимающие соответствующие параметры рантайма:</span>

```rust, ignore
#[cfg(feature="smol")]
#[derive(Debug, Clone)]
pub struct SmolDeps {
    pub clock: Clock,
    pub io_context: Context,
}

#[cfg(feature="tokio")]
#[derive(Debug, Clone)]
pub struct TokioDeps {
    pub clock: Clock,
}

impl Foo {
    #[cfg(feature="smol")]
    pub fn builder_smol(deps: impl Into<SmolDeps>) -> FooBuilder { ... }

    #[cfg(feature="tokio")]
    pub fn builder_tokio(deps: impl Into<TokioDeps>) -> FooBuilder { ... }
}
```

This approach ensures type safety at compile time and makes the runtime dependency explicit in the API surface. The resulting
builder methods follow the pattern `builder_{runtime}(deps)` where `{runtime}` indicates the specific runtime or execution environment.

<span class="ru">Такой подход обеспечивает типобезопасность на этапе компиляции и делает зависимость от рантайма явной в поверхности API. Получившиеся методы билдера следуют шаблону `builder_{runtime}(deps)`, где `{runtime}` указывает на конкретный рантайм или среду выполнения.</span>

### Further Reading

<span class="ru">### Дополнительные материалы</span>

- [Builder pattern in Rust: self vs. &mut self, and method vs. associated function](https://users.rust-lang.org/t/builder-pattern-in-rust-self-vs-mut-self-and-method-vs-associated-function/72892)
- [fundle](https://docs.rs/fundle)
