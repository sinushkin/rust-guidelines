<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Follow the upstream guidelines (M-UPSTREAM-GUIDELINES) { #M-UPSTREAM-GUIDELINES }

<span class="ru">Следуйте существующим руководствам сообщества (M-UPSTREAM-GUIDELINES)</span>

<why>a codebase that reflects community lessons and does not surprise users or contributors.</why>

<span class="ru">кодовая база, которая отражает уроки сообщества и не удивляет пользователей и контрибьюторов.</span>

The guidelines in this book complement existing Rust guidelines, in particular:

<span class="ru">Руководства в этой книге дополняют существующие руководства по Rust, в частности:</span>

- [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/checklist.html)
- [Rust Style Guide](https://doc.rust-lang.org/nightly/style-guide/)
- [Rust Design Patterns](https://rust-unofficial.github.io/patterns//intro.html)
- [Rust Reference - Undefined Behavior](https://doc.rust-lang.org/reference/behavior-considered-undefined.html)

We recommend you read through these as well, and apply them in addition to this book's items. Pay special attention to the ones below, as they are frequently forgotten:

<span class="ru">Рекомендуем также изучить их и применять наряду с пунктами этой книги. Особое внимание уделите приведённым ниже пунктам, поскольку о них часто забывают:</span>

- [ ] [C-CONV](https://rust-lang.github.io/api-guidelines/naming.html#ad-hoc-conversions-follow-as_-to_-into_-conventions-c-conv) - Ad-hoc conversions
  follow  `as_`, `to_`, `into_` conventions
  - <span class="ru">Ad-hoc-преобразования следуют соглашениям `as_`, `to_`, `into_`</span>
- [ ] [C-GETTER](https://rust-lang.github.io/api-guidelines/naming.html#getter-names-follow-rust-convention-c-getter) - Getter names follow Rust convention
  - <span class="ru">Имена геттеров следуют соглашению Rust</span>
- [ ] [C-COMMON-TRAITS](https://rust-lang.github.io/api-guidelines/interoperability.html#c-common-traits) - Types eagerly implement common traits
  - <span class="ru">Типы заранее реализуют распространённые трейты</span>
  - `Copy`, `Clone`, `Eq`, `PartialEq`, `Ord`, `PartialOrd`, `Hash`, `Default`, `Debug`
  - `Display` where type wants to be displayed
    - <span class="ru">`Display`, если тип должен отображаться</span>
- [ ] [C-CTOR](https://rust-lang.github.io/api-guidelines/predictability.html?highlight=new#constructors-are-static-inherent-methods-c-ctor) -
  Constructors are static, inherent methods
  - <span class="ru">Конструкторы — статические методы, принадлежащие типу</span>
  - In particular, have `Foo::new()`, even if you have `Foo::default()`
    - <span class="ru">В частности, имейте `Foo::new()`, даже если есть `Foo::default()`</span>
- [ ] [C-FEATURE](https://rust-lang.github.io/api-guidelines/naming.html#feature-names-are-free-of-placeholder-words-c-feature) - Feature names
  are free of placeholder words
  - <span class="ru">Имена features не содержат слов-заполнителей</span>
