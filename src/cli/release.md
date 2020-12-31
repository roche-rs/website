# The release command

The `release` command is similar to build but as rust release builds take longer it's not intended to be used as part of the development flow. 

It's designed to enable developers to confirm what the released artifact will contain.

See the complete list of options for `release` by executing the following

```bash
roche release --help
```

## simple

If you a looking for a quick end to end guide to get up and running then please refer to the [simple tutorial](/tutorials/simple.md).
This section aims to outline some of the detail behind that tutorial.

In the default scenario `release` can be executed from either the project root or the same folder with the function file in.

```bash
roche release
```

*you must ensure that you are logged into docker or podman if you don't provide a `--tag` option*

## details

Behind the scenes there are 4 things going on.

1. A Dockerfile is created in memory and piped to the docker command on the local machine.

2. The Dockerfile contains the build system for the function that has been compiled previously so dependencies are ready made.

3. The final build artefact is then passed to the runtime image to ensure a clean build is achieved.

4. Docker then applies the tag to the built image.

This is a sample Dockerfile that drives this flow.

```Dockerfile
FROM quay.io/roche/default:1.0.0 as builder
COPY functions.rs /app-build/src/app
RUN cargo build --release
FROM quay.io/roche/alpine:3.12
RUN apk add --no-cache libgcc
RUN addgroup -S rocheuser && adduser -S rocheuser -G rocheuser
WORKDIR "/app"
COPY --from=builder --chown=rocheuser /app-build/run.sh /app-build/Cargo.toml /app-build/target/release/roche-service ./
USER rocheuser
ENV PORT 8080
EXPOSE 8080
```

The options allow you to override the two images used in the from clauses as well as the output container tag

```
roche build \
--buildimage quay.io/roche/default:1.0.0 \
--runtime quay.io/roche/alpine:3.12 \
--tag namespace/buildname:version
```

## environment files

While Roche supports environment files for dev builds they are **not** included in the release build.
This is to prevent sensitive information being managed incorrectly in production environments.