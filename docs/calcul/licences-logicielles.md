# Software licenses

List of licenses detailed by tools, automatically updated every 5 minutes: [http://10.29.228.91/licences.html](http://10.29.228.91/licences.html) (only accessible from the school network or via VPN)

See the page of the [business tools](outils.md)

Here is the list of flexlm licenses for the tools commonly used at MEE:


| Tools     | Environment variable                     | License                       |
|-----------|------------------------------------------|-------------------------------|
| Xilinx    | `XILINXD_LICENSE_FILE`                   | `27007@licence-01.imta.fr`    |
| Mentor    | `MGLS_LICENSE_FILE` ou `LM_LICENSE_FILE` | `27005@licence-01.imta.fr`    |
| Orcad     | `CDS_LIC_FILE`                           | `27009@licence-01.imta.fr`    |
| Synopsys  | `LM_LICENSE_FILE`                        | `27013@licence-01.imta.fr`    |
| Altera    | `ALTERAD_LICENSE_FILE`                   | `27030@judy.enst-bretagne.fr` |
| Tensilica | `LM_LICENSE_FILE`                        | `27021@licence-01.imta.fr`    |

## Setup under linux

On linux, you have to assign the environment variables via the following commands depending on the desired tool:

```bash
export XILINXD_LICENSE_FILE="27007@licence-01.imta.fr"
export MGLS_LICENSE_FILE="27005@licence-01.imta.fr"
export LM_LICENSE_FILE="27013@licence-01.imta.fr;27005@licence-01.imta.fr;27021@licence-01.imta.fr"
export ALTERAD_LICENSE_FILE="27030@judy.enst-bretagne.fr"
```
All or part of these commands can be put in a script, for example `~/script_setup_licence.sh`, which can be executed via the `source` command:

```bash
source script_setup_licence.sh
```

!!! info

    To concatenate several values, we use the separator `:` (colon)

Then you have to launch the tool(s) in the same terminal.


## Setup under windows

`Start` :arrow_right: search for "Environment variables" :arrow_right: click on `Environment variables...`

add (or modify) the system variables:

| name                   | value                                                                        |
|------------------------|------------------------------------------------------------------------------|
| `XILINXD_LICENSE_FILE` | `27007@licence-01.imta.fr`                                                   |
| `MGLS_LICENSE_FILE`    | `27005@licence-01.imta.fr`                                                   |
| `CDS_LIC_FILE`         | `27009@licence-01.imta.fr`                                                   |
| `LM_LICENSE_FILE`      | `27013@licence-01.imta.fr;27005@licence-01.imta.fr;27021@licence-01.imta.fr` |
| `ALTERAD_LICENSE_FILE` | `27030@judy.enst-bretagne.fr`                                                |


!!! info
    To concatenate several values, we use the separator `;`  (semicolon)
