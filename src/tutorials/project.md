# project

This tutorial will walk you through modifiying the default project from the default surf request to return some JSON instead.
It is intended to get you orientated with the more rust-like developer flow for roche.

**Before your begin [install the roche cli](/cli/index.html)**

**1. Initialise a project using the default tempate and provide a name option**

```
roche init default --name sample-project
```
For more details on this command see the [init documentation](/cli/init.md/#predefined-template)

**2. Move into the project folder and inspect the contents**

```
cd sample-project
tree
.
├── Cargo.toml
├── LICENSE
├── README.md
├── src
│   ├── Dockerfile
│   ├── functions.rs
│   └── lib.rs
└── tests
    └── integration.rs
```


**3. Modify the api to return some JSON.**

We are going to replace the contents of the `src/functions.rs` with the following code:

```rust
use tide::prelude::*; // Pulls in the json! macro.

pub fn handler() -> tide::Server<()> {    
    let mut api = tide::new();
    api.at("/animals").get(|_| async {
        Ok(json!({
            "meta": { "count": 2 },
            "animals": [
                { "type": "cat", "name": "chashu" },
                { "type": "cat", "name": "nori" }
            ]
        }))
    });
    api
}
```

**3. Run the test**

Now run the tests with `roche test` to see if we have broken anything.

```
roche test
...
running 1 test
test run_lib ... FAILED

failures:

---- run_lib stdout ----
thread 'run_lib' panicked at 'assertion failed: resp_string.contains("httpbin.org")', src/lib.rs:11:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    run_lib

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out
```
As expected the test fails.

**4. Redefine test**

We have broken the tests the response string no longer contains *httpbin.org* :( 

So let's update the `src/lib.rs` file so that the test pass.

As we are now returning JSON we can use a more formal type to test against.

```rust
pub mod functions;

#[cfg(test)]
#[async_std::test]
async fn run_lib() {
    use tide::prelude::*;
    use tide_testing::TideTestingExt;
    let app = functions::handler();
    let response_json : serde_json::value::Value = app.get("/animals").recv_json().await.unwrap();

    assert_eq!(
        response_json,
        json!({
            "meta": { "count": 2 },
            "animals": [
                { "type": "cat", "name": "chashu" },
                { "type": "cat", "name": "nori" }
            ]
        })
    );
}
```

The [TideTestingExt library]() gives some nice utility features that allow us to exercise routes without running a full http server.

As the container is now so pre-fetched retests will be a lot faster.

Note that the integration tests are ignored. This is by design as they exercise the http endpoint.

The integration tests where the environment where the app is deployed. 

There will be a further tutorial on this when we look at an end to end knative workflow.

**5. Prepare for CI/CD**

Roche supports building releases from the desktop with the [release](/cli/release.md) option.
However it might be preferable to integrate the code in a more formal CI/CD pipeline.

As we don't want to introduce the roche cli into the CI/CD pipeline it's possible to generate the Dockerfile and include it with your project. This can then be picked up easily by the CI tool.

Just cd into the `src` folder and run `roche gen`.

```
cd src
roche gen
```

This generates the following document that can be modified to suit the CI requirements.

```dockerfile
FROM quay.io/roche/default:1.4.0 as builder
COPY functions.rs /app-build/src
COPY lib.rs /app-build/src
RUN cargo build --release && cargo test --lib --release
FROM quay.io/roche/alpine-libgcc:3.12
RUN addgroup -S rocheuser && adduser -S rocheuser -G rocheuser
WORKDIR "/app"
COPY --from=builder --chown=rocheuser /app-build/run.sh /app-build/Cargo.toml /app-build/target/release/roche-service ./
USER rocheuser
ENV PORT 8080
EXPOSE 8080
CMD ["./run.sh"]
```

**6. That's it!**

Hopefully you now have a feel for how roche development with tide can work in a more rust project manner.