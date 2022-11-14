## Typesystem

### Auto traits
Some traits like Send and Sync are auto derived.
This causes one to *accidentally* break backwards compatibility unknowingly when they change some internal implementation . 


### Linear types
we cannot express in rust that a type *must* be used exactly once. [^linear_types]






[^linear_types]: https://faultlore.com/blah/linear-rust/
