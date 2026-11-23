<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Reuse allocations where possible (M-MEM-REUSE) { #M-MEM-REUSE }

<span class="ru">Переиспользуйте аллокации там, где это возможно (M-MEM-REUSE)</span>

<why>low allocation overhead and fast hot paths.</why>

<span class="ru">низкие накладные расходы на аллокации и быстрые горячие пути.</span>

When designing APIs you should allow users to hold onto reusable resources. Inside your code you should make use of them where available.

<span class="ru">При проектировании API следует позволять пользователям удерживать переиспользуемые ресурсы. Внутри вашего кода их следует использовать там, где они доступны.</span>

The cost of repeated allocations inside hot loops can be significant, and from a user's perspective they can be invisible unless profiled:

<span class="ru">Стоимость повторных аллокаций внутри горячих циклов может быть значительной, и с точки зрения пользователя она может быть незаметна, если не проводить профилирование:</span>

```rust,ignore
// Bad, API design forces new allocation per element.
for id in ids {
    let value = db.get(id);
}
```

While this style of API may exist for convenience, it should be auxiliary. Instead, the core APIs should allow users to own the underlying object and re-use it:

<span class="ru">Хотя такой стиль API может существовать для удобства, он должен быть вспомогательным. Вместо этого базовые API должны позволять пользователям владеть нижележащим объектом и переиспользовать его:</span>

```rust,ignore
// Good, allows users to decide whether a new allocation is needed.
let mut value = Value::new();
for id in ids {
    db.get_in(id, &mut value);
}
```

The canonical method on reusable types to reuse them is `.clear()`, as can be found on many `std` items. Multiple flavors of this pattern exist. In simple cases user-owned types can hold a preexisting, reusable collection directly:

<span class="ru">Каноничный метод переиспользования для переиспользуемых типов — `.clear()`, как и у многих элементов `std`. Существует несколько вариантов этого паттерна. В простых случаях типы, принадлежащие пользователю, могут напрямую хранить уже существующую переиспользуемую коллекцию:</span>

```rust
struct Value {
    data: Vec<u8>
}
```

In heavyweight, deeply nested libraries it can be worthwhile to either pass a bump-style `Arena`, or to encapsulate one inside the user types, so it can be used throughout the call stack:

<span class="ru">В тяжёлых, глубоко вложенных библиотеках может быть полезно либо передавать `Arena` в стиле bump-аллокатора, либо инкапсулировать его внутри пользовательских типов, чтобы его можно было использовать по всему стеку вызовов:</span>

```rust,ignore
struct Query {
    arena: Arena,
    request: Request,
    data: Vec<u8>    
}

fn client_do_work(query: &mut Query) {
    let request = rewrite_request(&query.request, &query.arena);
    get_in(request, &mut query.data);
}
```
