# HomeLab Grafana with MCP Integration

This repository provides a ready-to-use Docker Compose setup for running Grafana in a homelab environment, with YAML-based provisioning, custom configuration, and integrated Model Context Protocol (MCP) server for AI assistant interactions.

## Features
- **Automatic provisioning** of datasources and dashboards via YAML files
- **Custom Grafana configuration** using `grafana.ini`
- **Environment variable passthrough** for secure and flexible datasource credentials
- **Persistent storage** for Grafana data
- **Pre-installed plugins** for enhanced functionality
- **MCP Grafana Server** for AI assistant integration with 40+ tools
- **Streamable HTTP endpoint** for external MCP client connections

## Directory Structure

```text
├── compose.yaml                # Docker Compose file with Grafana + MCP server
├── grafana.ini                 # Custom Grafana configuration
└── provisioning/
    └── datasources/
        └── datasources.yaml    # Example datasource provisioning
```

## Services

### Grafana
- **Port**: 3000
- **Image**: `grafana/grafana-enterprise`
- **Features**: Dashboard provisioning, OAuth integration, plugin support

### MCP Grafana Server
- **Port**: 8000  
- **Image**: `mcp/grafana`
- **Protocol**: Streamable HTTP
- **Tools**: 40+ available tools for dashboards, datasources, querying, alerting, and more

## Usage

1. **Clone this repository**

2. **Configure environment variables**

   Create a `.env` file in the root directory with your credentials:

   ```env
   # Database configurations
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
   
   # MCP Grafana API Key (required for MCP server)
   GRAFANA_API_KEY=your_grafana_service_account_token
   ```

   These variables are used in `provisioning/datasources/datasources.yaml` for secure, dynamic configuration.

3. **Create Grafana Service Account** (for MCP integration)

   - Start Grafana: `docker compose up grafana -d`
   - Go to [http://localhost:3000](http://localhost:3000) and log in
   - Navigate to **Administration** → **Service Accounts**
   - Create a new service account with appropriate permissions:
     - **Admin** role for full access
     - Or specific RBAC permissions for datasources, dashboards, etc.
   - Generate an API token and add it to your `.env` file as `GRAFANA_API_KEY`

4. **Customize Grafana settings**

   Edit `grafana.ini` to override Grafana defaults as needed.

5. **Provision datasources and dashboards**

   Place your YAML files in the `provisioning/` directory. Grafana will auto-load them at startup.

6. **Start the complete stack**

   ```powershell
   docker compose up -d
   ```

   - **Grafana**: Available at [http://localhost:3000](http://localhost:3000)
   - **MCP Server**: Available at [http://localhost:8000](http://localhost:8000)
   - Default Grafana login: `admin` / `admin` (change after first login)

## MCP Integration

The MCP (Model Context Protocol) server provides AI assistants with direct access to your Grafana instance through 40+ tools:

### Available Tool Categories

- **Dashboards**: Search, retrieve, and update dashboards
- **Datasources**: List and query Prometheus, Loki, and other data sources  
- **Alerting**: Manage alert rules and contact points
- **Incidents**: Create and manage Grafana Incident records
- **OnCall**: Access Grafana OnCall schedules and users
- **Sift**: Use Sift for log analysis and investigation
- **Admin**: Manage teams and users

### Connecting MCP Clients

The MCP server runs in **Streamable HTTP** mode and exposes an endpoint at `http://localhost:8000`. Configure your MCP-compatible AI assistant to connect to this endpoint.

Example configuration for supported clients:

```json
{
  "grafana": {
    "type": "http",
    "url": "http://localhost:8000"
  }
}
```

## References

- [Grafana Provisioning Docs](https://grafana.com/docs/grafana/latest/administration/provisioning/)
- [Grafana Configuration Docs](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/)
- [MCP Grafana GitHub](https://github.com/grafana/mcp-grafana)
- [Model Context Protocol](https://modelcontextprotocol.io/)

---

**Tips:**

- To add more datasources or dashboards, just drop additional YAML files into the appropriate subfolders under `provisioning/`.
- For plugin management, edit the `GF_INSTALL_PLUGINS` variable in `compose.yaml`.
- The MCP server requires a Grafana service account token with appropriate permissions for the tools you want to use.
- Use debug mode by adding `"-debug"` to the MCP server command for troubleshooting.
