# Rust
## Negatives
### Compatibility
1. Rust's std library has no way of allowing breaking changes.
2. [rust 2.0 breakage wishlist](https://github.com/rust-lang/rust/issues?q=is%3Aissue+sort%3Aupdated-desc+label%3Arust-2-breakage-wishlist+)


### Standard Library
1. `Range` implementing `Iterator` instead of `IntoIterator`. and thus, unable to implement `Copy`
2. not having an official runtime caused an ecosystem split where some libraries would only work with a particular runtime (tokio / async-std)


