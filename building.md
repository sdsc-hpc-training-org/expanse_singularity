### Building Singularity containers

For further customization of images, or for building images from scratch,
we can either use the free Sylabs build service or we need access to a GNU/Linux machine with `root` privileges.

Then the Singularity workflow is composed of 3 steps:

* Build the image on the external Linux system
* Transfer to Expanse
* Run on Expanse via Slurm
