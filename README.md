# Identity & Access Management Database (`ld-iam-db`)

[![SQL Server](https://img.shields.io/badge/SQL%20Server-2022--latest-CC292B?logo=microsoftsqlserver)](https://www.microsoft.com/en-us/sql-server/)
[![Docker](https://img.shields.io/badge/Docker-Container-2496ED?logo=docker)](https://www.docker.com/)
[![Ubuntu](https://img.shields.io/badge/OS-Ubuntu-E95420?logo=ubuntu)](https://ubuntu.com/)

The **`ld-iam-db`** repository contains the container configuration for the relational database component of the Numeraid Identity and Access Management service. It customizes and runs **Microsoft SQL Server 2022 Developer Edition** as a secure, non-root container instance.

---

## Technical Specification

* **Base Image:** `mcr.microsoft.com/mssql/server:2022-latest`
* **Default Runtime User:** `mssql` (non-root execution)
* **Default Database Port:** `1433`
* **Local Image Tag:** `numeraid-db:latest`
* **System Data Directory:** `/var/opt/mssql/data/`

---

## Repository Structure

Located under the private database directory structure (`private/databases/ld-iam-db`):

```text
private/databases/ld-iam-db/
├── .dockerignore       # Docker build exclusion rules
├── Dockerfile          # Custom SQL Server 2022 container definition
└── README.md           # Database repository documentation

```

---

## Docker Build & Execution Guide

### 1. Dockerfile Definition

The container image sets non-root execution rules and exposes standard SQL Server ports:

```dockerfile
FROM [mcr.microsoft.com/mssql/server:2022-latest](https://mcr.microsoft.com/mssql/server:2022-latest)
USER mssql
EXPOSE 1433

```

### 2. Building the Image

From inside the `private/databases/ld-iam-db` directory, execute:

```bash
docker build -t numeraid-db:latest .

```

### 3. Running the Container

SQL Server 2022 requires accepting the End-User License Agreement (EULA) and supplying a system administrator (`sa`) password via environment variables upon launch. Running without these flags will cause container initialization to fail.

```bash
docker run -d \
  --name ld-iam-db-container \
  -p 1433:1433 \
  -e "ACCEPT_EULA=Y" \
  -e "MSSQL_SA_PASSWORD=strongestAvangerPassword01" \
  numeraid-db:latest

```

---

## Startup Verification & Logs

On first launch with `ACCEPT_EULA=Y`, SQL Server initializes default system databases (`master.mdf`, `msdbdata.mdf`, `model.mdf`) in `/var/opt/mssql/data/`.

You can inspect the initialization and log state using:

```bash
docker logs <container_id>

```

**Expected Log Output:**

```text
SQL Server 2022 will run as non-root by default. This container is running as user mssql.
Setup step is copying system data file 'C:\templatedata\master.mdf' to '/var/opt/mssql/data/master.mdf'.
Setup step is copying system data file 'C:\templatedata\msdbdata.mdf' to '/var/opt/mssql/data/msdbdata.mdf'.
Microsoft SQL Server 2022 (RTM-CU26) - Developer Edition (64-bit) on Linux

```

---

## Related Repositories

This database container provides schema persistence for the platform microservices:

* **[`ld-iam-api`](https://www.google.com/search?q=https://github.com/YOUR_USERNAME/ld-iam-api)**: Identity & Access Management ASP.NET Core Backend API connecting via port 1433.
* **[`ld-ui`](https://www.google.com/search?q=https://github.com/YOUR_USERNAME/ld-ui)**: React / Vite frontend interacting with backend APIs.

---

## License

Distributed under the MIT License. See `LICENSE` for details.
