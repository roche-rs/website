# The init command

The `init` command is used to generate the initial boilerplate which can range from a simple function file to a predefined template or a full custom rust project. This documentation will step through each one of those scenarios to highlight how to use the init subcommand and options.

To see the complete list of options are available from the commandline by using

```bash
roche init --help
```

## simple

If you a looking for a quick end to end guide to get up and running then please refer to the [simple tutorial](/tutorials/simple.md).
This section aims to outline some of the detail behind that tutorial.


To generate a simple function the `init` command is used like this:

```bash
roche init
```

When using the `init` command without any parameters, a function file will be created for you in the current directory:

```bash
.
├── functions.rs
```

The functions.rs file is the bear minimum required to develop a running application and contains the following code:

```rust
pub fn handler() -> tide::Server<()> {    
    let mut api = tide::new();
    api.at("/").get(|_| async { Ok("Hello, World!") });
    api
}
```

This code can be modified as required and the base image has the following libraries imported as defined in the [default](https://github.com/roche-rs/default/blob/main/image/Cargo.toml) repository.

```bash
async-std = { version = "1.6.5", features = ["attributes"] }
tide = "0.15.0"
serde = "1.0.117"
serde_urlencoded = "0.7.0"
surf = "2.1.0"
dotenv = "0.15.0"
uuid = { version = "0.8", features = ["v4"] }
```

To see how these libraries can be used to larger functionality try the functions available in the [example library](https://github.com/roche-rs/roche/tree/main/examples).

When you're ready you can move onto the [build step](build.md).


## predefined template

```bash
roche init default
```

```
roche-test/
├── Cargo.toml
├── LICENSE
├── README.md
└── src
    ├── functions.rs
    └── lib.rs
```
- The `src` directory contains the functions.rs that is used to build the service. It also contains a lib.rs 
  file which provides a sample implementation that can be used to test the service.

- The `Cargo.toml` is for the test library. It is **not** used by the service.

- The `README.md` contains specific instructions for the template 


## custom template with options

```bash
roche init https://github.com/roche-rs/default.git --branch main --name roche-sample
```

Roche supports all the options of  a [cargo generate](https://github.com/ashleygwilliams/cargo-generate) project. 
This enables you to enable you to create your own templates without having to modify the main roche project.
That said if you would like your project to be a predefined template then PRs are welcome.

The branch option allows you to select which branch to choose the default is `main` if it isn't supplied.

The name option autocompletes the name prompt to enable automation.

When complete the project will look like this.

```
roche-sample/
├── Cargo.toml
├── LICENSE
├── README.md
└── src
    ├── functions.rs
    └── lib.rs
```
- The `src` directory contains the functions.rs that is used to build the service. It also contains a lib.rs 
  file which provides a sample implementation that can be used to test the service.

- The `Cargo.toml` is for the test library. It is **not** used by the service.

- The `README.md` contains specific instructions for the template 
