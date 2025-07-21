# HomeLab Grafana Quickstart

This repository provides a ready-to-use Docker Compose setup for running Grafana in a homelab environment, with YAML-based provisioning and custom configuration.

## Features
- **Automatic provisioning** of datasources and dashboards via YAML files
- **Custom Grafana configuration** using `grafana.ini`
- **Environment variable passthrough** for secure and flexible datasource credentials
- **Persistent storage** for Grafana data
- **Pre-installed plugins** for enhanced functionality

## Directory Structure
```
├── compose.yaml                # Docker Compose file
├── grafana.ini                 # Custom Grafana configuration
└── provisioning/
    └── datasources/
        └── datasources.yaml    # Example datasource provisioning
```

## Usage

1. **Clone this repository**

2. **Configure environment variables**
   - Create a `.env` file in the root directory with your database credentials:
     ```env
     PNS_DB_HOST=your_pns_host
     PNS_DB_PORT=5432
     PNS_DB_NAME=your_pns_db
     PNS_DB_USER=your_pns_user
     PNS_DB_PASSWORD=your_pns_password
     RCC_DB_HOST=your_rcc_host
     RCC_DB_PORT=5432
     RCC_DB_NAME=your_rcc_db
     RCC_DB_USER=your_rcc_user
     RCC_DB_PASSWORD=your_rcc_password
     ts-test_HOST=your_ts_host
     ts-test_PORT=5432
     ts-test_DB_NAME=your_ts_db
     ts-test_USER=your_ts_user
     ts-test_PASSWORD=your_ts_password
     ```
   - These variables are used in `provisioning/datasources/datasources.yaml` for secure, dynamic configuration.

3. **Customize Grafana settings**
   - Edit `grafana.ini` to override Grafana defaults as needed.

4. **Provision datasources and dashboards**
   - Place your YAML files in the `provisioning/` directory. Grafana will auto-load them at startup.

5. **Start Grafana**
   ```powershell
   docker compose up -d
   ```
   - Grafana will be available at [http://localhost:3000](http://localhost:3000)
   - Default login: `admin` / `admin` (change after first login)

## References
- [Grafana Provisioning Docs](https://grafana.com/docs/grafana/latest/administration/provisioning/)
- [Grafana Configuration Docs](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/)

---

**Tip:**
- To add more datasources or dashboards, just drop additional YAML files into the appropriate subfolders under `provisioning/`.
- For plugin management, edit the `GF_INSTALL_PLUGINS` variable in `compose.yaml`.
