# conda
conda is used in logik
- conda should be installed in the host system, with common tools installed (such as gcc)
- for tools maintained by logik either self-created or customized, a conda recipe is provided so that conda package can be generated and installed to host.

# conda build
build and install the local package

# env update
ref to https://izziswift.com/how-to-update-an-existing-conda-environment-with-a-yml-file/
```bash
conda env update --name myenv --file local.yml
```
