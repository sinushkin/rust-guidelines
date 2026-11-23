<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Complex type initialization hierarchies are cascaded (M-INIT-CASCADED) { #M-INIT-CASCADED }

<span class="ru">Иерархии инициализации сложных типов строятся каскадом (M-INIT-CASCADED)</span>

<why>construction free of parameter mix-ups.</why>

<span class="ru">создание без путаницы в параметрах.</span>

Types that require 4+ parameters should cascade their initialization via helper types.

<span class="ru">Типы, которым нужны 4 и более параметров, должны каскадно инициализироваться через вспомогательные типы.</span>

```rust, ignore
# struct Deposit;
impl Deposit {
    // Easy to confuse parameters and signature generally unwieldy.
    pub fn new(bank_name: &str, customer_name: &str, currency_name: &str, currency_amount: u64) -> Self { }
}
```

Instead of providing a long parameter list, parameters should be grouped semantically. When applying this guideline,
also check if [C-NEWTYPE] is applicable:

<span class="ru">Вместо длинного списка параметров их следует группировать семантически. Применяя это руководство, также проверьте, применим ли [C-NEWTYPE]:</span>

```rust, ignore
# struct Deposit;
# struct Account;
# struct Currency
impl Deposit {
    // Better, signature cleaner
    pub fn new(account: Account, amount: Currency) -> Self { }
}

impl Account {
    pub fn new_ok(bank: &str, customer: &str) -> Self { }
    pub fn new_even_better(bank: Bank, customer: Customer) -> Self { }
}
```

[C-NEWTYPE]: https://rust-lang.github.io/api-guidelines/type-safety.html#c-newtype
