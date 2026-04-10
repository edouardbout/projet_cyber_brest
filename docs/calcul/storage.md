# MEE Storage servers

A shared network storage space has been set up. Volumes have been created for teams that have requested them. All volumes are mounted on all servers (Slurm nodes and SSH nodes)

The emergence of AI and machine learning has led to a strong demand for large storage volumes.

A storage server has been set up by MEE. Different volumes are hosted on this server, and are mounted by default on all MEE servers with **ubuntu 24.04**.

A volume per team with needs has been created:

| Mount point on server | quota |
|-----------------------|-------|
| `/Brain`              | 20TB  |
| `/Codes`              | 20TB  |
| `/Odyssey`            | 20TB  |

!!! warning

    For now, no user quota is in place. Be careful not to fill these volumes, they are shared spaces.

Each team folder contains 2 subfolders, corresponding to 2 types of data:

- `public`: for data shared within the team:
    - Place your data in folders with names understandable by everyone
    - Change permissions to make the data accessible
- `private`: for personal data.
    - Create a subfolder corresponding to your login
    - Manage access rights according to constraints

## `/Odyssey` volume

In this volume are two directories:

- `public`: dedicated to the storage of data shared by several users, the rights must be set to `rwxr-xr-x`
- `private`: dedicated to data specific to each user. Each user should only create one folder named with their login.

---
## Storage space `$SCRATCH`

The `$SCRATCH` storage space is a local hard drive volume on a compute node. Each node has a `$SCRATCH` volume. These volumes are to be used to store temporary working data. The data on these volumes is supposed to be deleted at the end of the job that used it.

`$SCRATCH` is an environment variable that points to a path. This choice is made because the storage resources available in the cluster are heterogeneous. To access it, for example:

```bash
cd $SCRATCH
```

---
## IMT Atlantique Homedir on Slurm nodes

!!! warning

    The homedir (`/homes/login`) is not mounted on any nodes by default. So when a job is dispatched on a node, it cannot access it.
