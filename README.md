# AgentCAD

A project integrating n8n workflows with automated CAD processes.

## Overview

AgentCAD uses n8n workflow automation to streamline and automate CAD-related processes. This repository contains workflow definitions, documentation, and configuration for the AgentCAD n8n project.

## Project Structure

```
AgentCAD/
├── docs/              # Project documentation
│   ├── SETUP.md      # Setup and configuration guide
│   └── WORKFLOWS.md  # Workflow descriptions
├── workflows/         # n8n workflow JSON exports
├── .env              # Environment variables (not committed)
├── .env.example      # Template for environment variables
├── CHANGELOG.md      # Project change history
├── n8n-config.json   # n8n project configuration
├── .gitignore        # Git ignore rules
└── README.md         # This file
```

## Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/1seansean1/AgentCAD.git
   cd AgentCAD
   ```

2. **Configure environment**
   ```bash
   cp .env.example .env
   # Edit .env with your n8n credentials
   ```

3. **Read the documentation**
   - [Setup Guide](docs/SETUP.md) - Configuration and setup instructions
   - [Workflows](docs/WORKFLOWS.md) - Workflow descriptions and guidelines

## Prerequisites

- n8n account (cloud or self-hosted)
- n8n API access enabled
- Git for version control

## n8n Instance

- **URL**: https://n8n-production-7a2c.up.railway.app
- **Project ID**: vrikaMvfkO4wce9w
- **API Documentation**: https://docs.n8n.io/api/

## Security

- Never commit `.env` files or API keys
- Keep credentials secure and rotate regularly
- Use `.env.example` for sharing required environment variables

## Documentation

All project documentation is located in the [docs/](docs/) directory:
- [SETUP.md](docs/SETUP.md) - Setup and configuration
- [WORKFLOWS.md](docs/WORKFLOWS.md) - Workflow definitions and requirements
- [ONSHAPE_INTEGRATION.md](docs/ONSHAPE_INTEGRATION.md) - Onshape CAD platform integration guide
- [ARCHITECTURE.md](docs/ARCHITECTURE.md) - Architecture decisions and design rationale
- [CHANGELOG.md](CHANGELOG.md) - Project change history and version tracking

## Contributing

Workflow changes should be:
1. Developed and tested in n8n
2. Exported as JSON to `workflows/` directory
3. Documented in `docs/WORKFLOWS.md`
4. Committed with descriptive messages

## License

TBD
