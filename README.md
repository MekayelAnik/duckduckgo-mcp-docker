# DuckDuckGo Search MCP Server
### Multi-Architecture Docker Image for Distributed Deployment

<div align="left">

<img alt="ddg-mcp" src="https://img.shields.io/badge/DuckDuckGo-Search-DE5833?style=for-the-badge&logo=duckduckgo&logoColor=white" width="400">

[![Docker Pulls](https://img.shields.io/docker/pulls/mekayelanik/ddg-mcp.svg?style=flat-square)](https://hub.docker.com/r/mekayelanik/ddg-mcp)
[![Docker Stars](https://img.shields.io/docker/stars/mekayelanik/ddg-mcp.svg?style=flat-square)](https://hub.docker.com/r/mekayelanik/ddg-mcp)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg?style=flat-square)](LICENSE)

**[NPM Package](https://www.npmjs.com/package/@oevortex/ddg_search)** • **[GitHub Repository](https://github.com/OEvortex/ddg_search)** • **[Docker Hub](https://hub.docker.com/r/mekayelanik/ddg-mcp)** • **[Docker Image Github Repository](https://github.com/MekayelAnik/duckduckgo-mcp-docker/refs/heads/main/LICENSE)**

</div>

---

## 📋 Table of Contents

- [Overview](#overview)
- [Supported Architectures](#supported-architectures)
- [Available Tags](#available-tags)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
- [MCP Client Configuration](#mcp-client-configuration)
- [Available Tools](#available-tools)
- [Network Configuration](#network-configuration)
- [Updating](#updating)
- [Troubleshooting](#troubleshooting)
- [Additional Resources](#additional-resources)
- [Support & Contributing](#support--contributing)
- [License](#license)

---

## Overview

DuckDuckGo Search MCP Server is a privacy-focused, lightweight Model Context Protocol server that provides web search capabilities without requiring API keys. Built on Alpine Linux for a minimal footprint and maximum security, it offers real web scraping capabilities for comprehensive search results. This can be used with AI IDEs (VS Code, Claude CLI, Codex, Cursor, etc.), AI clients (Ollama, LM Studio, VLLM, etc.), or directly in RAG applications.

### Key Features

✨ **No API Keys Required** - Works out of the box with zero configuration  
🌐 **Multi-Source Search** - DuckDuckGo web search and Felo AI integration  
🔍 **Advanced Content Extraction** - Smart URL fetching with customizable filtering  
📊 **Metadata Extraction** - Retrieve titles, descriptions, and images from URLs  
🚀 **Multiple Transport Protocols** - HTTP, SSE, and WebSocket support  
🔒 **Privacy-First Design** - No tracking, no data collection  
⚡ **Performance Optimized** - Built-in caching and rate limiting  
🎯 **Production Ready** - Stable releases with comprehensive testing  
🔧 **Easy Configuration** - Simple environment variable setup  
🛡️ **Security Features** - Rotating user agents and request filtering

---

## Supported Architectures

DuckDuckGo Search MCP Server supports multiple architectures through Docker's multi-arch capabilities:

| Architecture | Tag Prefix | Status | Notes |
|:-------------|:-----------|:------:|:------|
| **x86-64** | `amd64-<version>` | ✅ Stable | Fully tested on Intel/AMD processors |
| **ARM64** | `arm64v8-<version>` | ✅ Stable | Optimized for Raspberry Pi, Apple Silicon |

> 💡 **Note:** Multi-arch images automatically select the correct architecture for your system.

---

## Available Tags

Choose the right tag for your deployment:

| Tag | Stability | Description | Use Case |
|:----|:---------:|:------------|:---------|
| `stable` | ⭐⭐⭐ | Most stable release | **Recommended for production** |
| `latest` | ⭐⭐⭐ | Latest stable release | Stay current with stable features |
| `1.x.x` | ⭐⭐⭐ | Specific version | Version pinning for consistency |
| `beta` | ⚠️ | Beta releases | **Testing only - not for production** |

### Version Tagging Strategy

```bash
# Production deployment (recommended)
mekayelanik/ddg-mcp:stable

# Latest stable features
mekayelanik/ddg-mcp:latest

# Pin to specific version
mekayelanik/ddg-mcp:1.1.3

# Testing beta features (use with caution)
mekayelanik/ddg-mcp:beta
```

---

## Quick Start

### Docker Compose (Recommended)

Create a `docker-compose.yml` file:

```yaml
services:
  ddg-mcp:
    image: mekayelanik/ddg-mcp:stable
    container_name: ddg-mcp
    restart: unless-stopped
    ports:
      - "8020:8020"
    environment:
      - PORT=8020
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Dhaka
      - PROTOCOL=SHTTP
      - CORS=*
    hostname: ddg-mcp
    domainname: local
```

**Deploy the container:**

```bash
# Start the service
docker compose up -d

# View logs
docker compose logs -f ddg-mcp

# Check status
docker compose ps
```

### Docker CLI

For quick testing or simple deployments:

```bash
docker run -d \
  --name=ddg-mcp \
  --restart=unless-stopped \
  -p 8020:8020 \
  -e PORT=8020 \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Asia/Dhaka \
  -e PROTOCOL=SHTTP \
  -e CORS=* \
  mekayelanik/ddg-mcp:stable
```

### Access the Server

After deployment, the server provides multiple endpoints:

| Protocol | Endpoint | Use Case |
|:---------|:---------|:---------|
| **HTTP (Streamable)** | `http://host-ip:8020/mcp` | Best compatibility, recommended |
| **SSE** | `http://host-ip:8020/sse` | Real-time streaming responses |
| **WebSocket** | `ws://host-ip:8020/message` | Bidirectional communication |
| **Health Check** | `http://host-ip:8020/healthz` | Service health monitoring |

> ⏱️ **ARM Devices:** Allow 30-60 seconds for initialization on Raspberry Pi and similar devices before accessing endpoints.

### Health Check

Verify the server is running:

```bash
# Health check endpoint
curl http://localhost:8020/healthz

# Test MCP endpoint
curl http://localhost:8020/mcp

# Detailed response
curl -v http://localhost:8020/mcp
```

---

## Configuration

### Environment Variables

Customize your deployment with these environment variables:

| Variable | Default | Options | Description |
|:---------|:-------:|:--------|:------------|
| `PORT` | `8020` | Any valid port (1-65535) | Internal server port |
| `PUID` | `1000` | Any valid UID | User ID for file permissions |
| `PGID` | `1000` | Any valid GID | Group ID for file permissions |
| `TZ` | `Asia/Dhaka` | [TZ database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) | Container timezone |
| `PROTOCOL` | `SHTTP` | `SHTTP`, `SSE`, `WS` | Default transport protocol |
| `CORS` | _(none)_ | Domain(s), `*`, or regex | Cross-Origin Resource Sharing config |
| `DEBUG_MODE` | `false` | `true`, `false`, `1`, `0` | Enable debug mode with nano editor |

### User & Group Identifiers

To avoid permission issues, set `PUID` and `PGID` to match your host user:

```bash
# Find your user and group IDs
id username

# Example output:
# uid=1000(dockeruser) gid=1000(dockergroup) groups=1000(dockergroup)
```

Use these values in your Docker configuration:

```yaml
environment:
  - PUID=1000  # Use your uid value
  - PGID=1000  # Use your gid value
```

### CORS Configuration

Configure Cross-Origin Resource Sharing for web access:

```yaml
# Allow all origins (development only)
environment:
  - CORS=*

# Allow specific domains
environment:
  - CORS=https://example.com,https://app.example.com

# Allow IP addresses
environment:
  - CORS=192.168.1.100,192.168.1.101

# Mix domains and IPs
environment:
  - CORS=https://example.com,192.168.1.100:3000

# Use regex patterns
environment:
  - CORS=/^https?:\/\/.*\.example\.com$/
```

> ⚠️ **Security Warning:** Using `CORS=*` allows all origins and should only be used in development. Always specify exact origins in production.

### Timezone Configuration

Set your local timezone for accurate logging:

```yaml
environment:
  - TZ=America/New_York  # US Eastern
  - TZ=Europe/London     # UK
  - TZ=Asia/Tokyo        # Japan
  - TZ=UTC               # Coordinated Universal Time
```

### Debug Mode

Enable debug mode for troubleshooting:

```yaml
environment:
  - DEBUG_MODE=true  # Enables nano editor and pauses container for inspection
```

When debug mode is enabled:
- Container installs nano editor
- Container pauses after initialization
- Exec into container to investigate issues
- Useful for debugging entrypoint or configuration problems

```bash
# Access debug container
docker exec -it ddg-mcp sh
```

---

## MCP Client Configuration

### Transport Support Matrix

| Client | HTTP | SSE | WebSocket | Recommended |
|:-------|:----:|:---:|:---------:|:------------|
| **VS Code (Cline/Roo-Cline)** | ✅ | ✅ | ❌ | HTTP |
| **Claude Desktop** | ✅ | ✅ | ⚠️* | HTTP |
| **Claude CLI** | ✅ | ✅ | ⚠️* | HTTP |
| **Codex CLI** | ✅ | ✅ | ⚠️* | HTTP |
| **Codeium (Windsurf)** | ✅ | ✅ | ⚠️* | HTTP |
| **Cursor** | ✅ | ✅ | ⚠️* | HTTP |

> ⚠️ *WebSocket support is experimental and not part of the official MCP specification

### VS Code (Cline/Roo-Cline Extension)

Configure in `.vscode/settings.json` or `~/.config/Code/User/settings.json`:

#### HTTP (Recommended)

```json
{
  "mcp.servers": {
    "ddg-search": {
      "url": "http://host-ip:8020/mcp",
      "transport": "http"
    }
  }
}
```

#### With Auto-Approval

```json
{
  "mcp.servers": {
    "ddg-search": {
      "url": "http://host-ip:8020/mcp",
      "transport": "http",
      "autoApprove": [
        "web-search",
        "felo-search",
        "fetch-url",
        "url-metadata"
      ]
    }
  }
}
```

### Claude Desktop App

#### Configuration Locations

| Platform | Configuration Path |
|:---------|:-------------------|
| **Linux** | `~/.config/Claude/claude_desktop_config.json` |
| **macOS** | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| **Windows** | `%APPDATA%\Claude\claude_desktop_config.json` |

#### HTTP Configuration

```json
{
  "mcpServers": {
    "ddg-search": {
      "transport": "http",
      "url": "http://localhost:8020/mcp"
    }
  }
}
```

#### SSE Configuration

```json
{
  "mcpServers": {
    "ddg-search": {
      "transport": "sse",
      "url": "http://localhost:8020/sse"
    }
  }
}
```

> 💡 **Tip:** Replace `localhost` with your server's IP address for remote access.

### Claude CLI

Configure in `~/.config/claude-cli/config.json`:

```json
{
  "mcpServers": {
    "ddg-search": {
      "transport": "http",
      "url": "http://host-ip:8020/mcp"
    }
  }
}
```

### Codex CLI

Configure in `~/.codex/config.json`:

```json
{
  "mcpServers": {
    "ddg-search": {
      "transport": "http",
      "url": "http://host-ip:8020/mcp"
    }
  }
}
```

### Codeium (Windsurf)

Configure in `.codeium/mcp_settings.json`:

```json
{
  "mcpServers": {
    "ddg-search": {
      "transport": "http",
      "url": "http://host-ip:8020/mcp"
    }
  }
}
```

### Cursor

Configure in `~/.cursor/mcp.json` or via **Settings → Features → MCP Servers**:

```json
{
  "mcpServers": {
    "ddg-search": {
      "transport": "http",
      "url": "http://host-ip:8020/mcp"
    }
  }
}
```

---

## Available Tools

The DuckDuckGo Search MCP Server provides four powerful tools for web search and content extraction:

### 🔍 Web Search Tool

**Tool Name:** `web-search`

Search the web using DuckDuckGo with customizable result limits and pagination.

**Parameters:**
- `query` (string, **required**): The search query
- `page` (integer, optional, default: 1): Page number for pagination
- `numResults` (integer, optional, default: 10): Number of results to return (1-20)

**Example Usage:**
```
Search the web for "climate change solutions"
Find recent articles about "machine learning applications" page 2
Search for "docker best practices" with 15 results
```

---

### 🧠 Felo AI Search Tool

**Tool Name:** `felo-search`

Get AI-powered responses and explanations using Felo AI's intelligent search engine.

**Parameters:**
- `query` (string, **required**): The search query or prompt
- `stream` (boolean, optional, default: false): Whether to stream the response

**Example Usage:**
```
Search Felo AI for "Explain quantum computing in simple terms"
Ask Felo: "What are the benefits of containerization?"
Get AI explanation for "difference between REST and GraphQL"
```

---

### 📄 Fetch URL Tool

**Tool Name:** `fetch-url`

Extract and process content from any URL with smart filtering and customizable options.

**Parameters:**
- `url` (string, **required**): The URL to fetch
- `maxLength` (integer, optional, default: 10000): Maximum content length in characters
- `extractMainContent` (boolean, optional, default: true): Extract only main content area
- `includeLinks` (boolean, optional, default: true): Include link text in output
- `includeImages` (boolean, optional, default: true): Include image alt text
- `excludeTags` (array, optional): HTML tags to exclude (e.g., `["script", "style"]`)

**Example Usage:**
```
Fetch the content from "https://example.com/article"
Get main content from "https://blog.example.com" without images
Extract text from "https://news.site.com" excluding navigation
```

---

### 📊 URL Metadata Tool

**Tool Name:** `url-metadata`

Extract structured metadata from any URL including Open Graph, Twitter Cards, and standard meta tags.

**Parameters:**
- `url` (string, **required**): The URL to extract metadata from

**Example Usage:**
```
Get metadata for "https://example.com"
Extract title and description from "https://blog.example.com/post"
Retrieve Open Graph data from "https://news.site.com/article"
```

---

## Network Configuration

### Standard Bridge Network

Default networking mode for simple, isolated deployments:

```yaml
services:
  ddg-mcp:
    image: mekayelanik/ddg-mcp:stable
    container_name: ddg-mcp
    restart: unless-stopped
    ports:
      - "8020:8020"
    environment:
      - PORT=8020
```

**Access:** `http://localhost:8020/mcp` or `http://host-ip:8020/mcp`

#### Benefits
- ✅ Container isolation from host network
- ✅ Easy to configure and manage
- ✅ Works across all platforms

### Host Network

Use the host's network stack directly for maximum performance:

```yaml
services:
  ddg-mcp:
    image: mekayelanik/ddg-mcp:stable
    network_mode: host
    environment:
      - PORT=8020
```

**Access:** `http://localhost:8020/mcp`

#### Benefits
- ✅ Maximum performance - no NAT overhead
- ✅ No port mapping needed
- ⚠️ Linux only - not available on Docker Desktop

### MACVLAN Network (Advanced)

For dedicated IP addresses on your LAN:

```yaml
services:
  ddg-mcp:
    image: mekayelanik/ddg-mcp:stable
    mac_address: "AB:BC:CD:DE:EF:02"
    networks:
      macvlan-net:
        ipv4_address: 192.168.1.101

networks:
  macvlan-net:
    driver: macvlan
    driver_opts:
      parent: eth0
    ipam:
      config:
        - subnet: 192.168.1.0/24
          gateway: 192.168.1.1
```

**Access:** `http://192.168.1.101:8020/mcp`

---

## Updating

### Docker Compose Method

```bash
# Pull latest images
docker compose pull

# Recreate containers with new images
docker compose up -d

# Remove old images
docker image prune -f
```

### Docker CLI Method

```bash
# Pull the latest image
docker pull mekayelanik/ddg-mcp:stable

# Stop and remove the old container
docker stop ddg-mcp
docker rm ddg-mcp

# Start new container (use your original docker run command)
docker run -d \
  --name=ddg-mcp \
  --restart=unless-stopped \
  -p 8020:8020 \
  -e PORT=8020 \
  mekayelanik/ddg-mcp:stable

# Clean up old images
docker image prune -f
```

---

## Troubleshooting

### Pre-Flight Checklist

Before reporting an issue, please verify:

- ✅ Docker Engine is version 23.0 or higher
- ✅ Port 8020 is not in use by another service
- ✅ Container has been given sufficient time to start
- ✅ Configuration follows the examples provided
- ✅ Latest stable image tag is being used

### Common Issues & Solutions

#### Container Won't Start

```bash
# Check Docker version
docker --version  # Should be 23.0+

# Verify port availability
sudo lsof -i :8020

# Check container logs
docker logs ddg-mcp

# Verify image integrity
docker pull mekayelanik/ddg-mcp:stable
```

#### Permission Denied Errors

```bash
# Get your user/group IDs
id $USER

# Update configuration
environment:
  - PUID=1000  # Your UID
  - PGID=1000  # Your GID
```

#### Client Cannot Connect

```bash
# Test local connectivity
curl http://localhost:8020/mcp

# Test health endpoint
curl http://localhost:8020/healthz

# Check firewall rules
sudo ufw status

# Verify container networking
docker inspect ddg-mcp | grep IPAddress
```

#### CORS Errors

```yaml
# For development - allow all origins
environment:
  - CORS=*

# For production - specify exact origins
environment:
  - CORS=https://yourdomain.com
```

#### Search Results Not Returning

```bash
# Check internet access
docker exec ddg-mcp ping -c 3 8.8.8.8

# Check DNS resolution
docker exec ddg-mcp nslookup duckduckgo.com

# Review logs
docker logs ddg-mcp --tail 50

# Restart container
docker restart ddg-mcp
```

---

## Additional Resources

### Documentation & Guides

- 📚 [NPM Package: @oevortex/ddg_search](https://www.npmjs.com/package/@oevortex/ddg_search)
- 📦 [GitHub Repository](https://github.com/OEvortex/ddg_search)
- 🎥 [YouTube Channel: @OEvortex](https://youtube.com/@OEvortex)
- 🔧 [MCP Inspector Tool](https://github.com/modelcontextprotocol/inspector)
- 🌟 [Glama AI MCP Server Registry](https://glama.ai/mcp/servers/@OEvortex/ddg_search)

### Docker Resources

- 🐳 [Docker Compose Best Practices](https://docs.docker.com/compose/production/)
- 🌐 [Docker Networking Overview](https://docs.docker.com/network/)
- 🛡️ [Docker Security Best Practices](https://docs.docker.com/engine/security/)

---

## Support & Contributing

### Getting Help

#### Docker Image Issues

For issues specific to this Docker image:
- **GitHub Issues:** [ddg-mcp-docker/issues](https://github.com/MekayelAnik/ddg-mcp/issues)
- **Discussions:** [ddg-mcp-docker/discussions](https://github.com/MekayelAnik/ddg-mcp/discussions)

#### Package Issues

For DuckDuckGo Search MCP functionality:
- **GitHub:** [OEvortex/ddg_search/issues](https://github.com/OEvortex/ddg_search/issues)
- **YouTube:** [@OEvortex](https://youtube.com/@OEvortex)

### Contributing

We welcome contributions! Here's how you can help:

1. **Report Bugs:** Use GitHub Issues with detailed information
2. **Suggest Features:** Open a discussion or feature request
3. **Improve Documentation:** Submit PRs for documentation updates
4. **Test Beta Releases:** Help test new features before stable release

---

## License

This Docker image is provided under the GPLv3.0. See [LICENSE](https://github.com/MekayelAnik/duckduckgo-mcp-docker/refs/heads/main/LICENSE) file for details.

The ddg_search MCP NPM package ([@oevortex/ddg_search](https://www.npmjs.com/package/@oevortex/ddg_search)) is licensed under Apache License 2.0.

---

## Disclaimer

This is an unofficial Docker image for [@oevortex/ddg_search](https://www.npmjs.com/package/@oevortex/ddg_search). Not affiliated with DuckDuckGo, Felo AI, or Anthropic.

**Web Scraping Notice:** This tool performs web scraping on public search results. Users are responsible for complying with website terms of service, robots.txt directives, rate limiting, ethical scraping practices, and local laws.

**Privacy:** This Docker image does not collect, store, or transmit your search queries or personal data. All searches are performed directly against public search engines.

---

<div align="center">

[⬆️ Back to Top](#duckduckgo-search-mcp-server)

</div>