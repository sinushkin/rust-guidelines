<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Services are Clone (M-SERVICES-CLONE) { #M-SERVICES-CLONE }

<span class="ru">Сервисы реализуют Clone (M-SERVICES-CLONE)</span>

<why>composable sharing of common services.</why>

<span class="ru">компонуемое совместное использование общих сервисов.</span>

Heavyweight _service_ types and 'thread singletons' should implement shared-ownership `Clone` semantics, including any type you expect to be used from your `Application::init`.

<span class="ru">Тяжеловесные типы _сервисов_ и «синглтоны потока» должны реализовывать семантику `Clone` с разделяемым владением, включая любой тип, который, как вы ожидаете, будет использоваться из вашего `Application::init`.</span>

Per thread, users should essentially be able to create a single resource handler instance, and have it reused by other handlers on the same thread:

<span class="ru">В каждом потоке пользователи должны иметь возможность создать один экземпляр обработчика ресурса и повторно использовать его другими обработчиками в том же потоке:</span>

```rust,ignore
impl ThreadLocal for MyThreadState {
    fn init(...) -> Self {

        // Create common service instance possibly used by many.
        let common = ServiceCommon::new();

        // Users can freely pass `common` here multiple times
        let service_1 = ServiceA::new(&common);
        let service_2 = ServiceA::new(&common);

        Self { ... }
    }
}
```

Services then simply clone their dependency and store a new _handle_, as if `ServiceCommon` were a shared-ownership smart pointer:

<span class="ru">Затем сервисы просто клонируют свою зависимость и сохраняют новый _дескриптор_, как если бы `ServiceCommon` был умным указателем с разделяемым владением:</span>

```rust,ignore
impl ServiceA {
    pub fn new(common: &ServiceCommon) -> Self {
        // If we only need to access `common` from `new` we don't have
        // to store it. Otherwise, make a clone we store in `Self`.
        let common = common.clone();
    }
}
```

Under the hood this `Clone` should **not** create a fat copy of the entire service. Instead, it should follow the `Arc<Inner>` pattern:

<span class="ru">Внутри такой `Clone` должен **не** создавать полную копию всего сервиса. Вместо этого он должен следовать шаблону `Arc<Inner>`:</span>

```rust, ignore
// Actual service containing core logic and data.
struct ServiceCommonInner {}

#[derive(Clone)]
pub ServiceCommon {
    inner: Arc<ServiceCommonInner>
}

impl ServiceCommon {
    pub fn new() {
        Self { inner: Arc::new(ServiceCommonInner::new()) }
    }

    // Method forwards ...
    pub fn foo(&self) { self.inner.foo() }
    pub fn bar(&self) { self.inner.bar() }
}
```
