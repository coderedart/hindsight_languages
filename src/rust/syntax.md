## Syntax
https://www.peter.bertok.com/posts/rust_refactoring_and_syntax/
### Struct Literals
Struct literal syntax used `:` instead of `=`.
This also ties in with the function syntax being different than struct syntax [^struct_literal_syntax][^struct_func_syntax]

> {} vs. (), named vs. positional, shorthand syntax

In an ideal world, both would use the same syntax like:
```rust,ignore
struct Foo(
    hello: String
)
fn foo(
    hello: String
)
```

### Generics 
Using `<T>` for generics instead of `[T]`. 
**Reason**:
Tokens like `<` and `>` can appear alone as greater than or less than operators. but `[` and `]` brackets always appear in pairs. 
This also makes parsing ambiguous in expression contexts and that's why we have `::<T>` turbofish syntax.

### BoilerPlatey Defaults of struct
adding default values per field in structs is boiler plate-y.[^default_struct_fields]
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

compare to equivalent kotlin

```kotlin
class Window(x: Int, y: Int, visible: Boolean = false)
```

### Inconsistency / Copy-Paste Friendliness
inconsistent, repetitive syntax for doing the same thing in different contexts.[^csharp_rust]
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
just move the declaration into a struct with copy/paste.
with rust, OTOH:
```rust
fn something() {
    let a_size: i32 = -1;
    let a_name: String = "default".to_string();
}
```
if we move this to a struct,
```rust
struct Thingie {
    /* nope. we cannot copy paste like this
    let a_size: i32 = -1;
    let a_name: String = "default".to_string();
    */
}
```
instead, we will have to 
1. remove the let keyword from each line
2. move the default values into a separate trait impl of Default
3. replace the semicolons with commas

[^default_struct_fields]: https://www.reddit.com/r/rust/comments/xcw6m5/a_personal_list_of_rust_grievances/io83zoj/
[^csharp_rust]: https://www.reddit.com/r/rust/comments/wvynot/does_rust_have_any_design_mistakes/ilkgkzg/
[^struct_literal_syntax]: https://www.reddit.com/r/rust/comments/xcw6m5/a_personal_list_of_rust_grievances/io9fznq/
[^struct_func_syntax]: https://www.reddit.com/r/rust/comments/xcw6m5/a_personal_list_of_rust_grievances/ioby850/
