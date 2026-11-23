<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Public types are Debug (M-PUBLIC-DEBUG) { #M-PUBLIC-DEBUG }

<span class="ru">Публичные типы реализуют Debug (M-PUBLIC-DEBUG)</span>

<why>easy debugging without leaking sensitive data.</why>

<span class="ru">простая отладка без утечки чувствительных данных.</span>

All public types exposed by a crate should implement `Debug`. Most types can do so via `#[derive(Debug)]`:

<span class="ru">Все публичные типы, которые экспортирует крейт, должны реализовывать `Debug`. Большинство типов могут сделать это через `#[derive(Debug)]`:</span>

```rust
#[derive(Debug)]
struct Endpoint(String);
```

Types designed to hold sensitive data should also implement `Debug`, but do so via a custom implementation.
This implementation must employ unit tests to ensure sensitive data isn't actually leaked, and will not be in the future.

<span class="ru">Типы, предназначенные для хранения чувствительных данных, тоже должны реализовывать `Debug`, но делать это через собственную реализацию. Эта реализация обязана использовать модульные тесты, чтобы гарантировать, что чувствительные данные не утекают и не будут утекать в будущем.</span>

```rust
use std::fmt::{Debug, Formatter};

struct UserSecret(String);

impl Debug for UserSecret {
    fn fmt(&self, f: &mut Formatter<'_>) -> std::fmt::Result {
        write!(f, "UserSecret(...)")
    }
}

#[test]
fn test() {
    let key = "552d3454-d0d5-445d-ab9f-ef2ae3a8896a";
    let secret = UserSecret(key.to_string());
    let rendered = format!("{:?}", secret);

    assert!(rendered.contains("UserSecret"));
    assert!(!rendered.contains(key));
}
```
