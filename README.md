# Sui duplicate module name bug

The `foo` package defines a module named `foo`. The `bar` package also defines a module named `foo`, and it depends on `bar`.

Since both packages locally have the `0x0` address, the compiler puts their modules in the same namespace, so running

``` shell
cd bar && sui move build
```

fails with 

``` text
error[E02001]: duplicate declaration, item, or annotation
  ┌─ ./../foo/sources/foo.move:1:13
  │
1 │ module foo::foo;
  │             ^^^ Duplicate definition for module '(foo=0x0)::foo'
  │
  ┌─ ./sources/bar.move:1:13
  │
1 │ module bar::foo;
  │             --- Module previously defined here, with '(bar=0x0)::foo'

Failed to build Move modules: Compilation error.
```

Even though this should be a valid move program.
