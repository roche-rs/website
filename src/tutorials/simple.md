# simple

**Before your begin [install the roche cli](/cli/index.html)**

**1. make an empty folder and generate a function template**

```
mkdir tide-faas
cd tide-faas
roche init
```
This creates a single function file that you can add functionality into. 

```rust
pub fn handler() -> tide::Server<()> {    
    let mut api = tide::new();
    api.at("/").get(|_| async { Ok("Hello, World!") });
    api
}
```

**2. Login to docker and build the function image.**
```
docker login 
roche build
```

*optionally if you aren't logged or using an imcompatible docker version you can set a `DOCKER_USERNAME` environment variable*

```
export DOCKER_USERNAME=yourusername
roche build
```

**3. If you would like to run the image use the standard docker commands**
```
docker run -p 8080:8080 registry/namespace/devimagename:version
```

**4. For a release build run the following**
```
$ roche release -t registry/namespace/imagename:version
```
*These take slightly longer as they are compiled with the --release flag*

**5. Deploy to your favourite container based FaaS platform**

First push your container to a container registry
```
$ docker push registry/namespace/imagename:version
```

Then deploy into the container service of your choice

**knative**
```
kn service create roche-function --image registry/namespace/imagename:version
``` 

**ibmcloud code engine**
```
ibmcloud ce app create -n roche-function --image registry/namespace/imagename:version
```

**google cloud run**
```
gcloud run deploy roche-function --image registry/namespace/imagename:version
```

**aws fargate**
```
fargate service deploy roche-function --image registry/namespace/imagename:version
```