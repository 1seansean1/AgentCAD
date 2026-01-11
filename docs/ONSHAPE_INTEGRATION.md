# Onshape Integration Guide

## Overview

AgentCAD uses Onshape as the web-based CAD platform for AI agents to create and manipulate 3D models programmatically. Onshape's cloud-native architecture and comprehensive REST API make it ideal for automated CAD workflows.

## Why Onshape?

- **Cloud-native**: Fully web-based, accessible from anywhere
- **Robust API**: Comprehensive REST API for all CAD operations
- **FeatureScript**: Programming language for creating CAD features
- **Real-time collaboration**: Multiple agents/users can work simultaneously
- **OAuth2 & API Keys**: Secure authentication mechanisms
- **Version control**: Built-in version history and branching

## Onshape API Capabilities

### Core Features

1. **Create & Modify Parts**: Programmatically create parts, features, and assemblies
2. **FeatureScript Execution**: Execute custom FeatureScript code via API
3. **Document Management**: Create, read, update documents and workspaces
4. **Translation**: Export to various CAD formats (STEP, STL, IGES, etc.)
5. **Metadata**: Manage properties, custom tables, and BOM data
6. **Real-time Access**: Query and modify CAD data in real-time

### API Methods

- **GET**: Retrieve information (documents, parts, features, metadata)
- **POST**: Create new elements, execute FeatureScript, create features
- **DELETE**: Remove elements and features

## Authentication

Onshape supports two authentication methods:

### 1. API Keys (Recommended for Server-to-Server)
- Generate from Onshape account settings
- Access Key + Secret Key pair
- Suitable for n8n workflows

### 2. OAuth2
- For user-authorized applications
- More complex but provides user-level permissions

## Key Resources

### Official Documentation
- [Onshape Developer Portal](https://onshape-public.github.io/docs/)
- [REST API Introduction](https://onshape-public.github.io/docs/api-intro/)
- [API Integration Guide](https://www.onshape.com/en/features/integrations)

### Tools
- **Glassworks API Explorer**: https://cad.onshape.com/glassworks
  - Test API endpoints without code
  - Browse all available API calls
  - Generate sample requests

### Code Examples
- [GitHub - Onshape Public Repositories](https://github.com/onshape-public)
- [Python API Snippets](https://colab.research.google.com/github/PTC-Education/PTC-API-Playground/blob/main/Onshape_API_Snippets.ipynb)

## FeatureScript

FeatureScript is Onshape's domain-specific language for creating CAD features:

- **Built-in**: All standard Onshape features (Extrude, Fillet, etc.) are written in FeatureScript
- **Custom Features**: Create custom parametric features
- **API Integration**: Execute FeatureScript via REST API
- **Part Studio Focus**: Primarily for Part Studios (not Assemblies)

**Documentation**: [FeatureScript Introduction](https://cad.onshape.com/FsDoc/)

## AgentCAD Integration Plan

### Phase 1: Setup & Authentication
1. Create Onshape account (if not already created)
2. Generate API keys
3. Configure n8n credentials for Onshape API
4. Test basic API connectivity

### Phase 2: Basic Operations
1. Create documents programmatically
2. Create Part Studios
3. Execute simple FeatureScript commands
4. Export models (STL, STEP)

### Phase 3: AI Agent Integration
1. Design agent prompts for CAD operations
2. Create n8n workflow nodes for:
   - Document creation
   - Feature execution
   - Model verification
   - Export and delivery
3. Implement error handling and validation

### Phase 4: Advanced Features
1. Parametric model generation
2. Assembly automation
3. BOM generation
4. Integration with external systems

## Workflow Architecture

```
AI Agent Request
    ↓
n8n Workflow (AgentCAD)
    ↓
Parse CAD Requirements
    ↓
Generate FeatureScript/API Calls
    ↓
Onshape REST API
    ↓
Create/Modify CAD Model
    ↓
Export & Validate
    ↓
Return Result to Agent
```

## Security Considerations

1. **API Key Storage**: Store Onshape API keys in `.env` file (never commit)
2. **Access Control**: Use appropriate Onshape permissions
3. **Rate Limiting**: Respect Onshape API rate limits
4. **Error Handling**: Implement robust error handling for API failures

## Next Steps

1. Set up Onshape account and generate API keys
2. Configure Onshape credentials in n8n
3. Test basic API connectivity via Glassworks
4. Define first CAD automation use case
5. Build proof-of-concept workflow

## Resources & References

- [Onshape API: 6 Key Insights](https://www.onshape.com/en/blog/cloud-native-cad-api-insights-engineers)
- [API for Beginners (Onshape Forum)](https://forum.onshape.com/discussion/22988/api-for-beginners)
- [Using API with FeatureScript](https://forum.onshape.com/discussion/26720/how-to-use-the-api-to-execute-a-featurescript-to-create-a-custom-feature)

---

**Last Updated**: 2026-01-11
