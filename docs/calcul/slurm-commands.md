# Main Slurm Commands

---
## Essential Commands

### Summary Table

| Command    | Purpose                       | Useful For                     | Typical Usage                        |
| ---------- | ----------------------------- | ------------------------------ | ------------------------------------ |
| `sinfo`    | Show node/partition status    | Cluster overview               | `sinfo`, `sinfo -N -l`, `sinfo -p gpu` |
| `scontrol` | Show detailed job/node info   | Debugging jobs, resource check | `scontrol show job <id>`, `scontrol show node` |
| `sacct`    | View job history and usage    | Job results, resource tracking | `sacct -u $USER`, `sacct -j <id>`    |
| `sacctmgr` | View account/project settings | Knowing your SLURM account     | `sacctmgr show user`, `list associations` |


### `sinfo` - Cluster Partition and Node Status

Displays the state of partitions and compute nodes. Use it to check which partitions (queues) are available, node status (idle/busy), and features (GPU, memory).

Documentation: [https://slurm.schedmd.com/sinfo.html](https://slurm.schedmd.com/sinfo.html)

**Examples:**
```bash
sinfo                      # Basic status of all partitions
sinfo -N -l                # Long format, per node details
sinfo -p Mee               # Status for the 'Mee' partition
```
**Use when:**

- Checking node/partition availability

### `scontrol` - Detailed Job/Node Info

View or modify SLURM objects (jobs, nodes, partitions). As a user, mainly for inspecting jobs and nodes.

Documentation: [https://slurm.schedmd.com/scontrol.html](https://slurm.schedmd.com/scontrol.html)

**Examples:**
```bash
scontrol show job <jobid>       # Full info for a job
scontrol show node <nodename>   # Details of a compute node
```

**Use when:**

- Debugging jobs or checking resource use

### `sacct` - Job Accounting and History

Shows info about current and past jobs: resource usage, run time, exit code, etc. Useful for performance analysis and debugging.

Documentation: [https://slurm.schedmd.com/sacct.html](https://slurm.schedmd.com/sacct.html)

**Examples:**
```bash
sacct -u $USER --starttime today
sacct -j <jobid> --format=JobID,JobName%-20,Elapsed,State,ExitCode
```
`%-N` allows you to display a field on `N` characters. Available fields are visible with the command `sacct -e`
  
**Use it when:**

- Checking if a job finished successfully
- Knowing how much CPU/memory your job used
- keeping track of your compute usage



### `sacctmgr` - View accounting settings (limited for users)

Mainly for admins, but users can view account names and associations.

Documentation: [https://slurm.schedmd.com/sacctmgr.html](https://slurm.schedmd.com/sacctmgr.html)

**Examples:**
```bash
sacctmgr show user where name=$USER
sacctmgr list associations user=$USER
```

**Use when:**

- Specifying your account in a job (--account=...)
- Checking your associated accounts/projects


---
## Account Information

**Overview**

SLURM manages:

- `user`: corresponds to the UNIX user
- `group`: corresponds to the UNIX group
- `account`: Slurm account, more global, corresponds to a project, a team, a department, ...

The `account` is central for resource management, quotas, priorities, and billing. Users/groups are attached to accounts, which control access to partitions and quotas.

**List accounts:**
```bash
sacctmgr show account
```

**Show your associations:**
```bash
sacctmgr show assoc user=$USER format=User,Account
```

**Check your user limits**
```bash
sacctmgr show user $USER format=User,GrpTRES,MaxTRES,MaxJobs
```

**Other useful commands:**

- Show all associations and rights:
  ```bash
  sacctmgr show assoc user=$USER
  ```
- Basic account info:
  ```bash
  sacctmgr show user $USER
  ```
- Accounts attached to you:
  ```bash
  sacctmgr show user $USER format=User,DefaultAccount,Account
  ```
- Partitions you can access:
  ```bash
  sacctmgr show user $USER format=User,Partition
  ```
- All SLURM rules and quotas:
  ```bash
  scontrol show assoc user=$USER
  ```

---
## Partition Information

**Display partition informations**
```bash
sinfo
```
This gives for example:
```
PARTITION     AVAIL  TIMELIMIT  NODES  STATE NODELIST
Global           up 1-00:00:00     20    mix sl-mee-br-[109,111-115,203,206,208,210-211,401],sl-tp-br-[513,515-516,518,521-524]
Global           up 1-00:00:00      1  alloc sl-mee-br-311
Global           up 1-00:00:00     42   idle sl-mee-br-[106,119,202,204-205,207,209,212-213,310,312-328,330,332-340,501],sl-tp-br-[514,517,519-520]
Mee*             up   infinite      6    mix sl-mee-br-[109,111-115]
MeeGPU           up 3-00:00:00      1   idle sl-mee-br-106
...
```
*The default partition is marked by a `*`.*

**Simple list of partitions**

List only the names of available partitions:
```bash
sinfo -h -o "%P"
```

- `-h`: removes the header
- `-o "%P"`: formats the output to display only the partition name (`%P`). Useful options
    - `%P`: partition name
    - `%D`: number of nodes in the partition
    - `%a`: availability (`up` or `down`)
    - `%l`: time limit
    - `%F`: associated nodes

**Show availability:**
```bash
sinfo -o "%P %a"
```

**Show all partitions, more detailed:**
```bash
scontrol show partitions
```
*Information include: `Nodes`, `State`, `AllowGroups`,  ...*

**Show default partition:**
```bash
scontrol show partition | grep -E 'PartitionName=|Default=YES'
# PartitionName=Global
# PartitionName=Mee
#    AllocNodes=ALL Default=YES QoS=N/A
# PartitionName=MeeGPU
# ...
```

**Display only specific information:**
```bash
scontrol show partitions | egrep "PartitionName=|State=|AllowGroups=|Nodes"
```

**Show info for a given partition:**
```bash
scontrol show partition PARTITION-NAME
```


**Show available nodes:**
```bash
sinfo -o "%P %D %a %l"
```

**Show partition access (by user/group/account):**
```bash
scontrol show partitions | egrep "PartitionName=|AllowGroups=|AllowUsers=|AllowAccounts="
```

---
## Job Information

**List jobs in progress:**
```bash
squeue
squeue --me
squeue -u $USER
```

**Show job history:**
```bash
sacct -u $USER --starttime today --format=JobID,JobName,State,Elapsed,AllocCPUs,NodeList
sacct -o jobid,jobname,AllocNodes,NCPUS,ReqMem,CPUTime,user,Partition,NTasks,State
```

**Show detailed job info:**
```bash
scontrol show jobs
scontrol show job jobid
```

**Quickly view allocated resources:**
```bash
squeue --me --format="%.10i %.20j %.8T %.10M %.6D %.15R"
```
- `%.10i`: JobID
- `%.20j`: JobName
- `%.8T`: Job state
- `%.10M`: Time used
- `%.6D`: Number of nodes
- `%.15R`: Reason or node

---
## Node Information

**Show all node details:**
```bash
scontrol show nodes
```
Detailed information for each node, including: `NodeName`, `CPUTot`, `CPULoad`, `RealMemory`, `AllocMem`, `Gres` (for GPUs), `State`, etc.
- ...

**Display only specific information:**
```bash
scontrol show nodes | egrep "NodeName=|CPUTot=|RealMemory=|Gres=|State="
```

**Display information for a given node only**
```bash
scontrol show node NODE-NAME
```

**List nodes with formatting:**

```bash
sinfo -N -o "%N %P %c %m %G %t" | column -t
```
Formatting options are:

- `%N`: node name
- `%c`: total number of CPUs
- `%m`: memory (in MB)
- `%G`: generic resources (e.g., GPU)
- `%t`: node state (idle, alloc, etc.)
- `%P`: Partition to which the node belongs

And use the `column` command to make the output more readable.


**List GPU nodes:**

If GPUs are available on a node, they appear in the `Gres` column. For example:

- `gpu:rtx3080:1`: 1 RTX3080 GPU card is available on this node.
- `gpu:h100:2`: 2 H100 GPU cards are available on this node.

For example, to list servers with a GPU:
```bash
sinfo -o "%N %G" | grep -v '(null)'| column -t
sinfo -N -o "%N %G" | grep -v '(null)'| column -t
```

**Node states:**

| STATE    | Meaning                                                         |
| -------  | ----------------------------------------------------------------|
| `idle`   | Node available: no job running, ready to execute a job.         |
| `alloc`  | Allocated: all resources used by jobs.                          |
| `mix`    | Mixed: some resources used, others available.                   |
| `down`   | Out of service: node inactive, hardware/manual issue.           |
| `drain`  | Draining: finishing jobs, no new jobs started (maintenance).    |
| `drng`   | Draining in progress.                                           |
| `comp`   | Completed: node finished a job, not yet restarted/reconfigured. |
| `resv`   | Reserved: node reserved for specific use.                       |
| `maint`  | Maintenance: non-production use.                                |
| `fail`   | Failed: node marked as failed (communication or other errors).  |

**Show nodes for a partition:**
```bash
sinfo -N -p Global -o "%N %c %m %G %t" | column -t
```

---
## Cluster Information

**Show cluster, partition, and node info:**
```bash
sinfo
```
**Show detailed info by partition and node type:**
```bash
sinfo -o "%15R %15C %10m %G %d"
```
**List all nodes sorted by partition:**
```bash
sinfo  -e -N -o "%15P %10a %20N  %10T %10c %8O  %10m %10d  %25f  %25G " | sort
```

**Show all node details:**
```bash
scontrol show nodes
scontrol show node sl-mee-br-102
```
**Show cluster configuration:**
```bash
scontrol show config
```

---
## Running Interactive Jobs with `srun`

`srun` must be used from a Slurm login node.
!!! warning
    Interactive mode is suitable for debugging and development. Limit allocated resources to the strict minimum.

- Change working directory:
```bash
srun --chdir=/scratch/username --pty bash --norc --noprofile
```

**Common options:**

| Option                 | Description                              |
| ---------------------- | ---------------------------------------- |
| `--job-name=<name>`    | Custom job name (visible in queue/logs)  |
| `--output=<file>`      | Write stdout to file                     |
| `--error=<file>`       | Write stderr to file                     |
| `--time=HH:MM:SS`      | Max wall time                            |
| `--mem=<amount>`       | Total RAM (e.g., `4G`, `800M`)           |
| `--cpus-per-task=N`    | CPU cores per task                       |
| `--ntasks=N`           | Number of parallel tasks                 |
| `--nodes=N`            | Number of compute nodes                  |
| `--partition=<name>`   | Submit to specific partition             |
| `--chdir=<path>`       | Change working directory                 |
| `--gres=gpu:N`         | Request N GPUs per node                  |
| `--account=<name>`     | Project/account for compute time         |
| `--mail-type=END,FAIL` | Email on job completion/failure          |
| `--mail-user=<email>`  | Email address for notifications          |
| `--pty`                | Run interactive shell                    |

Full list of `srun` options: [https://slurm.schedmd.com/srun.html](https://slurm.schedmd.com/srun.html)

---
## Reserving Resources with `salloc`

`salloc` must be used from a Slurm login node.

!!! warning

    Interactive mode is suitable for debugging and development. Limit allocated resources to the strict minimum.

**Examples:**

- Basic allocation
```bash
salloc
```

- Example of resource allocation for interactive use, with GPU, if you are in Odyssey Team
```bash
salloc -p Odyssey --cpus-per-gpu=1 --mem-per-cpu=8G --gres=gpu:rtx8000:1
```

Full list of SLURM `salloc` options:  [https://slurm.schedmd.com/salloc.html](https://slurm.schedmd.com/salloc.html)

---
## Running Non-Interactive Batch Jobs with `sbatch`

With `sbatch`, jobs are managed by SLURM. No need for a session manager as with `salloc`. Must be used from a Slurm login node.

`sbatch` must be used from one of the Slurm login nodes.

**Example sbatch script (`demo.sbatch`):**

Getting informations about a server :

- Write `srv-info.sbatch` file
```bash
#!/bin/bash
#SBATCH --partition=Global               # Partition name
#SBATCH --gres=gpu:rtx8000:1             # GPU request
#SBATCH --job-name=Get-GPU-infos         # Job name

srun sleep 10     # Wait 10 seconds, to be sure the session is OK
srun hostname     # Display server hostname
srun nvidia-smi   # Display Nvidia driver and GPU info
```
- Submit with:
```bash
sbatch srv-info.sbatch
```

By default, a Slurm job will run in the directory from which the `sbatch` command was submitted. This can cause issues if you use relative paths for output files (e.g., `#SBATCH --output=myscript.out`) or to launch a job (e.g., `srun python3 ./myscript.py`), and that directory does not exist on the compute node (such as your IMT Atlantique home directory).

The Minos shared spaces are available on all compute nodes. It is therefore recommended to submit your `sbatch` jobs from these shared spaces.

You can also explicitly set the working directory using `#SBATCH --workdir=/Odyssey/private/<YOUR-LOGIN>`.

Full list of `sbatch` options: [https://slurm.schedmd.com/sbatch.html](https://slurm.schedmd.com/sbatch.html)