# Ekant — Privacy-Focused Metasearch Engine

A self-hosted, privacy-focused metasearch engine powered by **SearXNG**, containerized with **Docker**, and customized with a minimal interface.

Ekant brings search results from multiple search providers into one interface while reducing direct exposure of users' search queries to individual search engines.

> **Ekant** is a personal learning and self-hosting project built on top of the open-source SearXNG project.

## Features

- **Metasearch:** Aggregates results from multiple search engines.
- **Privacy-focused:** Routes search requests through a self-hosted SearXNG instance.
- **Custom branding:** Personalized homepage with the Ekant name.
- **Autocomplete:** Google-powered search suggestions.
- **Safe Search:** Strict filtering enabled by default.
- **Image proxy:** Supports proxying search-result images.
- **Containerized deployment:** Runs locally using Docker Compose.
- **Configurable:** Search engines, interface preferences, and server settings can be customized.

## Tech Stack

| Technology | Purpose |
|---|---|
| SearXNG | Open-source metasearch engine |
| Docker | Containerization |
| Docker Compose | Multi-container orchestration |
| Valkey | Available as a supporting service for SearXNG |
| Python | SearXNG backend runtime |
| YAML | Application and deployment configuration |
| HTML / CSS | Homepage customization |
| WSL2 | Linux container environment on Windows |

## Architecture

```text
                  User / Browser
                        |
                        v
              Ekant (localhost:8080)
                        |
                        v
                  SearXNG Core
                        |
             +----------+----------+
             |          |          |
             v          v          v
           Google     Brave    DuckDuckGo
             |          |          |
             +----------+----------+
                        |
                        v
                Aggregated Results
                        |
                        v
                  User / Browser
```

Ekant acts as an intermediary between the browser and configured search providers.

Search providers generally receive requests from the SearXNG server rather than directly from the user's browser. However, this does not provide complete anonymity: external links, autocomplete services, browser behavior, and deployment configuration can affect privacy.

## Getting Started

### Prerequisites

- Git
- Docker Desktop
- Docker Compose
- WSL2 (recommended for Windows users)

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/Ekant.git
cd Ekant
```

Replace `YOUR_USERNAME` with your GitHub username.

### 2. Configure environment variables

Copy the example environment file:

**PowerShell:**

```powershell
Copy-Item .env.example .env
```

**Linux / macOS:**

```bash
cp .env.example .env
```

### 3. Configure SearXNG

Create `core-config/settings.yml`:

```yaml
use_default_settings: true

general:
  instance_name: "Ekant"

search:
  safe_search: 2
  autocomplete: "google"

server:
  secret_key: "REPLACE_WITH_A_LONG_RANDOM_SECRET"
  image_proxy: true

ui:
  templates_path: "/etc/searxng/templates"
```

Generate a unique secret key before starting the application. Never commit your real secret key.

The custom template directory must contain the required SearXNG Simple theme templates for the homepage customization to work. Keep the templates compatible with the SearXNG image version you deploy.

### 4. Start the application

```bash
docker compose up -d
```

### 5. Open Ekant

Visit:

**http://localhost:8080**

### 6. Stop the application

```bash
docker compose down
```

## Project Structure

```text
searxng/
├── core-config/
│   ├── templates/
│   │   └── simple/
│   │       ├── index.html
│   │       └── ...
│   └── settings.yml       # Local configuration (ignored)
├── .env                   # Local environment (ignored)
├── .env.example
├── .gitignore
├── docker-compose.yml
└── README.md
```

## What I Learned

This project helped me explore practical concepts beyond application development:

- How DNS resolution works inside Docker containers.
- How HTTP and HTTPS requests travel between services.
- TLS connections and certificate verification.
- Docker networking and container isolation.
- How metasearch engines retrieve and aggregate results.
- The difference between search privacy and complete anonymity.
- Debugging Python virtual environments inside containers.
- Reading application logs and troubleshooting network requests.
- Customizing and configuring an existing open-source application.

## Privacy and Security

Ekant is currently intended for **local development and personal use**.

- Real environment variables and secret keys are excluded from version control.
- Search requests are processed through the SearXNG backend.
- HTTPS certificate verification is enabled for outgoing connections.
- The application is not intended to be exposed publicly without additional security configuration.

For public deployment, additional measures such as HTTPS, rate limiting, access controls where appropriate, and proper reverse-proxy configuration should be considered.

**Note:** Ekant is not a VPN, Tor replacement, or guarantee of anonymity.

## Future Improvements

- Improve the custom branding and interface.
- Add deployment documentation for a hosted instance.
- Configure production-ready rate limiting.
- Add automated health checks and monitoring.
- Improve deployment reproducibility through version pinning.

## Acknowledgements

Ekant is built on [SearXNG](https://github.com/searxng/searxng), an open-source metasearch engine.

Thanks to the SearXNG contributors and the wider open-source community.

## License and Attribution

The underlying SearXNG software and any copied theme assets remain subject to their respective upstream licenses. Refer to the [SearXNG repository](https://github.com/searxng/searxng) for licensing information.

This repository documents a customized self-hosted deployment; it is not an independently developed search engine.
