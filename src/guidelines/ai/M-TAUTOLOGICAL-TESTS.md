<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Tests do not assert ground truth (M-TAUTOLOGICAL-TESTS) { #M-TAUTOLOGICAL-TESTS }

<span class="ru">Тесты не проверяют эталонную истину (M-TAUTOLOGICAL-TESTS)</span>

<why>tests that add value, not noise.</why>

<span class="ru">тесты, которые приносят пользу, а не шум.</span>

Unit tests should verify meaningful behavior instead of repeating foundational definitions.

<span class="ru">Модульные тесты должны проверять осмысленное поведение, а не повторять базовые определения.</span>

Agents frequently produce tests that re-state the expected value from the same logic the code under test uses, or that simply mirror the implementation's branches. Such tests pass by construction, provide virtually no value, but increase the noise floor of subsequent changes:

<span class="ru">Агенты часто создают тесты, которые повторно задают ожидаемое значение логикой, которую использует тестируемый код, или просто повторяют ветвления реализации. Такие тесты проходят по построению, почти не дают ценности, но повышают фоновый шум при последующих изменениях:</span>

```rust
const CHECKPOINTS: [u32; 4] = [0, 90, 180, 270];

#[test]
fn checkpoints_are_correct() {
    assert_eq!(CHECKPOINTS, [0, 90, 180, 270]);
}
```

Where these are used to satisfy mutation tests, the mutation test should be skipped instead.

<span class="ru">Если такие тесты нужны для удовлетворения мутационных тестов, мутационный тест следует пропустить.</span>

Instead, a meaningful test would check a property the constants are supposed to satisfy, for example that they are evenly spaced, monotonically increasing, or impose some direction in related logic.

<span class="ru">Вместо этого осмысленный тест проверял бы свойство, которому должны удовлетворять константы, например, что они расположены с равным шагом, монотонно возрастают или задают направление в связанной логике.</span>
