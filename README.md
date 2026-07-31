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
