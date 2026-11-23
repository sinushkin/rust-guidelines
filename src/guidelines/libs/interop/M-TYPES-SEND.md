<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Types are Send (M-TYPES-SEND) { #M-TYPES-SEND }

<span class="ru">Типы реализуют Send (M-TYPES-SEND)</span>

<why>use in Tokio and behind runtime abstractions.</why>

<span class="ru">использование с Tokio и за абстракциями рантайма.</span>

Public types should be `Send` for compatibility reasons:

<span class="ru">Публичные типы должны реализовывать `Send` по соображениям совместимости:</span>

- All futures produced (explicitly or implicitly) must be `Send`
  - <span class="ru">Все производимые (явно или неявно) футуры должны быть `Send`</span>
- Most other types should be `Send`, but there might be exceptions
  - <span class="ru">Большинство остальных типов следует делать `Send`, но возможны исключения</span>

### Futures

<span class="ru">Футуры (futures)</span>

When declaring a future explicitly you should ensure it is, and remains, `Send`.

<span class="ru">При явном объявлении футуры следует убедиться, что она является `Send` и остаётся таковой.</span>

```rust
# use std::future::Future;
# use std::pin::Pin;
# use std::task::{Context, Poll};
#
struct Foo {}

impl Future for Foo {
    // Explicit implementation of `Future` for your type
    # type Output = ();
    #
    # fn poll(self: Pin<&mut Self>, _: &mut Context<'_>) -> Poll<<Self as Future>::Output> { todo!() }
}

// You should assert your type is `Send`
const fn assert_send<T: Send>() {}
const _: () = assert_send::<Foo>();
```

When returning futures implicitly through `async` method calls, you should make sure these are `Send` too.
You do not have to test every single method, but you should at least validate your main entry points.

<span class="ru">При неявном возврате футур через вызовы `async`-методов их тоже следует делать `Send`. Не обязательно проверять каждый метод, но основные точки входа проверить стоит.</span>

```rust,edition2021
async fn foo() { }

// TODO: We want this as a macro as well
fn assert_send<T: Send>(_: T) {}
_ = assert_send(foo());
```

### Regular Types

<span class="ru">Обычные типы</span>

Most regular types should be `Send`, as they otherwise infect futures turning them `!Send` if held across `.await` points.

<span class="ru">Большинство обычных типов следует делать `Send`, так как иначе они «заражают» футуры, делая их `!Send`, если удерживаются через точки `.await`.</span>

```rust,edition2021
# use std::rc::Rc;
# async fn read_file(x: &str) {}
#
async fn foo() {
    let rc = Rc::new(123);      // <-- Holding this across an .await point prevents
    read_file("foo.txt").await; //     the future from being `Send`.
    dbg!(rc);
}
```

That said, if the default use of your type is _instantaneous_, and there is no reason for it to be otherwise held across `.await` boundaries, it may be `!Send`.

<span class="ru">Впрочем, если типичное использование вашего типа _мгновенно_ и нет причин удерживать его через границы `.await`, он может быть `!Send`.</span>

```rust,edition2021
# use std::rc::Rc;
# struct Telemetry; impl Telemetry { fn ping(&self, _: u32) {} }
# fn telemetry() -> Telemetry  { Telemetry }
# async fn read_file(x: &str) {}
#
async fn foo() {
    // Here a hypothetical instance Telemetry is summoned
    // and used ad-hoc. It may be ok for Telemetry to be !Send.
    telemetry().ping(0);
    read_file("foo.txt").await;
    telemetry().ping(1);
}
```

> ### <tip></tip> The Cost of Send
>
> <span class="ru">Цена Send</span>
>
> Ideally, there would be abstractions that are `Send` in work-stealing runtimes, and `!Send` in thread-per-core models based on non-atomic
> types like `Rc` and `RefCell` instead.
>
> <span class="ru">Идеально было бы иметь абстракции, которые `Send` в work-stealing рантаймах и `!Send` в моделях thread-per-core на основе неатомарных типов, таких как `Rc` и `RefCell`.</span>
>
> Practically these abstractions don't exist, preventing Tokio compatibility in the non-atomic case. That in turn means you would have to
> "reinvent the world" to get anything done in a thread-per-core universe.
>
> <span class="ru">На практике таких абстракций не существует, поэтому нет совместимости с Tokio в неатомарном случае. Значит, чтобы сделать что-либо в мире thread-per-core, пришлось бы «заново изобретать мир».</span>
>
> The good news is, in most cases atomics and uncontended locks only have a measurable impact if accessed more frequently than every 64 words or so.
>
> <span class="ru">Хорошая новость: в большинстве случаев атомики и неконкурентные блокировки заметно влияют на производительность, только если к ним обращаются чаще, чем примерно раз в 64 слова.</span>
>
> <div style="background-color:white;">
>
> ![TEXT](M-TYPES-SEND.png)
>
> </div>
>
> Working with a large `Vec<AtomicUsize>` in a hot loop is a bad idea, but doing the occasional uncontended atomic operation from otherwise thread-per-core
> async code has no performance impact, but gives you widespread ecosystem compatibility.
>
> <span class="ru">Работа с большим `Vec<AtomicUsize>` в горячем цикле — плохая идея, но редкая неконкурентная атомарная операция в async-коде thread-per-core не влияет на производительность и даёт широкую совместимость с экосистемой.</span>
