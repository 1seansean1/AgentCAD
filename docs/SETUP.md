# AgentCAD Setup Guide

## Prerequisites

- n8n account (cloud or self-hosted)
- n8n API access enabled
- Node.js (for local development, if needed)

## Configuration

1. Copy the `.env.example` file to `.env`:
   ```bash
   cp .env.example .env
   ```

2. Update the `.env` file with your credentials:
   - `N8N_API_URL`: Your n8n instance URL
   - `N8N_API_KEY`: Your n8n API key (get from n8n Settings > API)
   - `N8N_PROJECT_ID`: Your n8n project ID
   - `PROJECT_NAME`: Project name (default: AgentCAD)

## Security Notes

- Never commit the `.env` file to version control
- The `.env` file is already included in `.gitignore`
- Keep your API keys secure and rotate them regularly
- Use `.env.example` as a template for required environment variables

## Project Structure

```
AgentCAD/
├── docs/              # Project documentation
├── workflows/         # n8n workflow definitions (JSON)
├── .env              # Environment variables (not committed)
├── .env.example      # Template for environment variables
├── .gitignore        # Git ignore rules
└── README.md         # Project overview
```

## Next Steps

1. Define workflow requirements in `docs/WORKFLOWS.md`
2. Create workflows in n8n interface or via API
3. Export workflows to `workflows/` directory for version control
