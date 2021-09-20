# `example_tomlgen`

An example extension with automatically generated `Cargo.toml` manifest
files. Simply run `python setup.py tomlgen_rust` to generate the following
files:

* `Cargo.toml` for `hello-english`

  ```toml
  [package]
  name = "hello-english"
  version = "0.1.0"
  authors = ["Martin Larralde <martin.larralde@ens-paris-saclay.fr>"]
  publish = false

  [lib]
  crate-type = ["cdylib"]
  name = "hello_english"
  path = "lib.rs"

  [dependencies]
  pyo3 = { version = "*", features = ["extension-module"] }
  english-lint = "*"
  ```

* `Cargo.toml` for `hello.french`

  ```
  [package]
  name = "hello.french"
  version = "0.1.0"
  authors = ["Martin Larralde <martin.larralde@ens-paris-saclay.fr>"]
  publish = false

  [lib]
  crate-type = ["cdylib"]
  name = "hello_french"
  path = "lib.rs"

  [dependencies]
  pyo3 = { version = "*", features = ["extension-module"] }
  ```
