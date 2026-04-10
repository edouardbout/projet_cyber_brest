# MEE servers and SLURM Partitions

---
## MEE Servers

The MEE department has a number of computation servers available from the school network.

The OS installed on the servers is Ubuntu in version 22.04 or 24.04. The naming corresponds to a nomenclature adopted by the DISI. They are of the form: `sl-mee-br-123.imta.fr`.

  - `sl` : Linux Server
  - `mee` : for the department/service
  - `br` : for Brest campus

The numbers have been chosen to reflect the membership of the teams:

  - `1xx` : Servers in free service, not assigned to a team
  - `2xx` : Odyssey team servers
  - `3xx` : Codes team servers
  - `4xx` : Matrix team servers
  - `5xx` : Brain team servers

The list of servers is on this summary supervision page: [http://10.29.228.91/analyse_server.html](http://10.29.228.91/analyse_server.html){target="_blank"}, accessible only from the school network.

---
## SLURM Partitions

Most of the server are managed by SLURM, and are assigned to a partition.

The naming corresponds to a nomenclature of the form "<TEAM>-<SUB-CATEG>". For example :

- "Odyssey" : Odyssey team's servers
- "Brain3080" : Brain team's servers, with Nvidia 3080 GPU Card

The default partition is `Mee`, which includes only a limited number of compute nodes.

The `Global` partition provides access to all compute nodes, but jobs submitted here have a lower priority, a maximum runtime of 24 hours, and may be terminated if resources are needed for jobs in team-specific partitions.

---
## Servers not affiliated with a team

- Not managed by SLURM :

| server name           | CPU Core number | RAM (Go) | GPU |
| --------------------- | --------------- | -------- | --- |
| sl-mee-br-103.imta.fr | 8               | 128      | -   |
| sl-mee-br-104.imta.fr | 8               | 64       | -   |


| server name           | CPU Core number | RAM (Go) | GPU |
| --------------------- | --------------- | -------- | --- |
| sl-mee-br-103.imta.fr | 8               | 128      | -   |
| sl-mee-br-104.imta.fr | 8               | 64       | -   |
| sl-mee-br-106.imta.fr | 20              | 96       | -   |
| sl-mee-br-109.imta.fr | 24              | 192Go    | -   |
| sl-mee-br-111.imta.fr | 24              | 128Go    | -   |
| sl-mee-br-112.imta.fr | 20              | 128Go    | -   |
| sl-mee-br-113.imta.fr | 20              | 128Go    | -   |
| sl-mee-br-114.imta.fr | 24              | 192Go    | -   |
| sl-mee-br-115.imta.fr | 24              | 192Go    | -   |
| sl-mee-br-116.imta.fr | 24              | 192Go    | -   |


---
## Odyssey team Servers / Slurm Partitions

- SLURM `Odysey` partition :

| Server Name           | CPU Cores | RAM (Go) | GPU           |
| --------------------- | --------- | -------- | ------------- |
| sl-mee-br-201.imta.fr | 12        | 64       | tesla k80 : 2 |
| sl-mee-br-202.imta.fr | 64        | 192      | rtx8000 : 3   |
| sl-mee-br-203.imta.fr | 64        | 192      | rtx8000 : 3   |
| sl-mee-br-204.imta.fr | 64        | 192      | rtx8000 : 3   |
| sl-mee-br-205.imta.fr | 64        | 192      | a40 : 3       |
| sl-mee-br-206.imta.fr | 96        | 512      | A100 : 8      |
| sl-mee-br-207.imta.fr | 96        | 512      | A100 : 8      |
| sl-mee-br-208.imta.fr | 96        | 512      | L40S : 4      |
| sl-mee-br-209.imta.fr | 96        | 512      | L40S : 4      |
| sl-mee-br-210.imta.fr | 96        | 512      | H100 : 2      |
| sl-mee-br-211.imta.fr | 96        | 512      | H100 : 2      |
| sl-mee-br-212.imta.fr | 96        | 512      | H100 : 2      |
| sl-mee-br-213.imta.fr | 96        | 512      | H100 : 2      |

---
## CODES team servers / Slurm Partitions

| server name           | CPU Core number | RAM (Go) |
| --------------------- | --------------- | -------- |
| sl-mee-br-301.imta.fr | 4               | 8        |
| sl-mee-br-302.imta.fr | 4               | 8        |
| sl-mee-br-303.imta.fr | 4               | 8        |
| sl-mee-br-304.imta.fr | 4               | 8        |
| sl-mee-br-305.imta.fr | 4               | 8        |
| sl-mee-br-306.imta.fr | 4               | 8        |
| sl-mee-br-307.imta.fr | 4               | 8        |
| sl-mee-br-308.imta.fr | 4               | 8        |
| sl-mee-br-309.imta.fr | 4               | 8        |
| sl-mee-br-310.imta.fr | 8               | 8        |
| sl-mee-br-311.imta.fr | 8               | 32       |
| sl-mee-br-312.imta.fr | 8               | 8        |
| sl-mee-br-313.imta.fr | 8               | 8        |
| sl-mee-br-314.imta.fr | 4               | 32       |
| sl-mee-br-315.imta.fr | 4               | 16       |
| sl-mee-br-316.imta.fr | 4               | 16       |
| sl-mee-br-317.imta.fr | 4               | 16       |
| sl-mee-br-318.imta.fr | 4               | 16       |
| sl-mee-br-319.imta.fr | 4               | 32       |
| sl-mee-br-320.imta.fr | 4               | 16       |
| sl-mee-br-321.imta.fr | 4               | 16       |
| sl-mee-br-322.imta.fr | 4               | 16       |
| sl-mee-br-323.imta.fr | 4               | 16       |
| sl-mee-br-324.imta.fr | 4               | 32       |
| sl-mee-br-325.imta.fr | 4               | 32       |
| sl-mee-br-326.imta.fr | 4               | 16       |
| sl-mee-br-327.imta.fr | 4               | 16       |
| sl-mee-br-328.imta.fr | 4               | 16       |
| sl-mee-br-329.imta.fr | 4               | 16       |
| sl-mee-br-330.imta.fr | 4               | 16       |
| sl-mee-br-331.imta.fr | 4               | 16       |
| sl-mee-br-332.imta.fr | 4               | 16       |
| sl-mee-br-333.imta.fr | 4               | 16       |
| sl-mee-br-334.imta.fr | 16              | 32       |
| sl-mee-br-335.imta.fr | 16              | 32       |
| sl-mee-br-336.imta.fr | 16              | 32       |
| sl-mee-br-337.imta.fr | 6               | 16       |
| sl-mee-br-338.imta.fr | 6               | 16       |
| sl-mee-br-339.imta.fr | 6               | 16       |
| sl-mee-br-340.imta.fr | 18              | 64       |

---
## Matrix team servers / Slurm Partitions

| server name           | CPU Core number | RAM (Go) | GPU       |
| --------------------- | --------------- | -------- | --------- |
| sl-mee-br-401.imta.fr | 40              | 96       | a5000 : 1 |

---
## Brain team servers / Slurm Partitions

| server name           | CPU Core number | RAM (Go) | GPU      |
| --------------------- | --------------- | -------- | -------- |
| sl-mee-br-501.imta.fr | 96              | 503      | a100 : 3 |
| sl-tp-br-501.imta.fr  | 4               | 8        | GTX 1060 |
| sl-tp-br-502.imta.fr  | 4               | 8        | GTX 1060 |
| sl-tp-br-503.imta.fr  | 4               | 8        | GTX 1060 |
| sl-tp-br-504.imta.fr  | 4               | 8        | GTX 1060 |
| sl-tp-br-505.imta.fr  | 4               | 8        | GTX 1060 |
| sl-tp-br-506.imta.fr  | 4               | 8        | GTX 1060 |
| sl-tp-br-507.imta.fr  | 4               | 8        | GTX 1060 |
| sl-tp-br-508.imta.fr  | 4               | 8        | GTX 1060 |
| sl-tp-br-509.imta.fr  | 4               | 8        | GTX 1060 |
| sl-tp-br-510.imta.fr  | 4               | 8        | GTX 1060 |
| sl-tp-br-511.imta.fr  | 4               | 8        | GTX 1060 |
| sl-tp-br-512.imta.fr  | 4               | 8        | GTX 1060 |

- SLURM `Brain3080` partition :

| server name          | CPU Core number | RAM (Go) | GPU        |
| -------------------- | --------------- | -------- | ---------- |
| sl-tp-br-513.imta.fr | 8               | 16       | RTX 3080:1 |
| sl-tp-br-514.imta.fr | 8               | 16       | RTX 3080:1 |
| sl-tp-br-515.imta.fr | 8               | 16       | RTX 3080:1 |
| sl-tp-br-516.imta.fr | 8               | 16       | RTX 3080:1 |
| sl-tp-br-517.imta.fr | 8               | 16       | RTX 3080:1 |
| sl-tp-br-518.imta.fr | 8               | 16       | RTX 3080:1 |
| sl-tp-br-519.imta.fr | 8               | 16       | RTX 3080:1 |
| sl-tp-br-520.imta.fr | 8               | 16       | RTX 3080:1 |
| sl-tp-br-521.imta.fr | 32              | 64       | RTX 3090:1 |
| sl-tp-br-522.imta.fr | 32              | 64       | RTX 3090:1 |
| sl-tp-br-523.imta.fr | 32              | 64       | RTX 3090:1 |
| sl-tp-br-524.imta.fr | 32              | 64       | RTX 3090:1 |

---
## DFVS servers / Slurm Partitions

| server name          | CPU Core number | RAM (Go) | GPU        |
| -------------------- | --------------- | -------- | ---------- |
| sl-tp-br-021.imta.fr | 4               | 15       | GTX 1080:1 |
| sl-tp-br-022.imta.fr | 4               | 15       | GTX 1080:1 |
| sl-tp-br-023.imta.fr | 4               | 15       | GTX 1080:1 |
| sl-tp-br-024.imta.fr | 4               | 15       | GTX 1080:1 |
| sl-tp-br-025.imta.fr | 4               | 15       | GTX 1080:1 |
