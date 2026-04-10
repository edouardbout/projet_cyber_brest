# Slurm Usage Tips

---
## SSH Connection to Allocated Resources

By default, you cannot connect via SSH to Slurm-managed compute nodes. However, once resources are allocated using `srun`, `salloc` or `sbatch`, you can SSH into the server where your resources are reserved.

![SSH connection to a compute node](../img/mee-hpc/mee-HPC-slurm-cluster-ssh-job-running.png#center#shadow)

To find out which server your resources are allocated on, use the `squeue` command.

The SSH session will be automatically terminated when the allocation ends.

**Workflow:**

1. Reserve resources:
   ```bash
   salloc
   # salloc: Granted job allocation 11440
   # salloc: Nodes sl-mee-br-111 are ready for job
   ```
2. Check the allocated server:
   
   ```bash
   squeue
   # JOBID  PARTITION  NAME      USER  ST  TIME  NODES  NODELIST
   # 11440  Mee        interact  demo  R   0:31  1      sl-mee-br-111
   ```
3. Connect via SSH:
   ```bash
   ssh demo@sl-mee-br-111.imta.fr
   ```

!!! info

    In this case, your home directory is mounted.

---
## Displaying the **salloc** Session in Your Prompt

When you use `salloc`, it opens a new shell session. The `$SLURM_JOB_ID` environment variable contains the job allocation ID inside a **salloc** session and is empty otherwise. This helps you check if you are currently in an allocated session.

A recommended practice is to customize your shell prompt (`PS1`) to display the job ID, so you can easily see when you are working inside a Slurm allocation.

Example:
```bash
export PS1='${debian_chroot:+($debian_chroot)}\[\033[1;37;46m\]\u@\h $SLURM_JOB_ID \[\033[00m\]\[\033[90;47m\] \W >\[\033[00m\] '
```
You can define the `PS1` variable in your `.profile.PERSO` file in your home directory.


---
## Improving Your SSH Experience

Customizing your SSH client configuration can make connecting to Slurm nodes faster and more reliable, especially when working over VPN or unstable networks.

### Prevent SSH Timeouts (Recommended for VPN Users)

By default, SSH connections may drop after a few minutes of inactivity—common when using a VPN. To keep your sessions alive, add this to the top of your `~/.ssh/config` file:

```ssh
Host *
    ServerAliveInterval 300
```

> This setting sends a keepalive packet every 5 minutes, preventing disconnections due to inactivity.

### Define Short Aliases for Login Nodes

You can simplify SSH commands by creating aliases for frequently used login nodes. Add the following to your `~/.ssh/config` (replace `<YOUR-LOGIN>` with your actual username):

```ssh
Host slurm slurm-login-101
    HostName sl-mee-br-101.imta.fr
    User <YOUR-LOGIN>

Host slurm-login-116
    HostName sl-mee-br-116.imta.fr
    User <YOUR-LOGIN>
```

Now, you can connect with simple commands:

```bash
ssh slurm
ssh slurm-login-101
ssh slurm-login-116
```

> Using aliases saves time and reduces typing errors when connecting to different nodes.

For more advanced SSH configuration tips, see the [SSH/X2go access to servers](./connexion.md).


---
## Avoiding Interactive Session Interruptions

### Why Use a Session Manager?

When you start an interactive session with `salloc`, it opens a new shell. If you disconnect (e.g., close your SSH window or lose your connection), your resource allocation is immediately terminated.

To keep your session alive even if you disconnect, use a terminal multiplexer like `tmux` or `screen`. This allows your `salloc` session to persist in the background, so you can reconnect and resume your work at any time.

!!! warning
    For long-running or unattended jobs, always use `sbatch` instead of `salloc`.

### Keeping Your Session Alive with Tmux

**Recommended workflow:**

1. **Connect to a login node:**
   ```bash
   ssh <YOUR-LOGIN>@sl-mee-br-101.imta.fr
   ```
2. **Start a new tmux session:**
   ```bash
   tmux new -s SlurmAllocation
   ```
3. **Request a resource allocation:**
   ```bash
   salloc --mem-per-cpu=2G
   ```

**To temporarily disconnect (detach) from tmux:**
- Press `Ctrl+b`, then `d` (release both keys).
- You can now safely close your SSH session with `exit`.

**To reconnect to your running session:**
1. SSH back to the login node.
2. Reattach your tmux session:
   - If you have only one session:
     ```bash
     tmux attach
     ```
   - If you have multiple sessions:
     1. List sessions:
        ```bash
        tmux ls
        ```
     2. Attach to a specific session:
        ```bash
        tmux attach -t session_name
        ```

**To end (not just detach) your tmux session:**
- Type `exit` inside the tmux shell.

For more details, see the [Tmux guide](./tmux.md).


---
## Using Multiple `salloc` Sessions in Slurm

You can launch several interactive sessions with `salloc` at the same time, each in a separate terminal, or Tmux session on the login node. Every `salloc` command creates an independent resource allocation and opens a shell on the assigned compute node(s).

**Key Points**

- **Resource limits and quotas** (CPUs, GPUs, memory, number of jobs) apply across all your allocations. Exceeding your quota or cluster limits will cause new requests to wait in the queue.
- **Resource availability:** Each `salloc` session draws from the cluster’s shared pool. If resources are unavailable, your session will be queued until they are free.

**Examples**

- Session 1 – For a long-running job:
    ```bash
    salloc --ntasks=16 --time=04:00:00 --partition=<TEAMS-PARTITION>
    ```
- Session 2 – For quick testing:
    ```bash
    salloc --ntasks=1 --time=00:10:00 --partition=Global
    ```
- To check the status of your allocations and jobs:
    ```bash
    squeue -u $USER
    ```

**Best Practices**

- Use the smallest resource requests possible for testing to minimize wait times.
- Prefer `sbatch` for long, non-interactive jobs.
- Monitor your usage to avoid exceeding cluster policies or blocking other users.
- Release resources promptly when finished by typing `exit` in the session or running `scancel <jobid>`.


---
## Efficiently Transferring Large Datasets

### Why Use `rsync`?

Transferring large datasets with `cp` or `scp` is often inefficient, especially if the transfer is interrupted. `rsync` is recommended because it:

- Transfers only changed parts of files, saving time and bandwidth
- Can resume interrupted transfers
- Preserves permissions, symlinks, and timestamps
- Provides detailed progress information

> For best results, always use `rsync` for large or important data transfers. Avoid `cp` or `scp` for anything but small, non-critical files.

### Smart Data Transfer: Home Directory ↔ Minos Shared Storage

**Storage overview in Slurm:**

- **Home directory** (`$HOME`): Accessible from your workstation and Slurm login nodes only
- **Minos shared storage** (`/Odyssey`, `/Codes`, ...): Accessible from Slurm login and compute nodes, but **not** from your workstation
- *(Coming soon)* **Transfer node**: Will allow direct uploads from outside to shared storage

**Transferring between Home and Shared Storage (on a login node):**

- From home directory to shared storage:
  ```bash
  rsync -avh ~/myfolder/ /Odyssey/private/<YOUR-LOGIN>/myfolder/
  ```
- From shared storage to home directory:
  ```bash
  rsync -avh /Odyssey/private/<YOUR-LOGIN>/myfolder/ ~/myfolder/
  ```

**Key options:**

- `-a`: archive mode (preserves permissions, symlinks, timestamps)
- `-v`: verbose output
- `-h`: human-readable sizes

### (Future) Using the Transfer Node (External to Minos Shared Storage)

Once available, the transfer node will let you upload/download directly between your workstation and Minos shared storage:

- From your workstation to Minos shared storage:
  ```bash
  rsync -avh ./myfolder/ <YOUR-LOGIN>@<TRANSFER-NODE>:/Odyssey/private/<YOUR-LOGIN>/myfolder/
  ```
- From shared storage to your workstation:
  ```bash
  rsync -avh <YOUR-LOGIN>@<TRANSFER-NODE>:/Odyssey/private/<YOUR-LOGIN>/myfolder/ ./myfolder/
  ```


---
## Using VS Code for Remote Development

With the [Remote SSH extension](https://code.visualstudio.com/docs/remote/ssh), VScode can connect directly to a compute node via SSH, allowing you to edit files and run code as if you were working locally.

**How to connect:**
1. Open VScode and press `Ctrl + Shift + P` to open the command palette.
2. Run *"Remote-SSH: Add New SSH Host..."* and enter the SSH command for your compute node:
   ```bash
   ssh <YOUR-LOGIN>@<COMPUTE-NODE>.imta.fr
   ```
3. VS Code will save this configuration. Next time, use *"Remote-SSH: Connect to Host..."* and select your node from the list.

**Jupyter Notebooks in VS Code:**
- If you have installed the [Jupyter extension](https://github.com/microsoft/vscode-jupyter), you can work with Jupyter notebooks directly inside VScode.
- To access a Jupyter server running on the compute node, set up an SSH tunnel:
   ```bash
   ssh -L 8888:localhost:8888 <YOUR-LOGIN>@<COMPUTE-NODE>.imta.fr
   ```
- Then open `http://localhost:8888` in your browser or connect via the VS Code Jupyter extension.

*for more détails, see [Runnig a Jupyter Notebook](./slurm-common-usage.md#running-a-jupyter-notebook)*

---
## When Should You Use `srun` in an `sbatch` Script?

In a SLURM batch script, **you are not required to prefix every command with `srun`**.
However, knowing **when to use it** is important for correct resource usage and job accounting.

### Use `srun` for:

-  Running the **main compute task** of your job
-  Any **program that should run using SLURM-assigned resources**
-  **Parallel jobs** (e.g., MPI, job arrays, or multiple tasks)
-  Ensuring **proper tracking** in SLURM’s job accounting

```bash
#!/bin/bash
#SBATCH --job-name=myjob
#SBATCH --cpus-per-task=4
#SBATCH --mem=8G
#SBATCH --time=00:10:00

SETUP SETUP MATLAB2024a
srun matlab -nodisplay -nosplash -r "my_script; exit"
```

###  Do NOT use `srun` for:

- Simple shell commands (`cd`, `echo`, `module load`, `SETUP`, etc.)
- Pre- and post-processing steps that do not require compute resources
- Sequential scripts where no parallel execution is needed

```bash
#!/bin/bash
#SBATCH --job-name=simple
#SBATCH --time=00:05:00

echo "Starting script..."
module load python
python myscript.py
echo "Done."
```

> SLURM already runs your batch script **within the resource allocation**, so a single non-`srun` job will still use the assigned resources.


### Summary Table

| Use Case                                | Should You Use `srun`? | Reason                                      |
| --------------------------------------- | ---------------------- | ------------------------------------------- |
| Main compute job (e.g., MATLAB, Python) | Yes                  | Ensures the job runs with allocated CPUs    |
| One-liner shell commands                | No                   | Not resource-intensive                      |
| Loading modules or setting environment  | No                   | Runs instantly, doesn’t need scheduling     |
| MPI or parallel jobs                    | Yes (for each task)  | Needed for distributed task management      |
| Job scripts with a single command       | Optional            | Works without `srun`, but `srun` is cleaner |


### Best Practice

- Use `srun` for your **main program**, especially if it’s CPU- or memory-intensive.
- Avoid `srun` for auxiliary commands to keep your script clean and efficient.



