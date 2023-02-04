### Building Singularity images on Jetstream2

If you have access to the Jetstream2 cloud service and are already familiar with how to use it,
it could be a good option to build Singularity containers there
instead of on a laptop. Consider in particular that
complicated containers like PyTorch or Tensorflow take hours to build.

Login to <https://jetstream2.exosphere.app> with your ACCESS credentials.
Under the Create button choose "SSH Public key" and paste the key you will be using to SSH into the Virtual Machine.

Under the Create button choose "Instance", choose Ubuntu 22.04 as OS.

Choose at least a `m3.quad` which has 4 vCPUs and 15 GB of RAM.

Open the advanced options and select the key you just uploaded.

Wait for the VM to build, access the details to find out the public IP, then SSH with:

    ssh ubuntu@xxx.xxx.xxx.xxx

In order to avoid warnings during the Singularity build process, it is better to restart the Virtual Machine:

    sudo reboot
