#  Slurm Common Usage Examples

---
## Running python script

### Python script : demo.py

```python
import numpy as np

print("Starting computation...")
A = np.random.rand(1000, 1000)
B = A @ A.T
print("Done. Result shape:", B.shape)
```
*You can use venv or conda environments.*

### Interactive mode, without shell

- Without a venv - compute node system python will be used
```bash
srun python3 demo.py
```
- With a venv
```bash
srun bash -c "source <PATH-TO-VENV>/bin/activate && python3 demo.py" 
```
*`bash -c` run a non interactif shell, and `source` activate the environment.* 


### Interactive mode, with shell

- Open a shell
```bash
srun --pty --cpus-per-task=2 --mem=4G --time=00:20:00 bash --norc --noprofile
```
- And run commands
```bash
source <PATH-TO-VENV>/bin/activate    # If using virtualenv or conda
python3 demo.py
```

### Batch mode

- Create a batch script `run_python.sbatch`
```
#!/bin/bash
#SBATCH --job-name=python-job
#SBATCH --output=python-%j.out
#SBATCH --error=python-%j.err
#SBATCH --time=00:10:00
#SBATCH --cpus-per-task=2
#SBATCH --mem=4G

source <PATH-TO-VENV>/bin/activate    # If using virtualenv or conda
srun python3 demo.py
```
    - `%j` in the output/error filenames is replaced by the job ID.

- Then submit the job:
```
sbatch run_python.sbatch
```
- Display result
```bash
cat python-*.out
```   

---
## Running a Jupyter Notebook

### Interactive mode

- Open a shell
```bash
srun --pty --cpus-per-task=2 --mem=4G --time=00:20:00 bash --norc --noprofile
```
- Run commands, and get Notebook url `http://localhost:8888/?token=abc123...`
```bash

source <PATH-TO-VENV>/bin/activate    # If using virtualenv or conda

# Juypter env configuration
export WORKING_DIR=<YOUR-WORKING-DIR>

export XDG_CACHE_HOME=$WORKING_DIR/jupyter/cache
export XDG_CONFIG_HOME=$WORKING_DIR/jupyter/config
export XDG_DATA_HOME=$WORKING_DIR/jupyter/data
export JUPYTER_RUNTIME_DIR=$WORKING_DIR/jupyter/runtime
export JUPYTER_CONFIG_DIR=$WORKING_DIR/jupyter/config
export PYTHONUSERBASE=$WORKING_DIR/jupyter/env

jupyter notebook --no-browser --port=8888
```

Services running in compute node are not directly accessible from your local machine. A SSH tunnel must be used. Then from your local machine :

- Create a SSH un tunnel to the compute node
``` bash
ssh -L 8888:localhost:8888 <YOUR_LOGIN>@<COMPUTE_NODE>
```
*The `-L` flag in the `ssh` command creates a secure tunnel from a port on your local machine to the same port on the compute node, allowing you to access the Jupyter Notebook running remotely as if it were running on your own computer.*
- Open Notebook url `http://localhost:8888/?token=abc123...` in your browser.

### Batch mode

- Create/copy a notebook `demo.ipynb`. The content could be a cell with the code :
``` bash
import numpy as np
A = np.random.rand(1000, 1000)
print("Matrix product:", A @ A.T)
```
- Create the sbatch script : `run_notebbok.sbatch`
``` bash
#!/bin/bash
#SBATCH --job-name=notebook-batch
#SBATCH --output=notebook-%j.out
#SBATCH --error=notebook-%j.err
#SBATCH --time=00:10:00
#SBATCH --cpus-per-task=2
#SBATCH --mem=4G

source <PATH-TO-VENV>/bin/activate # If using virtualenv or conda

export WORKING_DIR=<YOUR-WORKING-DIR>

export XDG_CACHE_HOME=$WORKING_DIR/jupyter/cache
export XDG_CONFIG_HOME=$WORKING_DIR/jupyter/config
export XDG_DATA_HOME=$WORKING_DIR/jupyter/data
export JUPYTER_RUNTIME_DIR=$WORKING_DIR/jupyter/runtime
export JUPYTER_CONFIG_DIR=$WORKING_DIR/jupyter/config
export PYTHONUSERBASE=$WORKING_DIR/jupyter/env

# Convert and execute the notebook
srun jupyter nbconvert --to notebook --execute demo.ipynb --output executed_notebook.ipynb
```
- Then submit the job
``` bash
sbatch run_notebbok.sbatch
```

This will:

- Execute all cells in the notebook
- Write the output (with results) to executed_notebook.ipynb



---
## Launching an Application on GPU


### Overview

To use GPU, just add option `--gres=gpu:xxx` to `srun`, `salloc` or `sbatch` command.

- `--gres=gpu:1`: Request 1 GPU (Generic RESource: GPU)
- `--gres=gpu:rtx3080:2`: Request 2 RTX3080 GPU card
- `--gres=gpu:h100:1`: Request 1 H100

You also must add :

- `--cpus-per-task` : CPU cores allocation, needed to assist GPU workload (data loading, etc.)
- `--mem` : RAM allocation
- `--time=00:05:00` : duration limitation, otptional but recommended

GPU are available on :

- `MeeGPU` partition: very limited (e.g., only `gpu:p5000:1`)
- `Global` partition: all GPUs, but with low priority
- Your team's dedicated partition (if available)
  
List partitions and their available generic resources (like GPUs)
``` bash
sinfo -o "%P %G"
```

### Basic GPU request

- Request ressource allocation
``` bash
srun -p Global \
     --pty \
     --gres=gpu:1 --cpus-per-task=2 --mem=4G \
     --time=00:05:00 \
     bash --norc --noprofile
```
- Check GPU
```Bash
nvidia-smi 
...
|   0  NVIDIA GeForce RTX 3080        Off |   00000000:17:00.0 Off |                  N/A |
...
```
### Specific GPU model request
- Request ressource allocation
``` bash
srun -p Global \
     --gres=gpu:rtx8000:1 --cpus-per-task=2 --mem=4G \
     --pty \
     --time=00:05:00 \
     bash --norc --noprofile
```
- Check GPU
```Bash
nvidia-smi 
...
|   0  Quadro RTX 8000                Off |   00000000:3B:00.0 Off |                    0 |
...
```                                                                                         

---
## Distributed Multi-GPU Applications

### On the SLURM Side

Slurm natively allows you to request resources with multiple GPUs, either on a single compute node or across several nodes.

- Options for 4 GPUs, "Multi-GPU on the same machine":
    - `--nodes=1`: Run on 1 physical node
    - `--ntasks-per-node=4`: 4 tasks/processes per node (1 per GPU)
    - `--gpus-per-node=4`: Request 4 GPUs per node
    - `--cpus-per-task=6`: Allocate CPUs for data loading, I/O, etc.
- Options for 4 GPUs, "Multi-GPU / Multi-node":
    - `--nodes=2`: Run across 2 physical nodes
    - `--ntasks-per-node=2`: 2 tasks/processes per node (1 per GPU)
    - `--gpus-per-node=2`: Request 2 GPUs per node
    - `--cpus-per-task=6`: Allocate CPUs for data loading, I/O, etc.

For example:
```bash
srun -p Global \
         --nodes=2 --ntasks-per-node=4 --gpus-per-node=4 \
         --pty \
         --time=00:05:00 \
         bash --norc --noprofile
```

### On the Application Code Side

SLURM automatically sets environment variables for distributed jobs:

- `SLURM_PROCID`: Global process rank
- `SLURM_LOCALID`: GPU index on the node
- `SLURM_NODEID`: Node rank

Your code should use these variables to assign devices and ranks. Example with PyTorch DDP:

```python
import os
import torch
import torch.distributed as dist

def main():
    dist.init_process_group(backend='nccl')  # Use NCCL for GPU communication
    rank = dist.get_rank()
    world_size = dist.get_world_size()
    local_rank = int(os.environ["SLURM_LOCALID"])

    device = torch.device(f"cuda:{local_rank}")
    print(f"Process {rank}/{world_size} using {device}")

    # Initialize your model, wrap with DDP, training loop, etc.

if __name__ == "__main__":
    main()
```

### Optimizing GPU Memory Transfers with NVLINK

Slurm does not natively manage NVLINK topology. If you request `--gpus-per-node=2` on a server with 4 GPUs (paired via NVLINK), Slurm will allocate any 2 available GPUs, regardless of NVLINK connectivity.

- If the selected GPUs are NVLINK-connected, CUDA will use NVLINK by default.
- If not, NVLINK will not be available for your job.

**To ensure NVLINK usage:**
1. Analyze the GPU topology:
   ```bash
   nvidia-smi topo -m
   ```
2. Set `CUDA_VISIBLE_DEVICES` to select NVLINK-connected GPUs:
   ```bash
   export CUDA_VISIBLE_DEVICES=0,1  # if 0 and 1 are connected via NVLINK
   srun --gpus=2 python train.py
   ```

!!! note
    - This approach may increase wait times, as your job will only start when the specific GPUs are free.
    - For best results, use on clusters with multiple servers of identical architecture and GPU layout.



---
## Compiling Code

### C script : demo.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <time.h>

int main() {
    const int iterations = 5;

    printf("Starting computation...\n");

    for (int i = 1; i <= iterations; ++i) {
        printf("Step %d of %d\n", i, iterations);
        sleep(1); // simulate some work
    }

    printf("Computation completed successfully.\n");
    return 0;
}
```

### Interactive mode 

- Open a shell
```bash
srun --pty --cpus-per-task=1 --mem=1G --time=00:20:00 bash --norc --noprofile
```
- Run commands
```bash
gcc demo.c -o demo
```
- And test
```bash
./demo
```

### Batch mode 

- Create the sbatch script : `compil.sbatch`
``` bash
#!/bin/bash
#SBATCH --job-name=compile-c
#SBATCH --output=compile-%j.out
#SBATCH --error=compile-%j.err
#SBATCH --time=00:05:00
#SBATCH --cpus-per-task=1
#SBATCH --mem=1G

# Compile the code
gcc demo.c -o demo
```
- Then submit the job:
``` bash
sbatch compil.sbatch
```
- Display result
```bash
cat compile-*.out
```

---
## [WiP] Working with Matlab 

!!! warning

    [NOT WORKING AT THIS TIME - WORK IN PROGRESS - LICENCE PROBLEM]

### Matlab script : matlab-demo.m

```bash
% matlab-demo.m
disp('Starting MATLAB job...');
A = rand(1000);
B = eig(A);
disp('Job done.');
```

### Interactive mode 

- Open a shell
```bash
srun --pty --cpus-per-task=2 --mem=4G --time=00:20:00 bash --norc --noprofile
```
- And run commands
```bash
export MATLAB="/opt/img/MATLAB_R2024a_u6"
export PATH="$PATH:/opt/img/MATLAB_R2024a_u6/bin"
matlab -r "matlab-demo.m"
```

### Batch mode 

- Create the sbatch script : `run_matlab.sbatch`
``` bash
#!/bin/bash
#SBATCH --job-name=matlab-test
#SBATCH --output=matlab-%j.out
#SBATCH --error=matlab-%j.err
#SBATCH --time=00:10:00
#SBATCH --cpus-per-task=2
#SBATCH --mem=4G

export MATLAB="/opt/img/MATLAB_R2024a_u6"
export PATH=$PATH:/opt/img/MATLAB_R2024a_u6/bin"
srun matlab -nodisplay -nosplash -r "matlab-demo; exit"
```
    - `-nodisplay -nosplash -r "matlab_demo; exit"` : This tells MATLAB to run in CLI mode, without GUI, and execute your script.
    - `%j` in the output/error filenames is replaced by the job ID.

- Then submit the job
``` bash
sbatch run_matlab.sbatch
```
- Display result
```bash
cat matlab-*.out
```

---
## Running One or Multiple MATLAB Jobs Simultaneously

Recommendations: To define the number of tasks to reserve, use 1 task per simulation (or per thread if using a `parfor` loop in your MATLAB code).

The command below will allocate 1 physical core per task, which matches MATLAB's typical execution model.
```bash
salloc --nodelist=sl-mee-br-115 --ntasks=1 --cpus-per-task=1 --mem-per-cpu=XXGB --hint=nomultithread
```

The most important consideration is to allocate enough memory for each task/CPU, but not too much, to avoid unnecessarily pre-empting the entire node.

Start with 16GB per CPU (depending on the machine), and increase to 24GB or 32GB if the job is killed due to insufficient memory.

---
## [WiP] Launching a Distributed Application (MPI) 

!!! warning

    [NOT WORKING AT THIS TIME - WORK IN PROGRESS - MPI Problem]
    ``` bash
    --------------------------------------------------------------------------
    *** An error occurred in MPI_Init
    *** on a NULL communicator
    *** MPI_ERRORS_ARE_FATAL (processes in this communicator will now abort,
    ***    and potentially your MPI job)
    [sl-mee-br-111:3724358] Local abort before MPI_INIT completed completed successfully, but am not able to aggregate error messages, and not able to guarantee that all other processes were killed!
    --------------------------------------------------------------------------
    The application appears to have been direct launched using "srun",
    but OMPI was not built with SLURM's PMI support and therefore cannot
    execute. There are several options for building PMI support under
    SLURM, depending upon the SLURM version you are using:

    version 16.05 or later: you can use SLURM's PMIx support. This
    requires that you configure and build SLURM --with-pmix.

    Versions earlier than 16.05: you must use either SLURM's PMI-1 or
    PMI-2 support. SLURM builds PMI-1 by default, or you can manually
    install PMI-2. You must then build Open MPI using --with-pmi pointing
    to the SLURM PMI library location.

    Please configure as appropriate and try again.
    --------------------------------------------------------------------------
    ```

### Mpi application

- Create the code file `mpi_hello.c`
``` bash
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int world_size;
    MPI_Comm_size(MPI_COMM_WORLD, &world_size); // Total processes

    int world_rank;
    MPI_Comm_rank(MPI_COMM_WORLD, &world_rank); // Rank of this process

    printf("Hello from process %d of %d\n", world_rank, world_size);

    MPI_Finalize();
    return 0;
}
```
- And compile the program
``` bash
mpicc mpi_hello.c -o mpi_hello
```


### Interactive mode 

- Open a shell
```bash
srun --pty \
     --cpus-per-task=1 \
     --nodes=2 \
     --ntasks-per-node=2 \
     --mem=1G \
     --time=00:10:00 \
     bash --norc --noprofile
```
- Run commands
```bash
./mpi_hello
```

This exemple run 4 processes : it will run across 2 nodes (`--nodes=2`), and launch 2 MPI ranks per node  `--ntasks-per-node=2`


### Batch mode 

- Create the sbatch script : `run_mpi.sbatch`
``` bash
#!/bin/bash
#SBATCH --job-name=mpi-job
#SBATCH --output=mpi-out-%j.txt
#SBATCH --error=mpi-err-%j.txt
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=4
#SBATCH --time=00:10:00
#SBATCH --mem=1G

# Run the application
srun ./mpi_hello
```
- Then submit the job:
``` bash
sbatch run_mpi.sbatch
```
- Display result
```bash
cat notebook-*.out
```

This exemple run 8 process : it will run across 2 nodes (`--nodes=2`), and launch 4 MPI ranks per node  `--ntasks-per-node=4`
