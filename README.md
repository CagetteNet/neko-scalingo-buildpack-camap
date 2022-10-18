## Buildpacks

### Introduction

A buildpack has three mandatory entrypoints:

-   bin/detect: exit with success (return code is 0) if the buildpack applies to the current application.
-   bin/compile: installs the dependencies of the project. It is called with three arguments:

    -   The build directory: contains the code of the application. 

        _BUILD_DIR=${1:-}_
    -   The cache directory: used to store information one want to keep between two builds. 

        _CACHE_DIR=${2:-}_
    -   The environment directory: contains a file per environment variable defined. For instance, an environment variable TEST=1234 leads to a file named TEST containing 1234. 

        _ENV_DIR=${3:-}_
-   bin/release: handles some metadata and defines how the application should be started.

All these entrypoints are usually Bash script.

### Neko Scalingo Buildpack

-   Haxe on Scalingo is built with a 

    [custom buildpack](https://doc.scalingo.com/platform/deployment/buildpacks/custom)

     which is this repository.

Test build with:

```shell :eval,no
  docker run --pull always --rm -ti -e STACK=scalingo-20 \
         -v $PWD:/buildpack \
         -v $PWD/../app/:/build scalingo/scalingo-20:latest bash  
```

Note that a small profile script is required to merge runtime environement variables to the `config.xml` file.

### Buildpack github token

The custom buildpack is stored in a private repo. The buildpack URL must include a valid github token (aka `PAT`).

    BUILDPACK_URL=https://<token>@github.com/CagetteNet/neko-scalingo-buildpack.git#main

## DB

DB firewall was updated to allow access from [Scalingo's egress address](https://doc.scalingo.com/platform/internals/network#osc-fr1-region)(`171.33.105.206`)

