# AgentCAD Workflows

## Overview

This document describes the n8n workflows for the AgentCAD project.

## Workflow List

### AgentCAD (Main Workflow)

- **Workflow ID**: `ueHbmJPw9U4FTRk6`
- **Status**: Inactive (blank template)
- **Created**: 2026-01-11
- **File**: [agentcad-workflow.json](../workflows/agentcad-workflow.json)

**Description**:
Main AgentCAD workflow for AI-driven CAD automation using Onshape. This workflow enables AI agents to programmatically create, modify, and manage 3D CAD models via the Onshape REST API.

**Purpose**:
Enable AI agents to:
- Create CAD documents and Part Studios in Onshape
- Execute FeatureScript commands to generate 3D geometry
- Manage parametric features (extrude, fillet, revolve, etc.)
- Export models in various formats (STL, STEP, IGES)
- Automate CAD model generation based on specifications

**Technology Stack**:
- **CAD Platform**: Onshape (cloud-based CAD)
- **API**: Onshape REST API
- **Modeling Language**: FeatureScript
- **Automation**: n8n workflow
- **Authentication**: Onshape API Keys

**Planned Workflow Nodes**:
1. **Trigger**: Webhook/Manual trigger to initiate CAD generation
2. **Input Parsing**: Parse CAD requirements from AI agent
3. **Authentication**: Onshape API credential validation
4. **Document Creation**: Create new Onshape document
5. **Part Studio Setup**: Initialize Part Studio workspace
6. **FeatureScript Execution**: Execute CAD operations via API
7. **Validation**: Verify model creation success
8. **Export**: Generate output files (STL, STEP, etc.)
9. **Response**: Return model URL and metadata to agent

**Integration Documentation**: See [ONSHAPE_INTEGRATION.md](ONSHAPE_INTEGRATION.md) for detailed Onshape setup and API information.

**Current Status**: Planning phase - workflow design in progress.

## Workflow Development Guidelines

1. **Naming Convention**: Use descriptive names with version numbers
2. **Documentation**: Each workflow should have clear documentation
3. **Version Control**: Export workflows as JSON to `workflows/` directory
4. **Testing**: Test workflows thoroughly before deploying to production

## Workflow Storage

- Workflows are stored as JSON files in the `workflows/` directory
- Each workflow file should be named: `workflow-name-v1.json`
- Keep workflows organized and documented
