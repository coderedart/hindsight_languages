### Async 
Your function cannot be generic over async code.
#### Coloring
Either it is async or it is not. so, when you add a little bit of async, it starts infecting the rest of the codebase spreading async keyword everywhere.
1. Every function has a color. 
    yep. `async fn` vs `fn` -> returns future or not. 
2. The way you call a function depends on its color.
    yes. .await syntax. 
3. You can only call a red function from within another red function. 
    nope. technically, you can block_on a future inside a sync fn. 
4. Red functions are more painful to call.
    yep. well, it has more to do with the synchronization / borrowing issues rather than just calling them with some_fn().await. 
5. Some core library functions are red.
    yes, but not yet because rust doesn't really have async fns in std yet. instead we need to use external crates like async-std / tokio for which this is true. 
    unless we get keyword generics[^keyword_generics] , we will have a async version of a lot of std apis like fs, sync (mutexes, channels ), net and so on. 

#### Runtime
Unlike garbage collected languages like Go or .Net family or Javascript, Rust has to work *without* a runtime. 
This is made worse because Rust doesn't actually provide a runtime in std library either. So, we have to go find external crates to find a suitable runtime. This can be considered a good thing and a bad thing. On one hand, we can explicitly choose the most suitable runtime for your workload, but OTOH It is a long-lasting choice for your project. The runtime might be abandoned or your organization might not allow dependencies which are not backed by an organization. 

### Error Handling
Rust has two different ways of dealing with errors. 
1. Panic machinery
This is kinda like exceptions. It starts throwing the exception up the call chain until someone catches it or program crashes.
Has stack unwinding to provide a backtrace.
The primary advantage is that a function doesn't have to encode in the fn signature that it can panic. 
Similar to an allocator, this is a global thing. so, we can register a custom panic handler to log errors and so on before unwinding stack.
2. Result Type
This is a generic enum `Result<T, E>` with an `Ok(T)` variant and an `Err(E)` variant. This allows you to encode the return type clearly. 
Any function that wants the resulting `T` of this `Result` MUST deal with the error case explicitly (at the very least, simply by `unwrap`ing to panic).
Allows explicit `Error` types which you can recover from by simply handling the error and using a default T or calling the previous fn again etc..

From the reddit post[^cpp_rust_exception_error] :
#### Exception-Agnosticism is Easy, but Error-Agnosticism is Not
Exception vs Error can be considered as `panic` vs `result`.
We cannot bubble up an `Result` automatically when we don't care about it. we have to encode it in the fn signature.
so, if an infallible function becomes fallible, it will be a breaking change. 
Whereas, with panicking, we don't have to worry about this. let the caller handle the panic, we can just work with the code under the assumption that there will be no errors. 
#### Boiler plate
You will have to use match statements when dealing with error variants. Leads to lots of noise sometimes. and this noise is almost at every callsite even thought most of them repeat the same match code. 
```rust,ignore
match fallible_fn() {
    Ok(_) => {}
    Err(e) => {
        // do something with e. often, e is an enum itself
        match e {
            ErrorA => {}
            // ...
            ErrorX => {}
        }
    }
}
```
 ### std library versioning
 This is not a rust specific issue. 
 Rust can break backwards comaptibility in core language using editions. but, because two crates interacting with each other use the same std library, 
 Rust cannot break backwards compatibility in std library forever[^std_back_compat]. 
 





[^keyword_generics]: https://blog.rust-lang.org/inside-rust/2022/07/27/keyword-generics.html
[^cpp_rust_exception_error]: https://www.reddit.com/r/rust/comments/xj2a23/my_thoughts_on_rust_and_c/
[^std_back_compat]: https://news.ycombinator.com/item?id=22558259