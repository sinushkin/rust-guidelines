<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Applications target highest viable target-cpu (M-TARGET-CPU) { #M-TARGET-CPU }

<span class="ru">Приложения собираются под наивысший доступный `target-cpu` (M-TARGET-CPU)</span>

<why>fleet performance.</why>

<span class="ru">производительность всего парка машин.</span>

Server applications should compile against the highest `target-cpu` that the deployment environment is guaranteed to support, rather than defaulting to the generic baseline.

<span class="ru">Серверные приложения должны собираться под наивысший `target-cpu`, который среда развёртывания гарантированно поддерживает, а не под универсальный базовый уровень по умолчанию.</span>

This can be achieved, for example, by setting inside `.cargo/config.toml`:

<span class="ru">Этого можно добиться, например, задав в `.cargo/config.toml`:</span>

```toml
[target.x86_64-unknown-linux-gnu]
rustflags = ["-C", "target-cpu=x86-64-v3"]

[target.x86_64-pc-windows-msvc]
rustflags = ["-C", "target-cpu=x86-64-v3"]

# Add other platforms here based on needs ...
```

Note this guideline applies only to applications, as target settings are ignored for libraries.

<span class="ru">Учтите, что это руководство относится только к приложениям, так как настройки target игнорируются для библиотек.</span>
