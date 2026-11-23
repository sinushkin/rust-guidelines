<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Unsafe needs reason, should be avoided (M-UNSAFE) { #M-UNSAFE }

<span class="ru">Unsafe требует обоснования и его следует избегать (M-UNSAFE)</span>

<why>memory safety and a minimal attack surface.</why>

<span class="ru">безопасность памяти и минимальная поверхность атаки.</span>

You must have a valid reason to use `unsafe`. The only valid reasons are

<span class="ru">Вы обязаны иметь веское основание для использования `unsafe`. Допустимы только следующие основания:</span>

1) novel abstractions, e.g., a new smart pointer or allocator,
   - <span class="ru">новые абстракции, например, новый умный указатель или аллокатор,</span>
1) performance, e.g., attempting to call `.get_unchecked()`,
   - <span class="ru">производительность, например, попытка вызвать `.get_unchecked()`,</span>
1) FFI and platform calls, e.g., calling into C or the kernel, ...
   - <span class="ru">FFI и вызовы платформы, например, вызов C-кода или ядра, ...</span>

Unsafe code lowers the guardrails used by the compiler, transferring some of the compiler's responsibilities
to the programmer. Correctness of the resulting code relies primarily on catching all mistakes in code review,
which is error-prone. Mistakes in unsafe code may introduce high-severity security vulnerabilities.

<span class="ru">Небезопасный код снижает защитные барьеры, которыми пользуется компилятор, и переносит часть его обязанностей на программиста. Корректность итогового кода в первую очередь опирается на то, что все ошибки будут найдены при код-ревью, а это подвержено ошибкам. Ошибки в небезопасном коде могут привести к серьёзным уязвимостям безопасности.</span>

You must not use ad-hoc `unsafe` to

<span class="ru">Запрещено использовать `unsafe` ad-hoc, чтобы:</span>

- shorten a performant and safe Rust program, e.g., 'simplify' enum casts via `transmute`,
  - <span class="ru">сократить производительную и безопасную программу на Rust, например, «упростить» приведения enum через `transmute`,</span>
- bypass `Send` and similar bounds, e.g., by doing `unsafe impl Send ...`,
  - <span class="ru">обойти `Send` и аналогичные ограничения, например, через `unsafe impl Send ...`,</span>
- bypass lifetime requirements via `transmute` and similar.
  - <span class="ru">обойти требования к временам жизни через `transmute` и аналогичные средства.</span>

Ad-hoc here means `unsafe` embedded in otherwise unrelated code. It is of course permissible to create properly designed, sound abstractions doing these things.

<span class="ru">Ad-hoc здесь означает `unsafe`, встроенный в иначе не связанный с ним код. Разумеется, допустимо создавать правильно спроектированные, корректные абстракции, которые делают подобные вещи.</span>

In any case, `unsafe` must follow the guidelines outlined below.

<span class="ru">В любом случае `unsafe` должен следовать руководствам, изложенным ниже.</span>

### Novel Abstractions

<span class="ru">Новые абстракции</span>

- [ ] Verify there is no established alternative. If there is, prefer that.
  - <span class="ru">Проверьте, нет ли устоявшейся альтернативы. Если есть, предпочтите её.</span>
- [ ] Your abstraction must be minimal and testable.
  - <span class="ru">Ваша абстракция должна быть минимальной и тестируемой.</span>
- [ ] It must be hardened and tested against ["adversarial code"](https://cheats.rs/#adversarial-code), esp.
  - <span class="ru">Она должна быть укреплена и протестирована против [«adversarial code»](https://cheats.rs/#adversarial-code), в особенности:</span>
  - If they accept closures they must become invalid (e.g., poisoned) if the closure panics
    - <span class="ru">если они принимают замыкания, они должны становиться недействительными (например, «отравленными»), если замыкание паникует</span>
  - They must assume any safe trait is misbehaving, esp. `Deref`, `Clone` and `Drop`.
    - <span class="ru">они должны исходить из того, что любой безопасный трейт может вести себя некорректно, в особенности `Deref`, `Clone` и `Drop`.</span>
- [ ] Any use of `unsafe` must be accompanied by plain-text reasoning outlining its safety
  - <span class="ru">Любое использование `unsafe` должно сопровождаться текстовым обоснованием его безопасности</span>
- [ ] It must pass [Miri](https://github.com/rust-lang/miri), including adversarial test cases
  - <span class="ru">Она должна проходить [Miri](https://github.com/rust-lang/miri), включая адверсариальные тест-кейсы</span>
- [ ] It must follow all other [unsafe code guidelines](https://rust-lang.github.io/unsafe-code-guidelines/)
  - <span class="ru">Она должна следовать всем остальным [unsafe code guidelines](https://rust-lang.github.io/unsafe-code-guidelines/)</span>

### Performance

<span class="ru">Производительность</span>

- [ ] Using `unsafe` for performance reasons should only be done after benchmarking
  - <span class="ru">Использовать `unsafe` ради производительности следует только после замеров (бенчмарков).</span>
- [ ] Any use of `unsafe` must be accompanied by plain-text reasoning outlining its safety. This applies to both
  calling `unsafe` methods, as well as providing `_unchecked` ones.
  - <span class="ru">Любое использование `unsafe` должно сопровождаться текстовым обоснованием его безопасности. Это относится как к вызову `unsafe`-методов, так и к предоставлению `_unchecked`-методов.</span>
- [ ] The code in question must pass [Miri](https://github.com/rust-lang/miri)
  - <span class="ru">Соответствующий код должен проходить [Miri](https://github.com/rust-lang/miri)</span>
- [ ] You must follow the [unsafe code guidelines](https://rust-lang.github.io/unsafe-code-guidelines/)
  - <span class="ru">Вы обязаны следовать [unsafe code guidelines](https://rust-lang.github.io/unsafe-code-guidelines/)</span>

### FFI

<span class="ru">FFI</span>

- [ ] We recommend you use an established interop library to avoid `unsafe` constructs
  - <span class="ru">Рекомендуем использовать устоявшуюся библиотеку взаимодействия, чтобы избежать конструкций `unsafe`.</span>
- [ ] You must follow the [unsafe code guidelines](https://rust-lang.github.io/unsafe-code-guidelines/)
  - <span class="ru">Вы обязаны следовать [unsafe code guidelines](https://rust-lang.github.io/unsafe-code-guidelines/)</span>
- [ ] You must document your generated bindings to make it clear which call patterns are permissible
  - <span class="ru">Вы обязаны документировать сгенерированные привязки, чтобы было ясно, какие шаблоны вызовов допустимы.</span>

### Further Reading

<span class="ru">Дополнительная литература</span>

- [Nomicon](https://doc.rust-lang.org/nightly/nomicon/)
- [Unsafe Code Guidelines](https://rust-lang.github.io/unsafe-code-guidelines/)
- [Miri](https://github.com/rust-lang/miri)
- ["Adversarial code"](https://cheats.rs/#adversarial-code)
