<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Proc macros don't produce implied or hidden items (M-PROC-IMPLIED-ITEMS) { #M-PROC-IMPLIED-ITEMS }

<span class="ru">Процедурные макросы не создают неявных или скрытых элементов (M-PROC-IMPLIED-ITEMS)</span>

<why>clear errors and correct hygiene and visibility.</why>

<span class="ru">понятные ошибки, корректная гигиена имён и видимость.</span>

Macros should not define magic types on their own, in particular not public ones, or ones that don't rely on namespace tricks.

<span class="ru">Макросы не должны самостоятельно определять «магические» типы, в особенности публичные, или такие, которые не опираются на трюки с пространствами имён.</span>

Some macros want to define types, for example

<span class="ru">Некоторые макросы хотят определять типы, например:</span>

```rust,ignore
#[my_macro]
struct UserType;

// would expand to

struct UserType;
struct ExtraType; 
impl UserType {
    fn foo() -> ExtraType { ... };
}
```

This is almost always a bad idea for several reasons:

<span class="ru">Это почти всегда плохая идея по нескольким причинам:</span>

- they can conflict with existing user-defined types inside the same module,
  - <span class="ru">они могут конфликтовать с уже существующими пользовательскими типами в том же модуле,</span>
- if done naively, they can conflict with other expansions of the same macro,
  - <span class="ru">при наивной реализации они могут конфликтовать с другими раскрытиями того же макроса,</span>
- they can clash with the user's naming conventions,
  - <span class="ru">они могут противоречить принятым пользователем соглашениям об именовании,</span>
- they are invisible at source code level and easily forgotten to be re-exported where needed.
  - <span class="ru">они невидимы на уровне исходного кода и легко забываются при реэкспорте там, где это нужно.</span>

While it is possible for users to work around these limitations somewhat, these are paper cuts your users will have to deal with, possibly months after the fact when refactoring otherwise unrelated code.

<span class="ru">Пользователи могут до некоторой степени обойти эти ограничения, но это мелкие неудобства, с которыми им придётся мириться, возможно, спустя месяцы, когда они будут рефакторить совсем другой код.</span>

Note that there is one exception to this rule that has generally acceptable UX, the overloaded use of [namespaces](https://doc.rust-lang.org/reference/names/namespaces.html) made prominent by crates like Rocket:

<span class="ru">Обратите внимание: есть одно исключение из этого правила с приемлемым с точки зрения удобства поведением — перегруженное использование [пространств имён](https://doc.rust-lang.org/reference/names/namespaces.html), ставшее известным благодаря таким крейтам, как Rocket:</span>

```rust,ignore
#[my_macro]
fn foo() { ... }

// would expand to

fn foo() { ... }

struct foo;
impl SomeTrait for foo { ... }
```

Here a new type `foo` is introduced with the same name as the function `foo`. Due to Rust's namespace rules they can co-exist and are automatically re-exported with their parent, and due to [Rust's casing rules (C-CASE)](https://rust-lang.github.io/api-guidelines/naming.html#casing-conforms-to-rfc-430-c-case) these are highly unlikely to clash with user-defined types. However, they would still not make for a pretty _public_ type, and are therefore mainly used inside root crates to define request handlers or FFI functions.

<span class="ru">Здесь вводится новый тип `foo` с тем же именем, что и функция `foo`. Благодаря правилам пространств имён Rust они могут сосуществовать и автоматически реэкспортируются вместе с родителем, а благодаря [правилам регистра Rust (C-CASE)](https://rust-lang.github.io/api-guidelines/naming.html#casing-conforms-to-rfc-430-c-case) такие имена почти не конфликтуют с пользовательскими типами. Однако это всё равно не делает их красивым _публичным_ типом, поэтому такой приём в основном используется внутри корневых крейтов для определения обработчиков запросов или FFI-функций.</span>

> ### <tip></tip> Namespaces != Modules
>
> Namespaces in Rust have nothing to do with namespaces in other languages. A namespace in C# is approximately a module in Rust. A namespace in Rust
is an esoteric property of names (e.g., `fn foo`, `struct Bar {}`, `moo!`) that decides which 'naming bucket' it lives in inside a module.
>
> <span class="ru">Пространства имён (namespaces) в Rust не имеют ничего общего с пространствами имён в других языках. Namespace в C# примерно соответствует модулю в Rust. Namespace в Rust — это своеобразное свойство имён (например, `fn foo`, `struct Bar {}`, `moo!`), которое определяет, в какую «корзину имён» внутри модуля попадает имя.</span>
