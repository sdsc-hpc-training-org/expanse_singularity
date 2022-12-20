### Run a GPU container on Expanse

One of the most powerful features of Singularity is that it supports loading the GPU drivers
directly from the host, so the container doesn't need to have the required GPU drivers built into it.
This is accomplished by passing the `--nv` flag to `singularity exec`.

Let's now run an example PyTorch training on a GPU on Expanse. First make sure you have
[downloaded the PyTorch container](./sdscimages.html).

Then we can download an example MNIST training:

    cd /expanse/lustre/scratch/$USER/temp_project
    mkdir mnist
    cd mnist
    wget https://github.com/pytorch/examples/raw/2bf23f105237e03ee2501f29670fb6a9ca915096/mnist/main.py

We do the above in a new subfolder because the Python script will write some input data to `../data`, so we need that path to be writable.

Now we can create a Slurm job named `run-pytorch-gpu.slurm`:

```bash
#!/usr/bin/env bash
#SBATCH --job-name=pytorch-gpu
#SBATCH --account=<ADD YOUR PROJECT HERE>
#SBATCH --partition=gpu-debug
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=10
#SBATCH --cpus-per-task=1
#SBATCH --mem=93G
#SBATCH --gpus=1
#SBATCH --time=00:10:00
#SBATCH --output=log-pytorch-gpu.o%j.%N

declare -xr SINGULARITY_MODULE='singularitypro/3.9'

module purge
module load "${SINGULARITY_MODULE}"
module list

CONTAINER_FILE='../naked-singularity_pytorch-1.10.2-ubuntu-20.04-cuda-11.2-mlnx-ofed-4.9-4.1.7.0-openmpi-4.1.3.sif'

time -p singularity exec --bind /expanse,/scratch --nv $CONTAINER_FILE python3 main.py
```

The only differences are the `--nv` argument to `singularity exec`, the queue and the `gpus` flag in the Slurm job.

Finally we can submit it with:

    sbatch run-pytorch-gpu.slurm
