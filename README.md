# proc-macro2

[Documentation](https://docs.rs/proc-macro2)

A small shim over the `proc_macro` crate in the compiler intended to multiplex
the current stable interface (as of 2017-07-05) and the [upcoming richer
interface][upcoming].

[upcoming]: https://github.com/rust-lang/rust/pull/40939

The upcoming support has features like:

* Span information on tokens
* No need to go in/out through strings
* Structured input/output

The hope is that libraries ported to `proc_macro2` will be trivial to port to
the real `proc_macro` crate once the support on nightly is stabilize.

## Usage

This crate by default compiles on the stable version of the compiler. It only
uses the stable surface area of the `proc_macro` crate upstream in the compiler
itself. Usage is done via:

```toml
[dependencies]
proc-macro2 = "0.1"
```

followed by

```rust
extern crate proc_macro;
extern crate proc_macro2;

#[proc_macro_derive(MyDerive)]
pub fn my_derive(input: proc_macro::TokenStream) -> proc_macro::TokenStream {
    let input: proc_macro2::TokenStream = input.into();

    let output: proc_macro2::TokenStream = {
        /* transform input */
    };

    output.into()
}
```

If you'd like you can enable the `unstable` feature in this crate. This will
cause it to compile against the **unstable and nightly-only** features of the
`proc_macro` crate. This in turn requires a nightly compiler. This should help
preserve span information, however, coming in from the compiler itself.

You can enable this feature via:

```toml
[dependencies]
proc-macro2 = { version = "0.1", features = ["unstable"] }
```

# License

`proc-macro2` is primarily distributed under the terms of both the MIT license and
the Apache License (Version 2.0), with portions covered by various BSD-like
licenses.

See LICENSE-APACHE, and LICENSE-MIT for details.
