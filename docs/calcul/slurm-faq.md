# Slurm FAQ

---
## Why do I get "Access denied by pam_slurm_adopt: you have no active jobs on this node" when connecting via SSH?

```bash
ssh demo@sl-mee-br-106.imta.fr
demo@sl-mee-br-106.imta.fr's password:
Access denied by pam_slurm_adopt: you have no active jobs on this node
Connection closed by 10.129.163.226 port 22
```

Direct SSH access to compute nodes is not allowed unless you have an active resource allocation (e.g., via the `salloc` command). Always request resources before connecting.

---
## Why does `srun` fail with `couldn't chdir to xxx : Permission denied: going to /tmp instead`?

This error occurs if the working directory (`xxx`) does not exist on the compute node. For example, this can happen if you launch `srun` from a folder in your IMT Atlantique homedir that is not mounted on the node.

```bash
srun hostname
slurmstepd-sl-mee-br-111: error: couldn't chdir to `/homes/my-login/Documents/SLURM': Permission denied: going to /tmp instead
```

To avoid this, always run `srun` from a directory that exists on the compute node. Recommended locations include a MINOS shared volume (e.g., `/Odyssey`, `/Brain`) or system folders like `/tmp`.

---
## Why does `srun --pty bash` fail with `bash: /homes/my-login/.bashrc: Permission denied`?

When using the `--pty` option, `srun` starts an interactive shell and attempts to load your session configuration file (`$HOME/.bashrc`). This error means the file is not accessible due to permissions or because your home directory is not mounted.

**Solutions:**
- The simplest fix is to disable loading configuration files:
  ```bash
  srun --pty bash --norc --noprofile
  ```
  Note: The shell environment will not be initialized.

- Alternatively, you can adjust file permissions (not always recommended for security):
  - Desired permissions:
    ```bash
    drwx------  ... /homes/my-login
    -rw-r--r--  ... /homes/my-login/.bashrc
    ```
  - Commands to set permissions:
    ```bash
    chmod u+x ~/        # for homedir folder
    chmod u+r ~/.bashrc # for .bashrc file
    ```

- You can also specify a custom configuration file with the `--rcfile` option:
  ```bash
  srun --pty bash --rcfile /Odyssey/private/my-login/.bashrc
  ```

---
## Why do I get "error: Unable to allocate resources: Requested node configuration is not available" when requesting a GPU?

When requesting GPU resources, you must ensure that the partition you are using actually has GPU nodes available.

For example, the following request will fail because the default partition does not provide any GPUs:
```bash
srun --pty \
     --gres=gpu:1 --cpus-per-task=2 --mem=4G \
     --time=00:05:00 \
     bash --norc --noprofile
```

To request GPUs, you must specify a partition that supports them:

- `MeeGPU` partition: very limited (e.g., only `gpu:p5000:1`)
- `Global` partition: all GPUs, but with low priority
- Your team's dedicated partition (if available)

Example using the `Global` partition:
```bash
srun -p Global \
     --pty \
     --gres=gpu:1 --cpus-per-task=2 --mem=4G \
     --time=00:05:00 \
     bash --norc --noprofile
```

Always check which partitions have GPU resources before submitting your job. You can use `sinfo -o "%P %G"` to list partitions and their available generic resources (like GPUs).