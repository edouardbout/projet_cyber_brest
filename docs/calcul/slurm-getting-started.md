# Quick Start with Slurm

For detailed information about commands, see *[Main Slurm Commands](./slurm-commands.md)*

---
## Connecting to the MEE Slurm Cluster

You must run Slurm commands from a login node, not your personal computer. Use SSH to connect to one of the Slurm login nodes: `sl-mee-br-101.imta.fr` or `sl-mee-br-116.imta.fr`.

Connect to a login node via [SSH](connexion.md):
```bash
ssh YOUR-LOGIN@sl-mee-br-101.imta.fr
```
*Replace `YOUR-LOGIN` with your IMT Atlantique login*

![Connection to the login node sl-mee-br-101.imta.fr](../img/mee-hpc/mee-HPC-slurm-job-submisson.png#center#shadow)

!!! warning

    **IMPORTANT:** Never run heavy workloads directly on the login node. Always use Slurm commands (`srun`, `sbatch`, or `salloc`) to submit your jobs to the compute nodes. Running intensive tasks on the login node can disrupt other users and may result in your session being terminated.

---

## Running interactive `salloc` sessions (the quickest way to use Minos)

To overcome the constraints of IMT Atlantique IT infrastructure, and work both "like the old way" and with *Slurm*, use the `salloc` command, then `ssh` to the allocated server(s). `salloc` grants an interactive allocation with the requested computing resources. Then the `ssh` gives access to the servers where the allocated resources are. The ssh session is tied to the existence of the `salloc` allocation (`pam_slurm_adopt` mechanism). Thus it should be kept alive to keep the job running.

For best practice, follow the guidelines:

1. Open a terminal, connect with `ssh` on a login node (sl-mee-br-101 or sl-mee-br-116)
2. Use `tmux` to create a detachable session on the login node so you can disconnect or avoid network failure problems that would interrupt the allocation
3. Inside the `tmux` session, use `salloc` with [the options](#salloc-options) to get a resource allocation that suits your needs
4. Open another terminal, connect with `ssh` to the server on which the allocated resources are, and use `tmux` as well to create a detachable session
5. Setup your environment and launch your job.
6. When your job is finished, quit the tmux session, your ssh session
7. Terminate your allocation either by quitting the corresponding tmux/ssh session or by using the `scancel` command.

see [Tmux Documentation](./tmux.md){:target="_blank"}

### `salloc` basic options

| Options            | Description                                                                                                     |
|--------------------|-----------------------------------------------------------------------------------------------------------------|
| `-p`               | select a partition (group of machines on which jobs are queued)                                                 |
| `--ntasks=`        | number of tasks (usually and by default 1)                                                                      |
| `gres=`            | generic resources, used for example to allocate 2 A100 GPU, `gres=gpu:a100:2`                                   |
| `--cpus-per-task=` | number of CPU core per task (optional if `gres=gpu:x:x` is defined, there is default `cpus-per-gpu`)            |
| `--mem-per-cpu=`   | RAM per CPU core (optional if `gres=gpu:x:x` or `--cpus-per-task=` are defined, there is default `mem-per-cpu`) |


### Examples of `salloc` commands

Get a single CPU on the default partition (`Mee`):

```bash
salloc
```

Get a NVidia **L40S** GPU on the Odyssey servers:
```bash
salloc -p Odyssey --gres=gpu:l40s:1
```

Get a full server from its name:

```bash
salloc -p Global --nodelist=sl-mee-br-310
```



---
## Running Jobs on the Cluster with `srun`

!!! warning
    Slurm compute nodes don't have access to your IMT Atlantique home directory (`/homes/username`). To avoid errors like: `srun: error: unable to cd to /home/username: No such file or directory`, always run your jobs from a folder that exists on the compute servers, such as `/tmp` or a MINOS shared volume. *See [MEE Storage servers](./storage.md)*

- Move to a MINOS shared volume (`/Odyssey`, `/Codes`, `/Brain`) based on your team :
```bash
cd /Odyssey
```

- Launch a job, for example, the Unix command `hostname` to display the name of the server where the shell is running:
```bash
srun hostname
```
Slurm will select a server and execute your job. The command returns the name of the chosen server.




---
## Opening an Interactive Terminal

!!! warning
    Slurm compute nodes cannot access your session initialization files (stored in your IMT Atlantique home directory `/homes/username`). To avoid errors like: `bash: /homes/ebraux/.bashrc: Permission denied`, disable loading of initialization files with `--norc --noprofile` options.

- Open a terminal:
```bash
srun --pty bash --norc --noprofile
# bash-5.2$
```
A new terminal opens on the remote server. To check, use the `hostname` command again.

You can run Python commands, e.g.:
```bash
python3 -c "print('Hello SLURM')"
```

You can also check available storage spaces:

- IMT Atlantique homedir: `ls /homes/`
- MINOS shared volumes: `ls /Odyssey`, `ls /Brain`, ...
- `/tmp` folder: `ls /tmp`

To exit, use:
```bash
exit
```

---
## Specifying Job Resources

By default, Slurm uses preset values. For more details, see [Slurm default values](./slurm-default-values.md).

It's recommended to specify the resources your job needs. For example:
```bash
srun --pty \
     --cpus-per-task=2 \
     --mem=4G \
     --time=00:10:00 \
     bash --norc --noprofile
```

- `--cpus-per-task=2`: Allocates 2 CPU cores
- `--mem=4G`: Allocates 4 GB RAM
- `--time=00:10:00`: Limits job duration to 10 minutes

*See more options in [Run interactive job with `srun`](./slurm-commands.md#run-interactive-job-with-srun)*

Slurm attempts to allocate a compute node that matches your requested resources. If your requirements exceed what is available, Slurm cannot find a suitable node and your command will fail. For example:
``` bash
srun --mem=400G hostname
# srun: error: Memory specification can not be satisfied
# srun: error: Unable to allocate resources: Requested node configuration is not available
```

Test the time limit with:
```bash
time srun --time=00:01:00 sleep 300
```

- Linux commands:
    - `time`: shows how long a command takes to execute
    - `sleep 300`: does nothing for 5 minutes
- srun option:
    - `--time=00:01:00`: limits the job duration to 1 minute


---
## Running Jobs in Batch (Non-Interactive) Mode

Batch mode lets you run jobs automatically, without an interactive terminal. You describe your job and its environment in an **sbatch script** (a text file), containing Slurm options (lines starting with `#SBATCH`) and commands to be executed.

If resources are available, `sbatch` launches the tasks. Otherwise, it queues the request.

Example with this `srun` Command :
```bash
srun --job-name=test \
    --cpus-per-task=1 \
    --mem=1G \
    --time=00:00:10 \
    hostname
```

The following `sbatch` script runs the same job, but in batch mode:
```bash
#!/bin/bash
#SBATCH --job-name=test
#SBATCH --cpus-per-task=1
#SBATCH --mem=1G
#SBATCH --time=00:00:10
#SBATCH --output=test.out

srun hostname
```

- Save this script as `test.sbatch`.
- Submit your job with :
```bash
sbatch test.sbatch
# Submitted batch job 11304
```

Batch mode is non-interactive. It does not open a terminal, your job runs in background :

- The `--pty` option is not supported in batch mode.
- Output has to be saved to the file specified by `#SBATCH --output=test.out`.

Batch scripts are useful for automating jobs and running them without an interactive session.

For more details, see [Slurm batch jobs](./slurm-commands.md#running-non-interactive-batch-jobs-with-sbatch).

---
## Managing Jobs

- List jobs in progress (RUNNING, PENDING, etc):
    - All users:
    ```bash
    squeue
    ```
    - Your jobs only:
    ```bash
    squeue --me
    ```

- Cancel jobs :
    - Delete a job, whether it is running or in the queue, use the `scancel` command with the job ids to cancel :
    ```bash
    scancel jobid1 jobid2 ... jobidN
    ```
    - Dancel all your jobs:
    ```bash
    scancel -u $USER
    ```

---
## Reserving Resources for Interactive Environments

If you need to run multiple tasks or commands in a unique context, reserve resources with `salloc`:

- `srun`: requests resources, runs the command, then releases resources
- `salloc`: requests resources and keeps them reserved for your session. It doesn't run any command.

Once resources are allocated, you can run commands interactively with `srun`.
You can also SSH to the server(s) where resources are allocated.

Resource allocation only happens if the requested resources are available.

Example:

- Request resource allocation:
```bash
salloc --job-name=test-env \
       --time=10:00:00 \
       --cpus-per-task=1 \
       --mem=1G
# salloc: Granted job allocation 11494
# salloc: Nodes sl-mee-br-112 are ready for job
```
    - *The job ID (11494) is only useful if you want to track what is running in the session later.*
    - *The node name (sl-mee-br-112) can be used to connect via SSH.*
- Check allocation:
```bash
squeue
# JOBID  PARTITION  NAME      USER  ST  TIME  NODES NODELIST(REASON)
# 11494  Mee        test-env  demo  R   1:15  1     sl-mee-br-112
```
- Run job with srun:
```bash
srun --job-name=test hostname
# sl-mee-br-112
```
- SSH to the server:
```bash
ssh $USER@sl-mee-br-112
```

To ensure the job runs in the allocated environment, use `--verbose`:
```bash
srun --job-name=test --verbose hostname
# srun: defined options
# ...
# srun: jobid               : 11494
# srun: job-name            : test
# ...
# sl-mee-br-112
```

Each `srun` line in an allocation context is a *jobstep* with its own `JobID`, indexed relative to the allocation job ID: `jobid.stepid`.

```bash
sacct -u $USER --starttime today --format=JobID,JobName,State,Elapsed,AllocCPUs,NodeList
# JobID           JobName      State    Elapsed  AllocCPUS        NodeList
# ------------ ---------- ---------- ---------- ---------- ---------------
# 11494          test-env    RUNNING   00:10:26        1   sl-mee-br-112
# 11494.extern     extern    RUNNING   00:10:26        1   sl-mee-br-112
# 11494.0            test  COMPLETED   00:00:01        1   sl-mee-br-112
# 11494.1            test  COMPLETED   00:00:00        1   sl-mee-br-112
# 11494.3            test    RUNNING   00:00:05        1   sl-mee-br-112
```

!!! note

        Ressources can be allocated on multiple compute nodes

!!! warning

    If you reserve a node for interactive use (for example, with a 10-hour time limit) but finish your work early, be sure to **release the resources as soon as you are done**. This helps make resources available for other users.

    To end your allocation, simply type:
    ```bash
    exit
    ```
    After exiting, you can verify that your resources have been released with:
    ```bash
    squeue -u $USER
    ```
    If no jobs are listed, your allocation has ended and the node is free for others.

---
## Using specific Ressources

By default, `srun`, `sbatch`, and `salloc` use the default partition, which has limited resources. If you need more resources or access to specialized hardware, specify a different partition using the `-p` option.

For example, to run a job on the `Odyssey` partition:
```bash
srun -p Odyssey --job-name=test hostname
```

*Replace `Odyssey` with the partition that best fits your needs (e.g., `Brain`, `Codes`).*

Specifying the partition ensures your job runs where the required resources are available.

Useful commands :

- Show available partitions:
```bash
sinfo
```
*The default partition is marked by a `*`.*

- Show partitions you can access to:
```bash
sacctmgr show assoc user=$USER format=Account,Partition
```
*If no partition is displayed, you potentially have access to all partitions, since in SLURM, access is open by default unless restricted.*
- List servers for a specific partition:
```bash
sinfo -N -p Odyssey -o "%N %c %m %G %t" | column -t
```
- List servers with a GPU:
```bash
sinfo -N -o "%N %G" | grep -v '(null)' | column -t
```
