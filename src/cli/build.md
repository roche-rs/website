# The build command

The `build` command is the key part of the roche development workflow. 

It's specifically designed to compress rust build times to seconds rather than minutes.

That said it's intention is to be easy to operate from a developer point of view.

See the complete list of options for `build` by executing the following

```bash
roche build --help
```

## simple

If you a looking for a quick end to end guide to get up and running then please refer to the [simple tutorial](/tutorials/simple.md).
This section aims to outline some of the detail behind that tutorial.

In the default scenario `build` can be executed from either the project root or the same folder with the function file in.

```bash
roche build
```

**docker username config**
By default roche will try and obtain your current docker or podman user to generate the build tag. 

Unfortunately older versions of docker on non-linux environments don't output complete docker info.

This can be resolved by either supplying a `--tag` option or setting a `DOCKER_USERNAME` environment variable. 

## details

Behind the scenes there are 4 things going on.

1. A Dockerfile is created in memory and piped to the docker command on the local machine.

2. The Dockerfile contains the build system for the function that has been compiled previously so dependencies are ready made.

3. The final build artefact is then passed to the runtime image to ensure a clean build is achieved.

4. Docker then applies the tag to the built image.

This is a sample Dockerfile that drives this flow.

```Dockerfile
FROM quay.io/roche/dev-default:1.1.0 as builder
COPY functions.rs /app-build/src/app
RUN cargo build --release
FROM quay.io/roche/alpine:3.12
RUN apk add --no-cache libgcc
RUN addgroup -S rocheuser && adduser -S rocheuser -G rocheuser
WORKDIR "/app"
COPY --from=builder --chown=rocheuser /app-build/run.sh /app-build/Cargo.toml /app-build/target/debug/roche-service INCLUDE_ENV ./
USER rocheuser
ENV PORT 8080
EXPOSE 8080
```

The options allow you to override the two images used in the from clauses as well as the output container tag

```
roche build \
--buildimage quay.io/roche/dev-default:1.1.0 \
--runtime quay.io/roche/alpine:3.12 \
--tag namespace/buildname:version
```

## environment files

Roche supports environment files.
Simply create a `.env` file in the same location as the functions.rs and it will be included in the development build.
See the [env example](https://github.com/roche-rs/roche/tree/main/examples/env) for more details.