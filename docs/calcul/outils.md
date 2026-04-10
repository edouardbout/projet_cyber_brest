# Business tools (Matlab, Xilinx etc)

Information about the configuration of license tokens: [Software licenses](licences-logicielles.md)

## MEE tools on campux (servers/TP rooms)

### Usage on servers

Connect via graphical ssh or X2go to a [MEE server](servers-mee.md)

In the terminal, list the tools with the command `SETUP`. In what this command returns, here is the subset of tools managed by MEE:

```bash
# MATLAB2024a	            Matlab 2024a
# MEE_MODELSIM10.6d 	    Mentor Modelsim 10.6 (64 bits)
# MEE_MODELSIM2020.2 	    Mentor Modelsim 2020.2 (64 bits)
# MEE_MODELSIM2021.3	    Mentor Modelsim 2021.3 (64 bits)
# MEE_OPENOCD     	        OpenOCD with RISC-V support
# MEE_QUESTA10.7d 	        Mentor Questa 10.7 (64 bits)
# MEE_QUESTA20213     	    Mentor Questa 2021.3
# MEE_SYNOPSYSASIPDESIGNER	SYNOPSYS Asip Designer 2023
# MEE_SYSTEMC 		        SystemC 2.3.3 (x86_64)
# MEE_VITIS20202	        VITIS et VIVADO 2020.2 (x86_64)
# MEE_VIVADO20182  	        VIVADO 2018.2 (x86_64)
# MEE_VIVADO_CLASSROOM	    VIVADO 2024.1 FOR Classroom use(x86_64)
# MEE_VIVADO_LATEST	        VITIS et VIVADO 2024.1 (x86_64)
# MEE_XILINX_ISE147 	    XILINX ISE 14.7 (x86_64)
```

The `SETUP` command is provided by the DISI, and allows the setting up of environment variables allowing the use of the desired tools (`PATH`, `LD_LIBRARY_PATH`, `LM_LICENCE_FILE`…).

For example, for the use of Vivado in its latest version, you must do these commands:

```bash
SETUP MEE_VIVADO_LATEST
vivado
```

## Installation of tools on Ubuntu Salsa machine (desktop or lab)

### Installation of Vivado/Vitis:

Retrieve and extract the installation archive of the tool (ask the IT correspondents before)

Create a target directory for the installation (example for Vitis2020.2. To be replaced by a name adapted to the tool to be installed):

```bash
sudo mkdir -p /opt/xilinx/Vitis2020.2/
sudo chown -R $USER:$GROUPS /opt/xilinx/Vitis2020.2/
```
#### Manual installation

In the directory resulting from the extraction of the archive, run the command:
```bash
./xsetup
```

and follow the instructions.

#### Automatic installation

Go to the directory resulting from the extraction of the archive, then create the configuration file:
```bash
./xsetup -b ConfigGen
```

In the configuration file created (`install_config.txt`), edit the desired installation path (`/opt/xilinx/Vitis2020.2/` to be adapted)

Then install with the command:

```bash
./xsetup --agree XilinxEULA,3rdPartyEULA,WebTalkTerms --batch Install --config install_config.txt
```

## Call Questa 2021.3 from Xilinx Vivado 2020.1 **on the servers**

### Configuration

1. In the Tool Settings of third-party simulators (`Tools` :arrow_right: `Settings` :arrow_right: `3rd Party Simulators`), add the path `/usr/home/enstb1/MENTOR/Questa-2021.3/questasim/bin` in the Questasim field.
2. In the Project Settings of the current project, (`Tools` :arrow_right: `Settings` :arrow_right: `Simulator`), select `Questa Advanced Simulator` in the `Target Simulator` field. And put the path `/usr/home/enstb1/Xilinx/Vivado2020.1/Vivado/2020.1/SIM_LIB` in the `Compiled Library Location` field.

### Usage

You must do the `SETUP MEE_QUESTA20213` and the `SETUP MEE_VIVADO20201` before launching `vivado`

Then, click on `Run Simulation` will automatically launch `Questa`


## Matlab

List of licenses and available toolboxes: [https://intranet.telecom-bretagne.eu/cgi-bin/affich_lic.pl](https://intranet.telecom-bretagne.eu/cgi-bin/affich_lic.pl)

### Using Matlab on servers (preferred)

Connect via [graphical ssh or X2go](connexion.md) to a [MEE server](servers-mee.md)

```bash
SETUP MATLAB2024a
matlab
```

If there is licenses issues, delete all Matlab related configuration hidden files :

```bash
rm -rf ~/.MathWorks/ ~/.matlab/ ~/.MATLABConnector/
```

During the first use, you will be asked to log in with your school account to set up the licenses.

### Matlab web browser utilization on SALSA computer (windows/linux/mac)

It is now possible to access Matlab directly inside a web browser using the school's licenses on machines in SALSA, for (academic research only).

Simply go on [https://matlab.mathworks.com/](https://matlab.mathworks.com/), and use your IMT Atlantique email address then IMT Atlantique login/password to log on. All the computation is done on Mathworks servers

### Matlab installation on SALSA computer

If you still want to use it, go on [https://fr.mathworks.com/downloads/](https://fr.mathworks.com/downloads/) then downloads the desired version.

#### For Ubuntu 24.04

Use the R2025 version. Open a terminal ++ctrl+alt+t++, then create an installation directory and set you as owner:

```sh
sudo mkdir -p /opt/matlab
sudo chown $USER:$USER /opt/matlab
```

Extract the matlab installer .zip archive that you previously downloaded. For the example, lets assume you downloaded the installer archive into your `Download` directory of your homedir (**Change paths if needed**). Then launch the installer:

```sh
cd ~/Downloads
unzip matlab_R2025a_Linux.zip -d matlab_R2025a_Linux
cd matlab_R2025a_Linux
./install
```

Then use again your email address then IMT Atlantique login/password to authenticate. Choose the `/opt/matlab` directory as the installation destination directory. You can then choose the toolboxes that you need.

To simply and permanently add `matlab` to your `PATH`, create a symbolic link:

```sh
sudo ln -s /opt/matlab/bin/matlab /usr/bin/matlab
```

To launch Matlab, use the command `matlab` in a terminal.

#### For Windows

Unzip the archive, and double clic on the installer, then use your IMT Atlantique email address then IMT Atlantique login/password to log on. You can then choose the toolboxes that you need.

## SolidWorks sur serveur DISI AD

You can use Solidworks by connecting via [RDP](https://fr.wikipedia.org/wiki/Remote_Desktop_Protocol) to the server `sw-tp-br-001.ad.imta.fr`, on the domain `AD.imta.fr`.

You need to be on the school network (VPN or on site).

- From Windows, use “Remote Desktop Connection/Connexion bureau à distance”.
- From Linux, use the `redesktop` tool.

```sh
rdesktop -u $USER -d AD.imta.fr SW-TP-BR-001.ad.imta.fr
```
