# test

The test feature is still in design.

In the meantime the container assuming a project is [initialised](/cli/init.md) and [built](/cli/build.md) from a template it can be tested using the following steps:

1. Run the container

```bash
docker run -p 8080:8080 -t nameofyourbuild
```

2. From the template project folder run the cargo test against the container.

```bash
cargo test
```