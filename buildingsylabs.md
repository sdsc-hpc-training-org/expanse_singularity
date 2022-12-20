### Building Singularity images using the Sylabs remote build service

For building images that are not too big, i.e., less than 11 GB and with a build-time less than 1 hour, the company behind Singularity, Sylabs, offers a free cloud based service accessible from the command line.

This service sends your Singularity definition file to one of the Sylabs workers which builds the container and then sends it back to your machine.

This procedure also works on the login node on Expanse, because no computation is actually done by the local machine.

#### Login to Sylabs builder

Go to <https://cloud.sylabs.io/builder> and create a new account. We recommend using GitHub.

Go to <https://cloud.sylabs.io/tokens>, generate a token, name it after the machine you are running from, e.g. expanse.

Copy it to the clipboard and then paste it when requested in the terminal after running:

    singularity remote login

First specify the username if it is different between Expanse and Sylabs:

    SYLABS_USER='xxxxxxxxx'
    singularity remote login --username $SYLABS_USER

#### Create a custom definition file

We can take one of the definition files from `naked-singularity` and customize it, for example the latest Miniconda image, which currently is:

    definition-files/miniconda/Singularity.miniconda3-py39-4.11.0-ubuntu-20.04

Just after `conda` is installed, `rm "${CONDA_INSTALLER}"`, we can add some custom `conda` packages, and cleanup:

```
    conda install -y -c conda-forge numpy matplotlib astropy
    conda clean -afy
```

#### Build the image remotely

We can then submit the build job with:

    singularity build --remote /expanse/lustre/scratch/$USER/temp_project/miniconda_astropy.sif miniconda/Singularity.miniconda3-py39-4.11.0-ubuntu-20.04

It is quite impressive is that the log of your job shows up on Expanse even if the job is actually running remotely on Sylabs machines.

After the process is completed you should have your container in `/expanse/lustre/scratch/$USER/temp_project/miniconda_astropy.sif`.
