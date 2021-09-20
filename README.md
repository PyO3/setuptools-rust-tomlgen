# Setuptools plugin for generating Rust Cargo.toml Files

[![github actions](https://github.com/PyO3/setuptools-rust-tomlgen/actions/workflows/ci.yml/badge.svg)](https://github.com/PyO3/setuptools-rust-tomlgen/actions/workflows/ci.yml)
[![pypi package](https://badge.fury.io/py/setuptools-rust-tomlgen.svg)](https://pypi.org/project/setuptools-rust-tomlgen/)
[![code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/ambv/black)

`setuptools-rust-tomlgen` is a plugin for `setuptools` and `setuptools-rust`. It adds the `tomlgen_rust` command which generates Rust `Cargo.toml` files from the contents of `setup.py`.

## Usage

Install from pip:

```bash
$ pip install setuptools-rust-tomlgen
```

Create a project which contains both a `setup.py` file and some Rust `.rs` files, such as [the example project](examples/tomlgen).

Generate `Cargo.toml` files via `setup.py`:

```bash
$ python setup.py tomlgen_rust
```

## Metadata

The package name will be generated from the position of the extension within
the Python package. The same version is used as the one declared in ``setup.py``
or `setup.cfg`.

The authors list is generated after the `author` and `author_email` options
from `setup.py` / `setup.cfg`, but can also be overriden using the
`authors` key in the `[tomlgen_rust]` section of `setup.cfg`:

```ini
[tomlgen_rust]
authors =
  Jane Doe <jane@doe.name>
  John Doe <john@doe.name>
```

The library name is a slugified variant of the extension package name, to
avoid name collisions within the build directory.

As a safety, `publish = false` is added to the `[package]` section
(you wouldn't publish an automatically generated package, *would you ?!*).

## Options

Use `--force` (or add `force = true` to the `[tomlgen_rust]` section of
`setup.cfg`) to force generating a manifest even when one already exists.

Use `--create-workspace` to create a virtual manifest at the root of your
project (next to the `setup.py` file) which registers all of the extensions.
This way, generic `cargo` commands can be run without leaving the root of
the project.

If `--create-workspace` is enabled, a `.cargo/config` file will also be
created to force `cargo` to build to the temporary build directory. Use
`--no-config` to disable.

## Dependencies

To specify dependencies for all extensions, add them to the
`[tomlgen_rust.dependencies]` section of your setuptools configuration file
(`setup.cfg`), as you would normally in your `Cargo.toml` file. Here is
probably a good place to add `pyo3` as a dependency.

To specify per-extension dependency, create a section for each extension
(`[tomlgen_rust.dependencies.<DOTTEDPATH>]`, where `<DOTTEDPATH>` is the
complete Python path to the extension (e.g. `hello-english`). Extension
specific dependencies are added *after* global dependencies.

*Note that, since all projects are built in the same directory, you can also
declare all dependencies in the* `[tomlgen_rust.dependencies]`, *as they will
be built only once anyway*.

## Automatic generation at each build

If you intend to regenerate manifests everytime the library is built, you can
add `Cargo.toml` and `Cargo.lock` to your `.gitignore` file.

Then, make sure `tomlgen_rust` is run before `build_rust` everytime by
adding aliases to your `setup.cfg` file:

```ini
[aliases]
build_rust = tomlgen_rust -f build_rust
clean_rust = tomlgen_rust -f clean_rust
build = tomlgen_rust -f build
clean = clean_rust -f clean
```
