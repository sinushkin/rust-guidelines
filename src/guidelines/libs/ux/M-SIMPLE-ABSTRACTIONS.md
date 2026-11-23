<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Abstractions don't visibly nest (M-SIMPLE-ABSTRACTIONS) { #M-SIMPLE-ABSTRACTIONS }

<span class="ru">Абстракции не создают видимой вложенности (M-SIMPLE-ABSTRACTIONS)</span>

<why>low cognitive load and a good out-of-the-box UX.</why>

<span class="ru">низкая когнитивная нагрузка и хороший UX «из коробки».</span>

When designing your public types and primary API surface, avoid exposing nested or complex parametrized types to your users.

<span class="ru">При проектировании публичных типов и основной поверхности API избегайте предоставления пользователям вложенных или сложных параметризованных типов.</span>

While powerful, type parameters introduce a cognitive load, even more so if the involved traits are crate-specific. Type parameters
become infectious to user code holding on to these types in their fields, often come with complex trait hierarchies on their own, and
might cause confusing error messages.

<span class="ru">Параметры типа, будучи мощным средством, создают когнитивную нагрузку, и тем сильнее, если задействованные трейты специфичны для крейта. Параметры типа становятся «заразными» для пользовательского кода, который хранит эти типы в своих полях, часто сами по себе тянут сложные иерархии трейтов и могут порождать запутанные сообщения об ошибках.</span>

From the perspective of a user authoring `Foo`, where the other structs come from your crate:

<span class="ru">С точки зрения пользователя, который пишет `Foo`, где остальные структуры поставляются вашим крейтом:</span>

```rust,ignore
struct Foo {
    service: Service // Great
    service: Service<Backend> // Acceptable
    service: Service<Backend<Store>> // Bad

    list: List<Rc<u32>> // Great, `List<T>` is simple container,
                        // other types user provided.

    matrix: Matrix4x4 // Great
    matrix: Matrix4x4<f32> // Still ok
    matrix: Matrix<f32, Const<4>, Const<4>, ArrayStorage<f32, 4, 4>> // ?!?
}
```

_Visible_ type parameters should be avoided in _service-like_ types (i.e., types mainly instantiated once per thread / application that are often passed
as dependencies), in particular if the nestee originates from the same crate as the service.

<span class="ru">_Видимых_ параметров типа следует избегать в _сервисоподобных_ типах (то есть в типах, которые в основном создаются один раз на поток или приложение и часто передаются как зависимости), в особенности если вложенный тип происходит из того же крейта, что и сервис.</span>

Containers, smart-pointers and similar data structures obviously must expose a type parameter, e.g., `List<T>` above. Even then, care should
be taken to limit the number and nesting of parameters.

<span class="ru">Контейнеры, умные указатели и подобные структуры данных, очевидно, обязаны предоставлять параметр типа, например `List<T>` выше. Даже в этом случае следует стараться ограничивать количество и вложенность параметров.</span>

To decide whether type parameter nesting should be avoided, consider these factors:

<span class="ru">Чтобы решить, следует ли избегать вложенности параметров типа, учитывайте следующие факторы:</span>

- Will the type be **named** by your users?
  - <span class="ru">Будет ли тип **назван** вашими пользователями?</span>
  - Service-level types are always expected to be named (e.g., `Library<T>`),
    - <span class="ru">от типов уровня сервиса всегда ожидается, что их назовут (например, `Library<T>`),</span>
  - Utility types, such as the many [`std::iter`](https://doc.rust-lang.org/stable/std/iter/index.html) types like `Chain`, `Cloned`, `Cycle`, are not
    expected to be named.
    - <span class="ru">служебные типы, такие как многие типы [`std::iter`](https://doc.rust-lang.org/stable/std/iter/index.html) вроде `Chain`, `Cloned`, `Cycle`, не ожидается называть явно.</span>
- Does the type primarily compose with non-user types?
  - <span class="ru">Компонуется ли тип в основном с типами, не принадлежащими пользователю?</span>
- Do the used type parameters have complex bounds?
  - <span class="ru">Имеют ли используемые параметры типа сложные ограничения (bounds)?</span>
- Do the used type parameters affect inference in other types or functions?
  - <span class="ru">Влияют ли используемые параметры типа на вывод типов в других типах или функциях?</span>

The more of these factors apply, the bigger the cognitive burden.

<span class="ru">Чем больше из этих факторов применимы, тем больше когнитивная нагрузка.</span>

As a rule of thumb, primary service API types should not nest _on their own volition_, and if they do, only 1 level deep. In other words, these
APIs should not require users having to deal with an `Foo<Bar<FooBar>>`. However, if `Foo<T>` users want to bring their own `A<B<C>>` as `T` they
should be free to do so.

<span class="ru">Как эмпирическое правило, основные типы API сервисов не должны вкладываться _по собственной воле_, а если вкладываются, то не глубже одного уровня. Другими словами, API не должны заставлять пользователей работать с `Foo<Bar<FooBar>>`. Однако если пользователи `Foo<T>` хотят передать свой `A<B<C>>` в качестве `T`, они должны иметь такую возможность.</span>

> ### <tip></tip> Type Magic for Better UX?
>
> <span class="ru">Магия типов ради лучшего UX?</span>
>
> The guideline above is written with 'bread-and-butter' types in mind you might create during  _normal_ development activity. Its intention is to
> reduce friction users encounter when working with your code.
>
> <span class="ru">Руководство выше написано с учётом «обычных» типов, которые вы можете создавать в ходе _обычной_ разработки. Его цель — уменьшить трение, с которым пользователи сталкиваются при работе с вашим кодом.</span>
>
> However, when designing API patterns and ecosystems at large, there might be valid reasons to introduce intricate type magic to overall _lower_
> the cognitive friction involved, [Bevy's ECS](https://docs.rs/bevy_ecs/latest/bevy_ecs/) or
> [Axum's request handlers](https://docs.rs/axum/latest/axum/handler/trait.Handler.html) come to mind.
>
> <span class="ru">Однако при проектировании паттернов API и экосистем в целом могут быть веские причины ввести сложную магию типов, чтобы в целом _снизить_ когнитивное трение; на ум приходят [ECS Bevy](https://docs.rs/bevy_ecs/latest/bevy_ecs/) или [обработчики запросов Axum](https://docs.rs/axum/latest/axum/handler/trait.Handler.html).</span>
>
> The threshold where this pays off is high though. If there is any doubt about the utility of your creative use of generics, your users might be
> better off without them.
>
> <span class="ru">Однако порог, при котором это окупается, высок. Если есть сомнения в пользе вашего творческого использования обобщений, пользователям может быть лучше без них.</span>
