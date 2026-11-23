<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Essential functionality should be inherent (M-ESSENTIAL-FN-INHERENT) { #M-ESSENTIAL-FN-INHERENT }

<span class="ru">Ключевая функциональность должна быть собственной (inherent) (M-ESSENTIAL-FN-INHERENT)</span>

<why>easily discoverable essential functionality.</why>

<span class="ru">легко обнаруживаемая ключевая функциональность.</span>

Types should implement core functionality inherently. Trait implementations should forward to inherent functions, and not replace them. Instead of this

<span class="ru">Типы должны реализовывать основную функциональность как собственную (inherent). Реализации трейтов должны перенаправлять вызовы к собственным функциям, а не заменять их. Вместо этого:</span>

```rust
# trait Download {
#     fn download_file(&self, url: impl AsRef<str>);
# }
struct HttpClient {}

// Offloading essential functionality into traits means users
// will have to figure out what other traits to `use` to
// actually use this type.
impl Download for HttpClient {
    fn download_file(&self, url: impl AsRef<str>) {
        // ... logic to download a file
    }
}
```

do this:

<span class="ru">делайте так:</span>

```rust
# trait Download {
#     fn download_file(&self, url: impl AsRef<str>);
# }
struct HttpClient {}

impl HttpClient {
    fn download_file(&self, url: impl AsRef<str>) {
        // ... logic to download a file
    }
}

// Forward calls to inherent impls. `HttpClient` can be used
impl Download for HttpClient {
    fn download_file(&self, url: impl AsRef<str>) {
        Self::download_file(self, url)
    }
}
```
