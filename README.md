Rustpy
=====

A simple library to allow for easy use of python from rust.

## Status
Currently this library has not received much love (pull requests welcome for any interested) and does not build with rust 1.0.

For another library that also strives to bridge the gap between python and rust and might be a little more up to day see:

https://github.com/dgrunwald/rust-python27-sys

https://github.com/dgrunwald/rust-cpython

## How to Use

This library is meant to be middle ware for users wanting to use
python libraries from rust. It allows users to quickly use existing
tools and get working on interesting things fast!

See [pysmtplib.rs](https://github.com/lukemetz/pysmtplib.rs) for an
example of how to bind enough smtplib to send emails.


For more documentation, run `rustdoc src/rustpy.rs` and look at
doc/rustpy/index.html. Pull requests are welcome!

```rust
extern crate rustpy;
use rustpy::{ToPyType, FromPyType, PyState};

fn main() {
  let py = PyState::new();
  let module = py.get_module("math").unwrap();
  let func = module.get_func("sqrt").unwrap();
  let args = (144f32, ).to_py_object(&py).unwrap();
  let untyped_res = func.call(&args).unwrap();
  let result = py.from_py_object::<f32>(untyped_res).unwrap();
  assert_eq!(result, 12f32);
}
```
Important note: Only create one instance of PyState at a time.
On construction, it grabs a global lock to prevent more than one thread from
interacting with the interpreter thus making it very easy to deadlock.
