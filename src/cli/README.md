# Command Line Tool

roche can be used either as a command line tool or a [Rust
crate](https://crates.io/crates/roche). We will look at prebuilt releases first 
and then the different options for building from source.

## Install From Binaries

Precompiled binaries **will be** provided on a best-effort basis.
Visit [the releases page](https://github.com/roche-rs/roche/releases)
to download the appropriate version for your platform.

## Install From Source

roche can also be installed from source

### Pre-requisite

roche is written in **[Rust](https://www.rust-lang.org/)** and therefore needs
to be compiled with **Cargo**. If you haven't already installed Rust, please go
ahead and [install it](https://www.rust-lang.org/tools/install) now.

If you are new to rust we strongly recommend taking the rustup option at the top of that link.

### Install Crates.io version

Installing roche is relatively easy if you already have Rust and Cargo
installed. You just have to type this snippet in your terminal:

```bash
cargo install roche
```

This will fetch the source code for the latest release from
[Crates.io](https://crates.io/) and compile it. If you didn't choose the rustup option 
you may have to add Cargo's `bin` directory to your `PATH`.

Run `roche help` in your terminal to verify if it works. Congratulations, you
have installed roche!


### Install Git version

The **[git version](https://github.com/roche-rs/roche)** contains all
the latest bug-fixes and features, that will be released in the next version on
**Crates.io**, if you can't wait until the next release. You can build the git
version yourself. Open your terminal and navigate to the directory of you
choice. We need to clone the git repository and then build it with Cargo.

```bash
git clone --depth=1 https://github.com/roche-rs/roche.git
cd roche
cargo build --release
```

The executable `roche` will be in the `./target/release` folder, this should be
added to the path.
