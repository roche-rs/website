# test

The test feature is still in design.

In the meantime the container can be tested by using the following steps:

1. Run the container

```bash
docker run -p 8080:8080 -t nameofyourbuild
```

2. Run the cargo test against the container.

```bash
cargo test
```