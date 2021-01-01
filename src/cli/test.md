# test

The `test` command copies the `functions.rs` and `lib.rs` from the local project to a docker image 
and executes `cargo test --lib` in the running container with the output of the test is displayed on the commandline. 

See the complete list of options for `test` by executing the following

```bash
roche test --help
```

## simple

If you are looking for guidance in a wider development context then please see the [project tutorial](/tutorials/project.md).

In the folder with a functions.rs and lib.rs files run the following:

```bash
roche test
```
This will build a testimage and run it. The output will be available on the console. 

If something doesn't work as expext the an image is created you can inspect it using the standard `docker run -it nameofimage /bin/bash`

## provide test container and tag

It's unlikely that you will need these options as the test image tag is generated automatically and the test image should be avaiable from the template project. 

However options to overide them are provided for usage consistency.

```bash
roche test -l quay.io/roche/dev-default:1.4.0 -t mytestimage
```

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