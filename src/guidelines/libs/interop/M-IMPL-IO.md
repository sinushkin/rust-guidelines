<!-- Copyright (c) Microsoft Corporation. Licensed under the MIT license. -->

## Accept `impl 'IO'` where feasible ('sans IO') (M-IMPL-IO) { #M-IMPL-IO }

<span class="ru">Принимайте `impl 'IO'`, где это возможно («sans IO») (M-IMPL-IO)</span>

<why>business logic untangled from I/O, with N*M composability.</why>

<span class="ru">бизнес-логика отделена от I/O, с композируемостью N×M.</span>

Functions and types that only need to perform one-shot I/O during initialization should be written "[sans-io](https://www.firezone.dev/blog/sans-io)",
and accept some `impl T`, where `T` is the appropriate I/O trait, effectively outsourcing I/O work to another type:

<span class="ru">Функции и типы, которым нужна однократная операция I/O только при инициализации, следует писать в стиле [sans-io](https://www.firezone.dev/blog/sans-io) и принимать некоторый `impl T`, где `T` — соответствующий трейт I/O, фактически передавая работу с I/O другому типу:</span>

```rust,ignore
// Bad, caller must provide a File to parse the given data. If this
// data comes from the network, it'd have to be written to disk first.
fn parse_data(file: File) {}
```

```rust
// Much better, accepts
// - Files,
// - TcpStreams,
// - Stdin,
// - &[u8],
// - UnixStreams,
// ... and many more.
fn parse_data(data: impl std::io::Read) {}
```

Synchronous functions should use [`std::io::Read`](https://doc.rust-lang.org/std/io/trait.Read.html) and
[`std::io::Write`](https://doc.rust-lang.org/std/io/trait.Write.html). Asynchronous _functions_ targeting more than one runtime should use
[`futures::io::AsyncRead`](https://docs.rs/futures/latest/futures/io/trait.AsyncRead.html) and similar.
_Types_ that need to perform runtime-specific, continuous I/O should follow [M-RUNTIME-ABSTRACTED] instead.

<span class="ru">Синхронные _функции_ должны использовать [`std::io::Read`](https://doc.rust-lang.org/std/io/trait.Read.html) и [`std::io::Write`](https://doc.rust-lang.org/std/io/trait.Write.html). Асинхронные _функции_, рассчитанные более чем на один рантайм, следует реализовывать через [`futures::io::AsyncRead`](https://docs.rs/futures/latest/futures/io/trait.AsyncRead.html) и аналогичные. _Типы_, которым нужен непрерывный I/O, специфичный для рантайма, вместо этого следуют [M-RUNTIME-ABSTRACTED].</span>

[M-RUNTIME-ABSTRACTED]: ./#M-RUNTIME-ABSTRACTED
