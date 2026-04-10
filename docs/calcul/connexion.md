# SSH/X2go access to servers

It is possible to connect directly via SSH or via X2go to a subset of MEE servers that are not in the Slurm cluster.

For connection to the Slurm cluster compute node servers, you must first have an active resource allocation (`salloc`, `srun` or `sbatch`). The connection is automatically terminated when the allocation is terminated.

 

## SSH connection from a Linux workstation

Open a terminal (++ctrl+alt+t++) and type the command:

```bash
ssh login@fqdn_of_the_server
```

For example:

```bash
ssh jnbazin@sl-mee-br-101.imta.fr
```

To be able to launch graphical applications (X11 forwarding), add the `-X` option, for example:

```bash
ssh -X jnbazin@sl-mee-br-101.imta.fr
```

When connecting via SSH, you may encounter the following error: `Could not chdir to home directory /homes/login: Permission denied`. You must then use an option to force the use of the password:

```bash
ssh login@sl-mee-br-xxx.imta.fr -o PreferredAuthentications=password
```



### SSH keys

It is not possible to use an SSH key pair to connect from a SALSA machine to a campux machine. For this to work, the user's homedir, which would contain the public key, would have to be mounted on the server. However, it is only once the SSH connection is established that the homedir is mounted. Therefore, you must use your password each time you connect.

On the other hand, between campux machines, no password is required, system keys have been set up.

## Connection from Windows

### with Mobaxterm (recommended)

Download and install Mobaxterm: [https://mobaxterm.mobatek.net/](https://mobaxterm.mobatek.net/){target="_blank"}

### with Putty

- Download and install Putty: [https://www.putty.org/](https://www.putty.org/){target="_blank"}
- Download and install Xming: [https://sourceforge.net/projects/xming/](https://sourceforge.net/projects/xming/){target="_blank"}

- Configuration and use

Launch Xming then Putty.

In Putty, it is possible to save a configuration for each server you want to connect to:

![Putty Configuration](../img/puttyX11Config.png#center#shadow)

Then, just click on the server in the **Saved Sessions** list, then click on **Open**. Or just double-click on the server.

## Using `nohup`

Nohup is a Unix command that allows you to launch a process that will remain active even after the user who initiated it disconnects. For example:

```bash
nohup simu.o
```

Aliases are not recognized when using `nohup`. It is generally more interesting to use `tmux`, which creates a complete user session with its environment.

## Connection with X2go

X2go is a display forwarding tool, allowing on your desktop or laptop PC, to display a desktop open on a remote machine (computing server for example). There is on the one hand the X2go client, installed on your workstation, and the X2go server installed on the remote machine.

The X2go server is installed on all MEE servers: [http://10.29.228.91/analyse_server.html](http://10.29.228.91/analyse_server.html){target="_blank"}

### Installation of the server on a SALSA machine

On the remote machines, you must install:

```bash
sudo apt install x2goserver xfce4
```

### Installation of the client

#### On Ubuntu

```bash
sudo apt install x2goclient
```

#### On Windows

Download the client here: [https://wiki.x2go.org/doku.php/doc:installation:x2goclient](https://wiki.x2go.org/doku.php/doc:installation:x2goclient){target="_blank"} and install it.

### Use

#### Creating sessions

It is possible to create and keep sessions to access them more quickly.

**File :arrow_right: New** session then fill in the fields as follows:

![Creating sessions on X2go](../img/X2GO-create-session.png#center#shadow)

Then click on **OK**

In the main window you should now see the session appear in the right panel. Click on its name, then enter the password.

#### Properly closing

When you close the session window, the remote session is not closed, and continues to occupy resources. To properly close a session, right-click on the remote desktop then:

**Application :arrow_right: Disconnect :arrow_right: Disconnect**

### Solve the problem of CPU overuse by x2goagent on the server

Problem encountered on Ubuntu 22.04 with the XFCE desktop: the X2go agent process takes 100% CPU.

Solution:

In an open X2go session on the server in question:

1. Open a terminal
2. Run the command `xfwm4-tweaks-settings`
3. Go to the **Compositor** tab
4. Uncheck **Enable display compositor**

## Renewal of the Kerberos ticket

In the case of long simulations, the authentication ticket, which allows among other things the mounting of the homedir must be renewed.

To do this, you must launch your simulation with the following command:

```bash
krenew -K 60 "the-simulation with its arguments"
```

In case of an error like: `krenew: error getting principal from old cache: No credentials cache found`, do, before, the command:

```bash
kinit
```
