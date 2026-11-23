<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

# Checklist

<span class="ru">Чек-лист</span>

- **Universal**
  - [ ] Follow the upstream guidelines ([M-UPSTREAM-GUIDELINES])
    - <span class="ru">Следуйте вышестоящим руководствам</span>
  - [ ] Use static verification ([M-STATIC-VERIFICATION])
    - <span class="ru">Используйте статическую верификацию</span>
  - [ ] Lint overrides should use `#[expect]` ([M-LINT-OVERRIDE-EXPECT])
    - <span class="ru">Переопределения линтов используют `#[expect]`</span>
  - [ ] Public types are Debug ([M-PUBLIC-DEBUG])
    - <span class="ru">Публичные типы реализуют Debug</span>
  - [ ] Public types meant to be read are Display ([M-PUBLIC-DISPLAY])
    - <span class="ru">Публичные типы, предназначенные для чтения, реализуют Display</span>
  - [ ] If in doubt, split the crate ([M-SMALLER-CRATES])
    - <span class="ru">В сомнениях разделяйте крейт</span>
  - [ ] Names are free of weasel words ([M-WEASEL-WORDS])
    - <span class="ru">В именах нет расплывчатых слов</span>
  - [ ] Names of items are short ([M-SHORT-NAMES])
    - <span class="ru">Имена элементов короткие</span>
  - [ ] Prefer regular over associated functions ([M-REGULAR-FN])
    - <span class="ru">Предпочитайте обычные функции ассоциированным</span>
  - [ ] Magic values are documented ([M-DOCUMENTED-MAGIC])
    - <span class="ru">Магические значения задокументированы</span>
  - [ ] Use structured logging with message templates ([M-LOG-STRUCTURED])
    - <span class="ru">Используйте структурированное логирование с шаблонами сообщений</span>
- **Library / Interoperability**
  - [ ] Types are Send ([M-TYPES-SEND])
    - <span class="ru">Типы реализуют Send</span>
  - [ ] Native escape hatches ([M-ESCAPE-HATCHES])
    - <span class="ru">Нативные «запасные выходы»</span>
  - [ ] Don't leak external types ([M-DONT-LEAK-TYPES])
    - <span class="ru">Не раскрывайте внешние типы</span>
  - [ ] Items come from their original crate ([M-FOREIGN-REEXPORTS])
    - <span class="ru">Элементы берутся из исходного крейта</span>
  - [ ] Accept `impl AsRef<>` where feasible ([M-IMPL-ASREF])
    - <span class="ru">По возможности принимайте `impl AsRef<>`</span>
  - [ ] Accept `impl RangeBounds<>` where feasible ([M-IMPL-RANGEBOUNDS])
    - <span class="ru">По возможности принимайте `impl RangeBounds<>`</span>
  - [ ] Accept `impl 'IO'` where feasible ('sans IO') ([M-IMPL-IO])
    - <span class="ru">По возможности принимайте `impl 'IO'` («без IO»)</span>
- **Library / UX**
  - [ ] Abstractions don't visibly nest ([M-SIMPLE-ABSTRACTIONS])
    - <span class="ru">Абстракции не вложены видимым образом</span>
  - [ ] Avoid smart pointers and wrappers in APIs ([M-AVOID-WRAPPERS])
    - <span class="ru">Избегайте умных указателей и обёрток в API</span>
  - [ ] Prefer types over generics, generics over dyn traits ([M-DI-HIERARCHY])
    - <span class="ru">Предпочитайте типы обобщениям, а обобщения — трейт-объектам `dyn`</span>
  - [ ] Errors are canonical structs ([M-ERRORS-CANONICAL-STRUCTS])
    - <span class="ru">Ошибки — канонические структуры</span>
  - [ ] Canonical error conversion uses `From`, not `map_err` ([M-FROM-ERROR])
    - <span class="ru">Каноническое преобразование ошибок использует `From`, а не `map_err`</span>
  - [ ] Complex type construction has builders ([M-INIT-BUILDER])
    - <span class="ru">Для сложного создания типов есть билдеры</span>
  - [ ] Complex type initialization hierarchies are cascaded ([M-INIT-CASCADED])
    - <span class="ru">Иерархии инициализации сложных типов каскадные</span>
  - [ ] Services are Clone ([M-SERVICES-CLONE])
    - <span class="ru">Сервисы реализуют Clone</span>
  - [ ] Essential functionality should be inherent ([M-ESSENTIAL-FN-INHERENT])
    - <span class="ru">Основная функциональность реализована как inherent-методы</span>
  - [ ] Modules are balanced in size and scope ([M-BALANCED-MODULES])
    - <span class="ru">Модули сбалансированы по размеру и области ответственности</span>
  - [ ] Don't define preludes ([M-NO-PRELUDE])
    - <span class="ru">Не определяйте preludes</span>
  - [ ] Parameter ordering is consistent ([M-PARAMETER-CONSISTENCY])
    - <span class="ru">Порядок параметров единообразен</span>
  - [ ] Collections implement the appropriate iter traits ([M-COLLECTION-TRAITS])
    - <span class="ru">Коллекции реализуют соответствующие трейты итераторов</span>
  - [ ] Functions are `async` over returning a Future ([M-ASYNC-FN])
    - <span class="ru">Функции объявляются как `async`, а не возвращают Future</span>
- **Library / Resilience**
  - [ ] I/O and system calls are mockable ([M-MOCKABLE-SYSCALLS])
    - <span class="ru">Ввод-вывод и системные вызовы можно подменять (mock)</span>
  - [ ] Test utilities are feature gated ([M-TEST-UTIL])
    - <span class="ru">Тестовые утилиты включаются через feature-флаги</span>
  - [ ] Integration tests live under `tests/` ([M-INTEGRATION-TESTS])
    - <span class="ru">Интеграционные тесты находятся в `tests/`</span>
  - [ ] Use the proper type family ([M-STRONG-TYPES])
    - <span class="ru">Используйте подходящее семейство типов</span>
  - [ ] Newtypes guard their invariants ([M-STRONG-TYPES-GUARD])
    - <span class="ru">Newtype охраняют свои инварианты</span>
  - [ ] Builders validate in final `.build()` ([M-BUILD-RESULT])
    - <span class="ru">Билдеры проверяют данные в финальном `.build()`</span>
  - [ ] Don't glob re-export items ([M-NO-GLOB-REEXPORTS])
    - <span class="ru">Не реэкспортируйте элементы через glob</span>
  - [ ] Avoid statics ([M-AVOID-STATICS])
    - <span class="ru">Избегайте статических переменных</span>
  - [ ] Production code uses telemetry, not println ([M-LOG-NOT-PRINT])
    - <span class="ru">Продакшн-код использует телеметрию, а не println</span>
- **Library / Building**
  - [ ] Libraries work out of the box ([M-OOBE])
    - <span class="ru">Библиотеки работают сразу после подключения</span>
  - [ ] Native `-sys` crates compile without dependencies ([M-SYS-CRATES])
    - <span class="ru">Нативные `-sys` крейты собираются без зависимостей</span>
  - [ ] Features are additive ([M-FEATURES-ADDITIVE])
    - <span class="ru">Фичи аддитивны</span>
- **Macros**
  - [ ] Macros are a last resort ([M-MACRO-LAST-RESORT])
    - <span class="ru">Макросы — крайнее средство</span>
  - [ ] Prefer 'macros by example' over proc macros ([M-EXAMPLE-OVER-PROC])
    - <span class="ru">Предпочитайте 'macros by example' процедурным макросам</span>
  - [ ] Macros don't lie about signatures ([M-MACROS-DONT-LIE])
    - <span class="ru">Макросы не лгут о сигнатурах</span>
  - [ ] Macros assume main crate ([M-MACRO-MAIN-CRATE])
    - <span class="ru">Макросы предполагают основной крейт</span>
  - [ ] Pin supporting proc macro crates ([M-MACRO-VERSION-PIN])
    - <span class="ru">Вспомогательные крейты процедурных макросов зафиксированы по версии</span>
  - [ ] Third party items come from hidden `_private` module ([M-MACRO-HELPERS])
    - <span class="ru">Элементы сторонних крейтов берутся из скрытого модуля `_private`</span>
  - [ ] Proc macros should have separate impl crate incl. tests ([M-PROC-IMPL])
    - <span class="ru">Процедурные макросы имеют отдельный крейт реализации с тестами</span>
  - [ ] Proc macros don't produce implied or hidden items ([M-PROC-IMPLIED-ITEMS])
    - <span class="ru">Процедурные макросы не создают неявных или скрытых элементов</span>
- **Applications**
  - [ ] Use mimalloc for apps ([M-MIMALLOC-APPS])
    - <span class="ru">В приложениях используйте mimalloc</span>
  - [ ] Applications may use Anyhow or derivatives ([M-APP-ERROR])
    - <span class="ru">Приложения могут использовать Anyhow или его производные</span>
  - [ ] Applications target highest viable target-cpu ([M-TARGET-CPU])
    - <span class="ru">Приложения нацелены на наибольший подходящий target-cpu</span>
- **FFI**
  - [ ] Isolate DLL state between FFI libraries ([M-ISOLATE-DLL-STATE])
    - <span class="ru">Изолируйте состояние DLL между FFI-библиотеками</span>
  - [ ] Business logic belongs in core crates, FFI only translates ([M-FFI-TRANSLATES])
    - <span class="ru">Бизнес-логика — в основных крейтах, FFI только транслирует</span>
  - [ ] FFI crates follow established naming conventions ([M-FFI-NAMING])
    - <span class="ru">FFI-крейты следуют устоявшимся соглашениям об именовании</span>
- **Correctness**
  - [ ] Unsafe needs reason, should be avoided ([M-UNSAFE])
    - <span class="ru">Unsafe требует обоснования и его следует избегать</span>
  - [ ] Unsafe implies undefined behavior ([M-UNSAFE-IMPLIES-UB])
    - <span class="ru">Unsafe подразумевает неопределённое поведение</span>
  - [ ] All code must be sound ([M-UNSOUND])
    - <span class="ru">Весь код должен быть корректным (sound)</span>
  - [ ] Panic means 'stop the program' ([M-PANIC-IS-STOP])
    - <span class="ru">Паника означает «остановку программы»</span>
  - [ ] Detected programming bugs are panics, not errors ([M-PANIC-ON-BUG])
    - <span class="ru">Ошибки программирования обнаруживаются паниками, а не ошибками</span>
  - [ ] Panic continuation is last resort ([M-PANIC-CONTINUATION])
    - <span class="ru">Продолжение после паники — крайнее средство</span>
  - [ ] Custom panics have a helpful message ([M-PANIC-MESSAGE])
    - <span class="ru">Пользовательские паники содержат понятное сообщение</span>
- **Performance**
  - [ ] Optimize for throughput, avoid empty cycles ([M-THROUGHPUT])
    - <span class="ru">Оптимизируйте пропускную способность, избегайте пустых циклов</span>
  - [ ] Identify, profile, optimize the hot path early ([M-HOTPATH])
    - <span class="ru">Рано выявляйте, профилируйте и оптимизируйте горячий путь</span>
  - [ ] Long-running tasks should have yield points ([M-YIELD-POINTS])
    - <span class="ru">Долгие задачи имеют точки передачи управления (yield)</span>
  - [ ] Reuse allocations where possible ([M-MEM-REUSE])
    - <span class="ru">По возможности переиспользуйте аллокации</span>
  - [ ] Library telemetry does not tank performance ([M-LOG-OVERHEAD])
    - <span class="ru">Телеметрия библиотек не снижает производительность</span>
  - [ ] Nested type hierarchies should avoid needless indirection ([M-AVOID-INDIRECTION])
    - <span class="ru">Вложенные иерархии типов избегают лишней косвенности</span>
  - [ ] Use boxed slices and strings for immutable owned sequences ([M-BOX-DST])
    - <span class="ru">Для неизменяемых владеющих последовательностей используйте boxed-срезы и строки</span>
  - [ ] Shrink collections to fit after building ([M-SHRINK-TO-FIT])
    - <span class="ru">После построения коллекции уменьшаются до нужного размера</span>
  - [ ] Use a fast hasher where possible ([M-FAST-HASHER])
    - <span class="ru">По возможности используйте быстрый хешер</span>
  - [ ] Collections are created with sufficient initial capacity ([M-INITIAL-CAPACITY])
    - <span class="ru">Коллекции создаются с достаточной начальной ёмкостью</span>
  - [ ] Hot `async` functions reduce stack size ([M-ASYNC-STACK-SIZE])
    - <span class="ru">Горячие `async`-функции уменьшают размер стека</span>
- **Project**
  - [ ] Common settings come from the workspace Cargo.toml ([M-CARGO-WORKSPACE])
    - <span class="ru">Общие настройки берутся из Cargo.toml workspace</span>
  - [ ] The workspace lists and versions all crates ([M-CRATES-IN-WORKSPACE])
    - <span class="ru">Workspace перечисляет и версионирует все крейты</span>
  - [ ] All crates are siblings in one folder ([M-CRATES-FLAT-FOLDER])
    - <span class="ru">Все крейты — соседи в одной папке</span>
  - [ ] New crates target latest edition ([M-LATEST-EDITION])
    - <span class="ru">Новые крейты используют последнюю edition</span>
  - [ ] MSRV is conservatively updated ([M-MSRV])
    - <span class="ru">MSRV обновляется консервативно</span>
- **Documentation**
  - [ ] First sentence is one line; approx. 15 words ([M-FIRST-DOC-SENTENCE])
    - <span class="ru">Первое предложение умещается в одну строку, около 15 слов</span>
  - [ ] Has comprehensive module documentation ([M-MODULE-DOCS])
    - <span class="ru">Есть полная документация модуля</span>
  - [ ] Documentation has canonical sections ([M-CANONICAL-DOCS])
    - <span class="ru">Документация имеет канонические разделы</span>
  - [ ] Mark `pub use` items with `#[doc(inline)]` ([M-DOC-INLINE])
    - <span class="ru">Элементы `pub use` помечены атрибутом `#[doc(inline)]`</span>
- **AI**
  - [ ] Design with AI use in mind ([M-DESIGN-FOR-AI])
    - <span class="ru">Проектирование с учётом использования ИИ</span>
  - [ ] Items are only visible through one path ([M-SINGLE-ITEM-PATH])
    - <span class="ru">Элементы видны только через один путь</span>
  - [ ] Avoid meta design documentation ([M-NO-META-DESIGN-DOCUMENTATION])
    - <span class="ru">Избегайте мета-документации по проектированию</span>
  - [ ] Tests do not assert ground truth ([M-TAUTOLOGICAL-TESTS])
    - <span class="ru">Тесты не проверяют эталонную истину (ground truth)</span>
  - [ ] Rust code solves Rust problems ([M-RUST-SHAPED])
    - <span class="ru">Rust-код решает задачи Rust</span>

<!-- Universal  -->
[M-UPSTREAM-GUIDELINES]: ../universal/#M-UPSTREAM-GUIDELINES
[M-STATIC-VERIFICATION]: ../universal/#M-STATIC-VERIFICATION
[M-LINT-OVERRIDE-EXPECT]: ../universal/#M-LINT-OVERRIDE-EXPECT
[M-PUBLIC-DEBUG]: ../universal/#M-PUBLIC-DEBUG
[M-PUBLIC-DISPLAY]: ../universal/#M-PUBLIC-DISPLAY
[M-SMALLER-CRATES]: ../universal/#M-SMALLER-CRATES
[M-WEASEL-WORDS]: ../universal/#M-WEASEL-WORDS
[M-SHORT-NAMES]: ../universal/#M-SHORT-NAMES
[M-REGULAR-FN]: ../universal/#M-REGULAR-FN
[M-DOCUMENTED-MAGIC]: ../universal/#M-DOCUMENTED-MAGIC
[M-LOG-STRUCTURED]: ../universal/#M-LOG-STRUCTURED
[M-LOG-NOT-PRINT]: ../libs/resilience/#M-LOG-NOT-PRINT

<!-- Libs -->
[M-TYPES-SEND]: ../libs/interop/#M-TYPES-SEND
[M-DONT-LEAK-TYPES]: ../libs/interop/#M-DONT-LEAK-TYPES
[M-FOREIGN-REEXPORTS]: ../libs/interop/#M-FOREIGN-REEXPORTS
[M-ESCAPE-HATCHES]: ../libs/interop/#M-ESCAPE-HATCHES
[M-STRONG-TYPES]: ../libs/resilience/#M-STRONG-TYPES
[M-STRONG-TYPES-GUARD]: ../libs/resilience/#M-STRONG-TYPES-GUARD
[M-NO-GLOB-REEXPORTS]: ../libs/resilience/#M-NO-GLOB-REEXPORTS
[M-ESSENTIAL-FN-INHERENT]: ../libs/ux/#M-ESSENTIAL-FN-INHERENT
[M-MOCKABLE-SYSCALLS]: ../libs/resilience/#M-MOCKABLE-SYSCALLS
[M-SIMPLE-ABSTRACTIONS]: ../libs/ux/#M-SIMPLE-ABSTRACTIONS
[M-AVOID-WRAPPERS]: ../libs/ux/#M-AVOID-WRAPPERS
[M-DI-HIERARCHY]: ../libs/ux/#M-DI-HIERARCHY
[M-ERRORS-CANONICAL-STRUCTS]: ../libs/ux/#M-ERRORS-CANONICAL-STRUCTS
[M-FROM-ERROR]: ../libs/ux/#M-FROM-ERROR
[M-INIT-BUILDER]: ../libs/ux/#M-INIT-BUILDER
[M-BUILD-RESULT]: ../libs/resilience/#M-BUILD-RESULT
[M-INIT-CASCADED]: ../libs/ux/#M-INIT-CASCADED
[M-SERVICES-CLONE]: ../libs/ux/#M-SERVICES-CLONE
[M-IMPL-ASREF]: ../libs/interop/#M-IMPL-ASREF
[M-IMPL-RANGEBOUNDS]: ../libs/interop/#M-IMPL-RANGEBOUNDS
[M-IMPL-IO]: ../libs/interop/#M-IMPL-IO
[M-BALANCED-MODULES]: ../libs/ux/#M-BALANCED-MODULES
[M-NO-PRELUDE]: ../libs/ux/#M-NO-PRELUDE
[M-PARAMETER-CONSISTENCY]: ../libs/ux/#M-PARAMETER-CONSISTENCY
[M-COLLECTION-TRAITS]: ../libs/ux/#M-COLLECTION-TRAITS
[M-ASYNC-FN]: ../libs/ux/#M-ASYNC-FN
[M-TEST-UTIL]: ../libs/resilience/#M-TEST-UTIL
[M-INTEGRATION-TESTS]: ../libs/resilience/#M-INTEGRATION-TESTS
[M-AVOID-STATICS]: ../libs/resilience/#M-AVOID-STATICS
[M-OOBE]: ../libs/building/#M-OOBE
[M-SYS-CRATES]: ../libs/resilience/#M-SYS-CRATES
[M-FEATURES-ADDITIVE]: ../libs/building/#M-FEATURES-ADDITIVE

<!-- Apps -->
[M-APP-ERROR]: ../apps/#M-APP-ERROR
[M-MIMALLOC-APPS]: ../apps/#M-MIMALLOC-APPS
[M-TARGET-CPU]: ../apps/#M-TARGET-CPU

<!-- FFI -->
[M-ISOLATE-DLL-STATE]: ../ffi/#M-ISOLATE-DLL-STATE
[M-FFI-TRANSLATES]: ../ffi/#M-FFI-TRANSLATES
[M-FFI-NAMING]: ../ffi/#M-FFI-NAMING

<!-- Correctness -->
[M-UNSAFE]: ../correctness/#M-UNSAFE
[M-UNSAFE-IMPLIES-UB]: ../correctness/#M-UNSAFE-IMPLIES-UB
[M-UNSOUND]: ../correctness/#M-UNSOUND
[M-PANIC-IS-STOP]: ../correctness/#M-PANIC-IS-STOP
[M-PANIC-ON-BUG]: ../correctness/#M-PANIC-ON-BUG
[M-PANIC-CONTINUATION]: ../correctness/#M-PANIC-CONTINUATION
[M-PANIC-MESSAGE]: ../correctness/#M-PANIC-MESSAGE

<!-- Performance -->
[M-HOTPATH]: ../performance/#M-HOTPATH
[M-THROUGHPUT]: ../performance/#M-THROUGHPUT
[M-YIELD-POINTS]: ../performance/#M-YIELD-POINTS
[M-MEM-REUSE]: ../performance/#M-MEM-REUSE
[M-LOG-OVERHEAD]: ../performance/#M-LOG-OVERHEAD
[M-AVOID-INDIRECTION]: ../performance/#M-AVOID-INDIRECTION
[M-BOX-DST]: ../performance/#M-BOX-DST
[M-SHRINK-TO-FIT]: ../performance/#M-SHRINK-TO-FIT
[M-FAST-HASHER]: ../performance/#M-FAST-HASHER
[M-INITIAL-CAPACITY]: ../performance/#M-INITIAL-CAPACITY
[M-ASYNC-STACK-SIZE]: ../performance/#M-ASYNC-STACK-SIZE

<!-- Project -->
[M-CARGO-WORKSPACE]: ../project/#M-CARGO-WORKSPACE
[M-CRATES-IN-WORKSPACE]: ../project/#M-CRATES-IN-WORKSPACE
[M-CRATES-FLAT-FOLDER]: ../project/#M-CRATES-FLAT-FOLDER
[M-LATEST-EDITION]: ../project/#M-LATEST-EDITION
[M-MSRV]: ../project/#M-MSRV

<!-- Docs -->
[M-FIRST-DOC-SENTENCE]: ../docs/#M-FIRST-DOC-SENTENCE
[M-MODULE-DOCS]: ../docs/#M-MODULE-DOCS
[M-CANONICAL-DOCS]: ../docs/#M-CANONICAL-DOCS
[M-DOC-INLINE]: ../docs/#M-DOC-INLINE

<!-- Macros -->
[M-MACRO-LAST-RESORT]: ../macros/#M-MACRO-LAST-RESORT
[M-EXAMPLE-OVER-PROC]: ../macros/#M-EXAMPLE-OVER-PROC
[M-MACROS-DONT-LIE]: ../macros/#M-MACROS-DONT-LIE
[M-MACRO-MAIN-CRATE]: ../macros/#M-MACRO-MAIN-CRATE
[M-MACRO-VERSION-PIN]: ../macros/#M-MACRO-VERSION-PIN
[M-MACRO-HELPERS]: ../macros/#M-MACRO-HELPERS
[M-PROC-IMPL]: ../macros/#M-PROC-IMPL
[M-PROC-IMPLIED-ITEMS]: ../macros/#M-PROC-IMPLIED-ITEMS

<!-- AI -->
[M-DESIGN-FOR-AI]: ../ai/#M-DESIGN-FOR-AI
[M-SINGLE-ITEM-PATH]: ../ai/#M-SINGLE-ITEM-PATH
[M-NO-META-DESIGN-DOCUMENTATION]: ../ai/#M-NO-META-DESIGN-DOCUMENTATION
[M-TAUTOLOGICAL-TESTS]: ../ai/#M-TAUTOLOGICAL-TESTS
[M-RUST-SHAPED]: ../ai/#M-RUST-SHAPED
