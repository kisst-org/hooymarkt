# markisst: MAnifest Renderer Keep It Simple Software Tools

The program `render-manifests` is a simple but flexible tool to render manifests for kubernetes.

The working is to render the manifest for a specific application (`appname`) and a specific environment.

# Environments
By default the following environments are recognized:
- `lab`: meant for infra structure testing
- `tst`: meant for testing applications
- `stg`: staging
- `prd`: production
- `all`: all environments of an application
The list of environments can be configured in the file `markisst.config`

# Render Definitions


# Running `render-manifests`
It can be run with the following options:

```
render-manifests [options] [environment...] <render-def>...

Options:
  -h|--help        show this help
  -v|--verbose     give more output
  -q|--quiet       no output
  -d|--diff        run `kubectl diff` with the rendered manifests
  -a|--apply       run `kubectl apply` with the rendered manifests
  -c|--commit      run `git commit` (--pull is implied)
  -p|--pull        run `git pull` before starting to render, to avoid merge conflicts
  -o|--output-dir  the directory where manifests will be rendered to

Environments:
It is possible to specify an environment as follows `@lab`.
Shortcuts e.g. `@l` and even just `l` or `lab` are planned

Render Definitions:
Can either be a file, that will be sourced, or a directory that contains exactly 1 file with a name `render-*.def`.
```

# Installation
The only requirement is to have a recent version of `bash` available.

Additionally the following tools are needed for some commands:
- `git`: for the `--pull` and `--commit` commands
- `kubectl`: with a correct kubeconfig file for the `--diff` and `--apply`

Installation can be done by copying the `render-manifests` script to the correct location.
The script can be placed in the directory with all the manifests definitions.

The following directory structure is recommended:
- `render-manifests`: the script itself
- `apps/<appname>/render-app-<appname>.def`: render-definitions for an application and environments
- `apps/<appname>/values-<appname>-<env>.yaml`: helm values file for a specific environment of an application
- `apps/<appname>/values-<appname>.yaml`: Optionally helm values file for all environments of an application (included before the environment specifc values)
- Optionally `apps/<appname>/render-appenv-<appname>-<env>.def`: render-definition for a specific environment of an application. Only needed if this
- `envs/values-<env>.def`: values specific for this environment
- `envs/render-env-<env>.def`: settings for the environment (e.g. kubeconfig context name)
- `manifests`: the location where the renderen manifests are stored
- `helm-charts`: the helm-charts that can be referred to in the render definitions
