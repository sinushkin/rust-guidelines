<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Names are free of weasel words (M-WEASEL-WORDS) { #M-WEASEL-WORDS }

<span class="ru">Имена свободны от слов-пустышек (M-WEASEL-WORDS)</span>

<why>readability.</why>

<span class="ru">читаемость.</span>

Symbol names, especially type and trait names, should be free of weasel words that do not meaningfully
add information. Common offenders include `Service`, `Manager`, and `Factory`.

<span class="ru">Имена символов, особенно типов и трейтов, должны быть свободны от слов-пустышек, которые не добавляют существенной информации. Типичные примеры: `Service`, `Manager` и `Factory`.</span>

While your library may very well contain or communicate with a booking service&mdash;or even hold an `HttpClient`
instance named `booking_service`&mdash;one should rarely encounter a `BookingService` _type_ in code.

<span class="ru">Ваша библиотека вполне может содержать службу бронирования или взаимодействовать с ней&mdash;и даже хранить `HttpClient` в переменной `booking_service`&mdash;но тип `BookingService` в коде встречается редко.</span>

An item handling many bookings can just be called `Bookings`. If it does anything more specific, then that quality
should be appended instead. It submits these items elsewhere? Calling it `BookingDispatcher` would be more helpful.

<span class="ru">Элемент, обрабатывающий множество бронирований, можно просто назвать `Bookings`. Если он делает что-то более конкретное, то вместо этого следует добавить это качество к имени. Если он отправляет эти элементы куда-то ещё, называть его `BookingDispatcher` будет полезнее.</span>

The same is true for `Manager`s. All code manages _something_, so that moniker is rarely useful. With rare
exceptions, life cycle issues should likewise not be made the subject of some manager. Items are created in whatever
way they are needed, their disposal is governed by `Drop`, and only `Drop`.

<span class="ru">То же касается `Manager`. Весь код что-то управляет, поэтому такое название редко полезно. За редкими исключениями вопросы жизненного цикла тоже не должны становиться предметом какого-либо менеджера. Элементы создаются так, как это нужно, а их уничтожение определяется `Drop` и только `Drop`.</span>

Regarding factories, at least the term should be avoided. While the concept `FooFactory` has its use, its canonical
Rust name is `Builder` (compare [M-INIT-BUILDER](../libs/ux/#M-INIT-BUILDER)). A builder that can produce items repeatedly is still a builder.

<span class="ru">Что касается фабрик, по крайней мере этот термин следует избегать. Хотя концепция `FooFactory` имеет смысл, её каноническое имя в Rust — `Builder` (сравните [M-INIT-BUILDER](../libs/ux/#M-INIT-BUILDER)). Билдер, который может многократно создавать элементы, по-прежнему остаётся билдером.</span>

In addition, accepting factories (builders) as parameters is an unidiomatic import of OO concepts into Rust. If
repeatable instantiation is required, functions should ask for an `impl Fn() -> Foo` over a `FooBuilder` or
similar. In contrast, standalone builders have their use, but primarily to reduce parametric permutation complexity
around optional values (again, [M-INIT-BUILDER](../libs/ux/#M-INIT-BUILDER)).

<span class="ru">Кроме того, принимать фабрики (билдеры) как параметры — неидиоматичный перенос концепций ООП в Rust. Если требуется многократное создание экземпляров, функции должны принимать `impl Fn() -> Foo`, а не `FooBuilder` или что-то подобное. В отличие от этого, самостоятельные билдеры полезны, но в первую очередь для снижения сложности перебора параметров при необязательных значениях (опять же, см. [M-INIT-BUILDER](../libs/ux/#M-INIT-BUILDER)).</span>
