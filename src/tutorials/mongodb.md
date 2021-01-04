# mongodb

This tutorial will walk you creating a service using mongodb as a data store. 

It will use the [mongodb template project](https://github.com/roche-rs/mongodb/) 
and outline some of the features that are bundled in the template.

The template is a `pre-release` build and colaboration on final features and functions would be highly appreciated. 

Please [take or raise an issue](https://github.com/roche-rs/mongodb/). 

**Before your begin [install the roche cli](/cli/index.html)**

## 1. Initialise a named mongo project

Projects can be initialised using templates or a remote git repo.

Here we are using a predefined template and providing a project name to the prompt.

```bash
roche init mongodb --name mongodb-sample
```

## 2. Move into the project and inspect the structure

```bash 
cd mongodb-sample
ls -R
.
├── .rocherc
├── Cargo.toml
├── LICENSE
├── README.md
└── src
    ├── .env
    ├── functions.rs
    └── lib.rs
```

**.rocherc**
.rocherc contains the images that are used to build the services.

These images store the intialised builds that reduce build times and provide an initial implementation to get up and running with.

**Cargo.toml**

This Cargo.toml is *not* used to run the builds but is supplied so that code completion and external integrations tests can be developed.

**README.md**

The description of the project and how to use it.

**src/.env**

The .env file is used by the build and test commands to ensure the environent is configured correctly.
The values in here can be modified to suite your local development environment. 

But the `.env` file is *not* shipped in a release build [(See 12 Factor Apps)](https://12factor.net/config) but *is* passed to the build stages so that tests can run.

**src/functions.rs**

This contains a some sample code to create a `City` struct to illustrate creating an Object Document Mapper with [wither](https://github.com/thedodd/wither).

Note the use of the `model` key word in the struct definintion.

```rust
#[derive(Debug, Model, Serialize, Deserialize)]
pub struct City {
``` 

The `handler` function then creates a tide app with the state object [defined in the tempate](https://github.com/roche-rs/mongodb/blob/main/image/src/main.rs#L47).

```rust
let mut api = tide::with_state(state);
```

A `get` route is then attached to the app object and the db client is extracted from the state and a call is made to the database.

**src/lib.rs**

This file contains a complete unit test that will exercise the route that you have defined in functions.rs.

```rust
use tide_testing::TideTestingExt;
let app = functions::handler(engine);
let response_json: serde_json::value::Value = app.get("/").recv_json().await.unwrap();
```

## 3. run a local mongodb

Before we run the unit test we should start a mongodb instance.
Here is a quick command using docker

```bash
docker run -p 27017:27017 -v ~/data:/data mongo:4.2
```

## 4. run the test

Once the docker instance is up and running you can run the tests with

```
roche test
```

Running roche test copies the lib.rs and the function.rs to the `test_build_image` specified in `.rocherc`.

Roche then runs cargo test --lib in the container and the output is provided on the console.

```bash
roche test
DOCKER_USERNAME environment variable not found trying to docker cli
No tag provided using roche/test-mongodb-sample
Roche: Sent file to builder for -troche/test-mongodb-sample
 Downloading crates ...
  Downloaded tide-testing v0.1.2
   Compiling tide-testing v0.1.2
   Compiling roche-mongodb-service v0.1.0 (/app-build)
    Finished test [unoptimized + debuginfo] target(s) in 11.94s
     Running target/debug/deps/roche_mongodb_service-372fa7f25cd4aa41
Roche: Build complete for -troche/test-mongodb-sample
STEP 1: FROM quay.io/roche/dev-mongodb:1.0.0
STEP 2: COPY . /app-build/src/
--> c27648fb9c7
STEP 3: RUN cargo test --lib 

running 1 test
test run_lib ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

STEP 4: COMMIT roche/test-mongodb-sample
--> 2e6d1366404
2e6d13664048224fa41a024430dd2c513b15c331c294de28cbdf673e090c08d8
```

Notice that the DOCKER_USERNAME is resolved by roche. If you wish to override this behaviour that a `DOCKER_USERNAME=yourusername` can be added to `.rocherc`.

The test also generates a docker image called `roche/test-mongodb-sample` just incase it fails and more investigation of the build is required.

## 5. Generate a Dockerfile for CI/CD

In the same folder as the `.rocherc` run in order to create a Dockerfile.

```
roche gen
```

You will notice that the file refers to the releaes image `quay.io/roche/mongodb:1.0.0 as builder`.

It also builds and tests the code in release mode and if sucessful it will copy the exe into a minimum configuration image to keep the size of the final artifact to a minimum. 

## 6. Test the Dockerfile

You can run `roche release` to mimic this functionality but here we are going to test the Dockerfile explicitly.

In the base of the project folder generate the docker file.
```
roche gen
```

Now move the docker file into the src folder, then run a docker build with a custom tag.

```bash
mv Dockerfile src
cd src
docker build -t mongo-sample-local .
```

Once the build completes we can run it 

```bash
docker run --network host \
-e MONGODB_CONNECTION_STRING=mongodb://127.0.0.1:27017 \
-e TIDE_SECRET=tobegeneratedtobegeneratedtobegenerated mongo-sample-local

```

## 7. Next Steps

Congratulations on running the mongodb template on your local machine!
Next we will look at publishing it into a knative environment.
