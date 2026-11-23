<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## If in doubt, split the crate (M-SMALLER-CRATES) { #M-SMALLER-CRATES }

<span class="ru">Если сомневаетесь — разделите крейт (M-SMALLER-CRATES)</span>

<why>fast compile times and good modularity.</why>

<span class="ru">быстрая компиляция и хорошая модульность.</span>

You should err on the side of having too many crates rather than too few, as this leads to dramatic compile time improvements—especially
during the development of these crates—and prevents cyclic component dependencies.

<span class="ru">Лучше иметь слишком много крейтов, чем слишком мало: это приводит к заметному ускорению компиляции — особенно во время разработки этих крейтов — и предотвращает циклические зависимости компонентов.</span>

Essentially, if a submodule can be used independently, its contents should be moved into a separate crate.

<span class="ru">По сути, если подмодуль можно использовать независимо, его содержимое следует вынести в отдельный крейт.</span>

Performing this crate split may cause you to lose access to some `pub(crate)` fields or methods. In many situations, this is a desirable
side-effect and should prompt you to design more flexible abstractions that would give your users similar affordances.

<span class="ru">Разделение крейта может лишить вас доступа к некоторым полям и методам `pub(crate)`. Во многих случаях это желательный побочный эффект: он должен побудить вас спроектировать более гибкие абстракции, которые дают вашим пользователям схожие возможности.</span>

In some cases, it is desirable to re-join individual crates back into a single _umbrella crate_, such as when dealing with proc macros, or runtimes.
Functionality split for technical reasons (e.g., a `foo_proc` proc macro crate) should always be re-exported. Otherwise, re-exports should be used sparingly.

<span class="ru">В некоторых случаях желательно объединить отдельные крейты обратно в один _зонтичный крейт_, например при работе с процедурными макросами или рантаймами. Функциональность, разделённая по техническим причинам (например, процедурный макрос-крейт `foo_proc`), всегда должна реэкспортироваться. В остальных случаях реэкспорты следует использовать экономно.</span>

> ### <tip></tip> Features vs. Crates
>
> <span class="ru">Features и крейты</span>
>
> As a rule of thumb, crates are for items that can reasonably be used on their own. Features should unlock extra functionality that
> can't live on its own. In the case of umbrella crates, see below, features may also be used to enable constituents (but then that functionality
> was extracted into crates already).
>
> <span class="ru">Как правило, крейты — для элементов, которые разумно использовать самостоятельно. Features должны открывать дополнительную функциональность, которая не может существовать сама по себе. В случае зонтичных крейтов (см. ниже) features также могут включать составные части, но тогда эта функциональность уже должна быть вынесена в крейты.</span>
>
> For example, if you defined a `web` crate with the following modules, users only needing client calls would also have to pay for the compilation of server code:
>
> <span class="ru">Например, если бы вы определили крейт `web` с приведёнными ниже модулями, пользователям, которым нужны только клиентские вызовы, пришлось бы тоже платить за компиляцию серверного кода:</span>
>
> ```text
> web::server
> web::client
> web::protocols
> ```
>
> Instead, you should introduce individual crates that give users the ability to pick and choose:
>
> <span class="ru">Вместо этого следует выделить отдельные крейты, которые дают пользователям возможность выбирать нужное:</span>
>
> ```text
> web_server
> web_client
> web_protocols
> ```
