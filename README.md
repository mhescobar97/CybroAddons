# Odoo Addons Collection for Odoo v18

This repository is a fork of the [CybroAddons](https://github.com/mhescobar97/CybroAddons.git) project. It provides a curated collection of third-party addons for **Odoo Community Edition v18**. This fork is intended for development, learning, and training purposes and assumes deployment via Docker using the included `docker-compose.yml`.

---

## Table of Contents

1. [About the Fork](#about-the-fork)
2. [Prerequisites](#prerequisites)
3. [Repository Structure](#repository-structure)
4. [Usage](#usage)
   - [Step 1: Clone the Repository](#step-1-clone-the-repository)
   - [Step 2: Configure Directories and Permissions](#step-2-configure-directories-and-permissions)
   - [Step 3: Deploy with Docker Compose](#step-3-deploy-with-docker-compose)
   - [Step 4: Access Odoo](#step-4-access-odoo)
   - [Step 5: Add Custom Addons](#step-5-add-custom-addons)
5. [Technical Details](#technical-details)
6. [Security Notes](#security-notes)
7. [Contributing](#contributing)
8. [Acknowledgments](#acknowledgments)

---

## About the Fork

- **Original Repository**: [CybroAddons on GitHub](https://github.com/mhescobar97/CybroAddons)
- **Purpose**:
  - Keep addons synced with upstream changes.
  - Enhance documentation and setup for Docker-based deployment.
  - Enable customization and community contributions.

## Prerequisites

Ensure you have the following installed:

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install)

## Repository Structure

```text
addons/             # Custom or third-party Odoo modules
etc/                # Odoo configuration files (e.g., odoo.conf)
postgresql/         # Persistent PostgreSQL data storage
entrypoint.sh       # Custom Odoo container entrypoint script
docker-compose.yml  # Docker Compose configuration
timezone.conf       # Optional timezone configuration
README.md           # This file
```

## Usage

### Step 1: Clone the Repository

```bash
git clone https://github.com/mhescobar97/CybroAddons.git
cd CybroAddons
```

### Step 2: Configure Directories and Permissions

```bash
mkdir -p ./addons ./etc ./postgresql
chmod -R 777 ./addons ./etc ./postgresql
```

> **Note:** `chmod 777` is used for development. Use stricter permissions in production.

### Step 3: Deploy with Docker Compose

Use the provided `docker-compose.yml`:

```yaml
version: "3"
services:
  db:
    image: docker.io/postgres:17
    user: root
    network_mode: "host"
    environment:
      - POSTGRES_USER=odoo
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=postgres
    volumes:
      - ./postgresql:/var/lib/postgresql/data
    restart: always

  odoo18:
    image: docker.io/odoo:18
    user: root
    network_mode: "host"
    depends_on:
      - db
    tty: true
    command: -- --dev=all
    environment:
      - HOST=localhost
      - USER=odoo
      - PASSWORD=postgres
      - PIP_BREAK_SYSTEM_PACKAGES=1
    volumes:
      - ./entrypoint.sh:/entrypoint.sh
      - ./addons:/mnt/extra-addons
      - ./etc:/etc/odoo
    restart: always
```

Start the services:

```bash
docker-compose up -d
```

### Step 4: Access Odoo

Open your browser to `http://localhost:8069` and log in with:

- **Master Password**: `postgres` (or your configured password)

### Step 5: Add Custom Addons

1. Place your modules in the `addons/` directory.
2. In Odoo:
   - Go to **Apps**.
   - Click **Update Apps List**.
   - Search for and install your addon.

## Technical Details

- **Odoo Version**: Community Edition v18
- **Database**: PostgreSQL 17
- **Deployment**: Docker Compose
- **Development Mode**: Enabled (`--dev=all`)

## Security Notes

- **Default Passwords**: `odoo` / `postgres` (development only). Use strong credentials in production.
- **Permissions**: `chmod 777` is a convenience for development. Adjust in production.

## Contributing

1. Fork this repository.
2. Create a feature branch: `git checkout -b feature/YourFeature`.
3. Commit changes: `git commit -m "Add some feature"`.
4. Push to: `git push origin feature/YourFeature`.
5. Open a pull request and describe your changes.

## Acknowledgments

This project is based on [CybroAddons](https://github.com/mhescobar97/CybroAddons). Thanks to all contributors for their work and support of the Odoo community!


