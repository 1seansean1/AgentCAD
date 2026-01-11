# Changelog

All notable changes to the AgentCAD project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Onshape integration planning and documentation
  - `docs/ONSHAPE_INTEGRATION.md` - Comprehensive Onshape API integration guide
  - Onshape chosen as web-based CAD platform for AI agents
  - Updated workflow documentation with Onshape integration details
  - Added Onshape credentials to `.env.example`
- Onshape API setup and verification
  - API credentials generated and securely stored in `.env`
  - API connection tested and verified successfully
  - Full API access confirmed (OAuth2 scopes: 4127)
  - User permissions: OWNER level with READ, WRITE, DELETE, COPY, EXPORT, RESHARE
  - Account: Sean Allen (sean.p.allen9@gmail.com) - Free plan with DEVELOPER role
  - Updated `docs/ONSHAPE_INTEGRATION.md` with setup status

### Planned
- Configure Onshape credentials in n8n workflow
- Build proof-of-concept workflow nodes:
  - Document creation
  - FeatureScript execution
  - Model export
- Implement AI agent CAD generation workflow
- Add workflow testing and validation

## [0.1.0] - 2026-01-11

### Added
- Initial project setup and repository structure
- GitHub repository created at https://github.com/1seansean1/AgentCAD
- Project documentation structure in `docs/` directory
  - `docs/SETUP.md` - Setup and configuration guide
  - `docs/WORKFLOWS.md` - Workflow documentation
- Environment configuration files
  - `.env.example` - Template for environment variables
  - `.env` - Local environment configuration (not committed)
- n8n API integration
  - API credentials configured
  - Connection to n8n instance verified (https://n8n-production-7a2c.up.railway.app)
- n8n configuration tracking
  - `n8n-config.json` - Project and workflow configuration
- Blank "AgentCAD" workflow created
  - Workflow ID: `ueHbmJPw9U4FTRk6`
  - Exported to `workflows/agentcad-workflow.json`
  - Status: Inactive (template ready for development)
- Repository security configuration
  - `.gitignore` updated to exclude sensitive files (.env, credentials)
  - API keys properly secured
- Comprehensive README.md with:
  - Project overview and structure
  - Quick start guide
  - Setup instructions
  - Security best practices

### Project Structure
```
AgentCAD/
├── docs/              # Project documentation
│   ├── SETUP.md      # Setup guide
│   └── WORKFLOWS.md  # Workflow documentation
├── workflows/         # n8n workflow JSON exports
│   └── agentcad-workflow.json
├── .env              # Environment variables (gitignored)
├── .env.example      # Environment template
├── .gitignore        # Git ignore rules
├── CHANGELOG.md      # This file
├── n8n-config.json   # n8n configuration
└── README.md         # Project overview
```

### Configuration
- n8n Instance: https://n8n-production-7a2c.up.railway.app
- n8n Project ID: vrikaMvfkO4wce9w
- Repository: https://github.com/1seansean1/AgentCAD

### Notes
- Project initialized with git version control
- n8n API access confirmed and working
- All sensitive credentials properly secured in .env (not committed)
- Blank workflow ready for implementation

---

## Version History

### [0.1.0] - 2026-01-11
Initial release with project setup, documentation, and blank workflow template.
