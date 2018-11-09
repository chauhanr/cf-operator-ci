# cf-operator-ci

## Pre-reqs
To fly one of the pipelines, a couple pre-req tools are required on your local machine:
- [`jq`](https://stedolan.github.io/jq/) is used in the convenience script (`fly-pipeline`)
- [`spruce`](https://github.com/geofffranks/spruce) is used to modify/insert parts into the final pipeline YAML (inlining scripts)
- [`fly`](https://concourse-ci.org/fly.html) to set the pipeline
- [`lpass`](https://github.com/lastpass/lastpass-cli) to retrieve secrets from the LastPass shared folder

## Set a concourse pipeline
The convenience script `fly-pipeline` allows you to set your pipeline into an existing concourse server. Run it without any arguments to get a list of configured targets and available pipelines.

_Note:_ Make sure you are logged in the concourse target, e.g `fly -t TARGET login`.

_Example:_
```
./fly-pipeline flintstone hello-world
```

## Pipeline directory structure
The pipelines must follow a simple contract in order to work with the convenience script `fly-pipeline`:
- A directory with the final pipeline name must be located under `pipelines` in the Git repo root.
- Inside the pipeline directory, there must be two files:
  - `pipeline.yml` contains the pipeline definition and supports Spruce operators, for example `(( file ... ))`.
  - `vars.yml` contains variables that are likely to be changed every once in a while, but not secrets.

You can find a pipeline example under `pipelines/hello-world`.

### Caveat
In order to not have to explicitly specify each secret by key in the `fly` command, we use the _Notes_  section of one secret as a YAML and store the required secrets in there as one block. This keeps the `fly` command simple and allows for an easy way to add more secrets, however this also means that everybody has to use one LastPass site entry. The usage of CredHub would be preferred if possible in the future.
