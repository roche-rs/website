# The gen command

The `gen` command is used to generate a Dockerfile that can be shipped with your function.
This enables CI/CD scenarios where the preference is not to allow the roche cli to proliferate outside of development environments.

See the complete list of options for `gen` by executing the following

```bash
roche gen --help
```

## simple

To take the default settings run 

```bash
roche gen
```

This will create the following Dockerfile which is identical to the one created during the [release](cli/release.md) command.

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

For more information on how this file see the [release](cli/release.md) documentation.

If you are using a custom baseimage or a custom runtime then these can be overridden using the appropriate options.

```
roche gen --buildimage quay.io/roche/default:1.0.0 --runtime quay.io/roche/alpine:3.12
```


By default the Dockerfile is configured to expect the functions.rs file to be in the same folder. 

This of course is modifiable by editing the location of the functions.rs in the COPY line.
For example you may wish to have the Dockerfile in the root of your project so the line could be edited as follows

```Dockerfile
COPY src/functions.rs /app-build/src/app
```

