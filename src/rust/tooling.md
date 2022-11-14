## Tooling
### Beginner Friendliness
1. Cargo is really great. serves as both a build system and a dependency manager. 
2. crates.io, docs.rs, rustup.rs etc.. are all great websites 

### Performance
1. compile times are atrocious. 




### Issues
1. macros also make it harder to develop IDE tooling for rust.
2. Proc macros and build.rs are not sandboxed *at compilation time*. So, random build scripts from external crates can do whatever they want.
3. Rust-analyzer + intellij rust plugin are the main helpers, but neither of them are lightweight.



