# C++
C++ is a statically typed compiled langauge 
## Negatives


### Backwards Compatability 

C++ evolved from C inheriting all the negatives of [C languages](./c.md). 
This means that unlike languages which have a clean slate to start from, C++ already had a bunch of cruft by the time it was already standardized. 
C/C++ are often lumped in together and this restricts how far c++ can evolve away from C. 
Anything added to C++ stays forever even if its unused by a large majority or is considered a design mistake.
Any new feature that wants to make into the language must also consider the interaction with all pre-existing features as the older features cannot be replaced / deprecated. 
> Rust has editions to deal with breakage in a defined manner

### ABI
Everything in C++ is ABI stable by default.
once something is added to the libstd++, its ABI cannot be changed without breaking all downstream users of the library.

### Build Systems and Dependency management
1. C++ doesn't have a standard build system.
2. The most popular metabuild tool at present is cmake, which has a custom DSL forcing users to learn yet another language to build C++. 
3. Some projects don't use Cmake and also require a specific combination of compiler switches to compile and usually, it involves reading their README page to copy-paste custom commands and praying to god.
4. Most hello world C++ tutorials also don't teach cmake (or any build system) and adding / linking libraries. This makes it a continuous painful area for a new C++ programmer working on his first project.

> Most modern languages like Rust (Cargo), Go, JS (npm) now come with a default dependency manager and central library repositories. 
### Committee Beaurocracy

### Macros

### Coherency

