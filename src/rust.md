# Rust
## Negatives
### Syntax
1. struct literal syntax used `:` instead of `=`. this also ties in with the function syntax being different than struct syntax [^struct_literal_syntax][^struct_func_syntax]
> {} vs. (), named vs. positional, shorthand syntax
In an ideal world, both would use the same syntax
```rust
struct Foo(
    hello: String
)
fn foo(
    hello: String
)
```

2. using `<T>` for generics instead of `[T]`. 
**Reason**:
Tokens like `<` and `>` can appear alone as greater than or less than operators. but `[` and `]` brackets always appear in pairs. 
This also makes parsing ambiguous in expression contexts and that's why we have `::<T>` turbofish syntax.

3. adding default values per field in structs is boiler plate-y.[^default_struct_fields]
```rust
struct Window {
    x: u16,
    y: u16,
    visible: bool,
}

impl Window {
    fn new_with_visibility(x: u16, y: u16, visible: bool) -> Self {
        Window {
            x, y, visible
        }
    }

    fn new(x: u16, y: u16) -> Self {
        Window::new_with_visibility(x, y, false)
    }
}
```
```kotlin
class Window(x: Int, y: Int, visible: Boolean = false)
```

4. inconsistent, repetitive syntax for doing the same thing in different contexts.[^csharp_rust]
In C#, say you have this block of repetitive looking code:
```csharp
void Something()
{
    int    a_size = -1; // default value
    string a_name = "default";
    int    b_size = -1; // default value
    string b_bame = "default";

    string a_desc = string.Format("{0} is {1} big", a_name, a_size);
}
```
The repeated variables probably make you feel itchy, so you decide to extract them to a class or struct, so:
```csharp
class Thingie {
    int a_size = -1; // default value
    string a_name = "default";
}
```

### Compatibility
1. Rust's std library has no way of allowing breaking changes.
2. [rust 2.0 breakage wishlist](https://github.com/rust-lang/rust/issues?q=is%3Aissue+sort%3Aupdated-desc+label%3Arust-2-breakage-wishlist+)

### Typesystem
1. Auto traits: Some traits like Send and Sync are auto derived. This causes one to *accidentally* break backwards compatibility unknowingly when they change some internal implementation . 
2. Async coloring: your function cannot be generic over async code. either it is async or it is not. so, when you add a little bit of async, it starts infecting the rest of the codebase spreading async keyword everywhere.
3. 

### Standard Library
1. `Range` implementing `Iterator` instead of `IntoIterator`. and thus, unable to implement `Copy`
2. not having an official runtime caused an ecosystem split where some libraries would only work with a particular runtime (tokio / async-std)

### Tooling
1. Proc macros and build.rs are not sandboxed *at compilation time*. 
2. macros also make it harder to develop IDE tooling for rust.
3. compile times are atrocious. probably one of the longest even with a top tier CPU. 




[^default_struct_fields]: https://www.reddit.com/r/rust/comments/xcw6m5/a_personal_list_of_rust_grievances/io83zoj/
[^csharp_rust]: https://www.reddit.com/r/rust/comments/wvynot/does_rust_have_any_design_mistakes/ilkgkzg/
[^struct_literal_syntax]: https://www.reddit.com/r/rust/comments/xcw6m5/a_personal_list_of_rust_grievances/io9fznq/
[^struct_func_syntax]: https://www.reddit.com/r/rust/comments/xcw6m5/a_personal_list_of_rust_grievances/ioby850/