# test

The `test` command copies the `functions.rs` and `lib.rs` from the local project to a docker image 
and executes the following command:
```bash
cargo test --lib 
```

The output of the test is displayed on the commandline. 

See the [project tutorial](/tutorials/project.md) for how this works in a wider development context.

**Notes**
If you create a default project a sample integration test is provided.

Once [built](/cli/build.md) the project can be tested using the following steps:

1. Run the container

```bash
docker run -p 8080:8080 -t nameofyourbuild
```

2. From the template project folder run the cargo test against the container.

```bash
cargo test --test '*'
```

The `--test '*'` flag is designed to ignore the tests in the lib and only run the tests in the `tests` folder.