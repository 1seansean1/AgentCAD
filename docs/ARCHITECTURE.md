# AgentCAD Architecture Decisions

**Version**: 0.1.0
**Last Updated**: 2026-01-11
**Status**: Initial decisions for MVP

This document records architectural choices for the AgentCAD agentic CAD automation system. Each decision includes the current choice, rationale, and trade-offs.

---

## Governance Tier

**Decision**: Single owner (Sean Allen) with AI assistance
**Rationale**: Early stage project, fast iteration needed
**Trade-offs**: ✅ Fast decisions, clear accountability | ❌ Limited perspective
**Revisit**: When adding team members or external contributors

---

## Hosting Model

**Decision**: Distributed cloud services
- n8n: Railway (https://n8n-production-7a2c.up.railway.app)
- Onshape: SaaS cloud (https://cad.onshape.com)
- AI: Claude API (Anthropic)
- Repository: GitHub (https://github.com/1seansean1/AgentCAD)

**Rationale**: Leverage managed services, minimize infrastructure overhead
**Trade-offs**: ✅ Low maintenance, scalable | ❌ Multi-service dependencies, potential cost
**Revisit**: If cost becomes prohibitive or latency issues emerge

---

## Base Model + Configuration

**Decision**: Claude Sonnet 4.5 via n8n AI nodes
- Temperature: Default (0.7 recommended for CAD creativity/precision balance)
- Tools: Enabled for Onshape API calls
- Context: Per-session (no persistent memory initially)

**Rationale**: Sonnet 4.5 balances capability and cost, strong tool use, good at technical tasks
**Trade-offs**: ✅ Excellent reasoning, tool use | ❌ API costs, latency
**Alternatives Considered**: GPT-4, local models (rejected: cost/capability trade-off)
**Revisit**: v0.2.0 - evaluate cost vs. performance

---

## Agent Pattern

**Decision**: Single-agent with specialized tools (Phase 1)
- One agent orchestrates entire CAD workflow
- Tools: Onshape API endpoints (create doc, add features, export)
- Future: Multi-agent (design → validate → export) if complexity warrants

**Rationale**: Start simple, validate approach, avoid premature optimization
**Trade-offs**: ✅ Simpler implementation/debugging, lower latency | ❌ Limited parallelization, potential complexity as features grow
**Alternatives Considered**:
- Multi-agent from start (rejected: too complex for MVP)
- Pure FeatureScript generation (rejected: need validation layer)

**Revisit**: v0.2.0 or when single agent becomes unwieldy

---

## Tool Selection Strategy

**Decision**: Curated minimal tool set (Phase 1)
- Document creation (POST /documents)
- Part Studio creation (POST /partstudios)
- Feature execution (POST /partstudios/features - FeatureScript)
- Export (GET /export - STL, STEP formats)
- Document retrieval (GET /documents/:id)

**Rationale**: Start with core CAD operations, expand based on actual needs
**Trade-offs**: ✅ Simpler agent, easier to validate | ❌ Limited capabilities initially
**Expansion Plan**: Add tools based on usage patterns and user requests
**Revisit**: After first 10 successful CAD generations

---

## Integration Protocols

**Decision**: REST APIs only (Phase 1)
- n8n HTTP nodes for Onshape REST API
- n8n AI nodes for Claude API
- No MCP or A2A initially

**Rationale**: n8n provides sufficient orchestration, MCP adds complexity without clear benefit yet
**Trade-offs**: ✅ Simple, well-understood | ❌ Less modular than MCP
**Future Consideration**: MCP if exposing tools to external agents, A2A for multi-agent coordination
**Revisit**: v0.3.0 or when integrating with external agent systems

---

## Memory Architecture

**Decision**: Stateless per-request (Phase 1)
- No persistent memory across sessions
- Context limited to current n8n workflow execution
- Onshape provides version history (external memory)

**Rationale**: Simplify MVP, Onshape already tracks design history
**Trade-offs**: ✅ No database needed, simple | ❌ Can't learn from past designs, no user preferences
**Future Enhancement**:
- Phase 2: Session memory (recent conversation)
- Phase 3: Long-term memory (design patterns, user preferences)

**Revisit**: When users request "remember my last design" or pattern reuse

---

## Interoperability Stance

**Decision**: Onshape-first, export for interoperability
- Primary storage: Onshape cloud
- Interoperability: STEP, STL, IGES exports
- No direct CAD-to-CAD translation initially

**Rationale**: Onshape API is rich, cloud-based, exports cover 90% of needs
**Trade-offs**: ✅ Full Onshape feature set | ❌ Vendor lock-in risk
**Alternatives Considered**: Multi-CAD support (rejected: too complex for MVP)
**Revisit**: If users require native Fusion 360, SolidWorks, etc. support

---

## Observability Stack

**Decision**: Basic logging (Phase 1)
- n8n execution logs (built-in)
- Onshape API response logging
- Manual error review

**Rationale**: n8n provides adequate visibility for MVP
**Trade-offs**: ✅ No additional infrastructure | ❌ Limited analytics, no tracing
**Future Enhancement**:
- Structured logging (JSON)
- Error aggregation (Sentry or similar)
- Agent decision tracing

**Revisit**: When debugging becomes difficult or need analytics

---

## Error Handling Strategy

**Decision**: Fail-fast with descriptive errors
- Validate inputs before Onshape API calls
- Return clear error messages to user
- No automatic retry initially (due to CAD state mutation)
- Human-in-loop for error resolution

**Rationale**: CAD operations are stateful; bad retries can corrupt designs
**Trade-offs**: ✅ Predictable, safe | ❌ Manual intervention required
**Retry Policy**: Only for transient network errors, not CAD validation errors
**Revisit**: Add smart retry logic after observing error patterns

---

## Security & Guardrails

**Decision**: API key authentication + input validation
- Onshape API keys stored in .env (never committed)
- Input sanitization (prevent injection attacks)
- No destructive operations without confirmation (delete, etc.)
- Rate limiting: Respect Onshape API limits (no explicit enforcement initially)

**Rationale**: Protect credentials, prevent accidental damage
**Trade-offs**: ✅ Basic security, prevents accidents | ❌ No advanced threat detection
**Guardrails**:
- No DELETE operations in Phase 1
- Validate all numeric inputs (dimensions, counts)
- Limit max feature count per design (100 initially)

**Revisit**: Add formal rate limiting, audit logging if usage scales

---

## Human-in-the-Loop (HITL) Design

**Decision**: Approval before creation (Phase 1)
- Agent generates CAD plan (features list)
- Human reviews and approves before Onshape API calls
- Post-creation review (manual check in Onshape)

**Rationale**: CAD errors costly to fix, human judgment valuable for design quality
**Trade-offs**: ✅ Safe, quality control | ❌ Slower, not fully automated
**Automation Path**: Reduce HITL as confidence grows (telemetry-based)
**Revisit**: Phase 2 - allow "trusted" operations without approval

---

## Cost Controls

**Decision**: Manual monitoring (Phase 1)
- Track Claude API usage monthly
- Track Onshape API calls (free tier: unlimited for now)
- No automated limits initially

**Rationale**: Free tier sufficient for MVP, premature to add cost infrastructure
**Trade-offs**: ✅ No overhead | ❌ Could overspend if usage spikes
**Thresholds**: Set alerts at $50/month Claude usage
**Revisit**: Implement hard limits if approaching $100/month or leaving free tier

---

## Context Management

**Decision**: Single-turn initially, plan for multi-turn
- Phase 1: Each CAD request is independent
- Phase 2: Session context (conversation history within n8n workflow)
- Store context in n8n workflow variables (ephemeral)

**Rationale**: Most CAD requests are one-shot, multi-turn adds complexity
**Trade-offs**: ✅ Stateless simplicity | ❌ Can't iterate on designs easily
**Context Size**: Limit to 10 turns or 4000 tokens (whichever first)
**Revisit**: When users request "modify the bracket from earlier"

---

## Testing & Evaluation Framework

**Decision**: Manual testing (Phase 1)
- Test each workflow change in n8n UI
- Validate CAD output in Onshape manually
- Track success rate informally

**Rationale**: Small scale, manual testing feasible
**Trade-offs**: ✅ No test infrastructure needed | ❌ Not repeatable, no regression detection
**Future Enhancement**:
- Automated workflow tests (n8n test nodes)
- CAD validation suite (dimension checks, feature counts)
- Quality metrics (manufacturability scores)

**Revisit**: After 20+ CAD generations or first regression bug

---

## Deployment Pipeline

**Decision**: Manual deployment via n8n UI
- Edit workflows in n8n web interface
- Export to workflows/ directory for version control
- No CI/CD initially

**Rationale**: Rapid iteration, n8n UI is deployment interface
**Trade-offs**: ✅ Fast changes | ❌ No automated testing, rollback risk
**Version Control**: Git for workflow JSON, manual export
**Revisit**: Implement CI/CD if multiple developers or frequent breaking changes

---

## Latency Requirements

**Decision**: Best-effort, <30s target (Phase 1)
- Simple CAD operations: <10s (goal)
- Complex designs: <30s acceptable
- No SLA initially

**Rationale**: CAD design is not real-time, users tolerate seconds for quality
**Trade-offs**: ✅ No optimization pressure | ❌ Could be slow for simple tasks
**Bottlenecks**: Claude API latency (5-15s), Onshape API (1-5s per operation)
**Revisit**: If user complaints or latency exceeds 60s regularly

---

## Audit & Compliance Approach

**Decision**: Basic audit trail (Phase 1)
- All CAD operations logged in n8n
- Onshape maintains version history automatically
- No formal compliance requirements

**Rationale**: Personal project, no regulatory requirements
**Trade-offs**: ✅ No overhead | ❌ Can't prove what happened if needed
**Future**: Structured logging if used commercially or in regulated industry
**Revisit**: If project used in enterprise or regulated context

---

## Orchestration Tooling

**Decision**: n8n as primary orchestrator
- n8n workflows coordinate: Input → Agent → Onshape → Response
- n8n handles retries, error handling, logging
- No additional orchestration (Temporal, Airflow, etc.)

**Rationale**: n8n already in use, visual workflows, adequate for CAD automation
**Trade-offs**: ✅ Unified platform, visual debugging | ❌ Limited compared to dedicated orchestrators
**Alternatives Considered**: LangGraph, custom code (rejected: more complex)
**Revisit**: If workflows become too complex for n8n or need advanced features

---

## Data Pipeline (RAG)

**Decision**: No RAG initially
- No vector database
- No CAD pattern library
- No retrieval-augmented generation

**Rationale**: Start with direct API use, add RAG if pattern reuse becomes valuable
**Trade-offs**: ✅ Simpler | ❌ Can't leverage past designs
**Future Enhancement**:
- Phase 3: CAD pattern library (common brackets, enclosures)
- Vector DB for design search (Pinecone, Chroma)
- Retrieval of similar designs

**Revisit**: When users ask "make something like the bracket from project X"

---

## State Management

**Decision**: Onshape as source of truth
- CAD state lives in Onshape documents
- n8n workflows are stateless
- No local state persistence

**Rationale**: Onshape provides version control, collaboration, persistence
**Trade-offs**: ✅ No database to maintain | ❌ Dependent on Onshape availability
**Rollback**: Use Onshape version history
**Revisit**: If offline CAD editing becomes a requirement

---

## Validation Strategy

**Decision**: Basic validation (Phase 1)
- Check API responses for success
- Verify Onshape document created
- Manual visual inspection

**Rationale**: CAD quality subjective, manual review most reliable initially
**Trade-offs**: ✅ Simple | ❌ Not scalable, no automated quality checks
**Future Enhancement**:
- Automated dimension verification
- Manufacturability checks (wall thickness, aspect ratio)
- Structural analysis integration

**Revisit**: When generating >50 designs/month or quality issues emerge

---

## Caching Strategy

**Decision**: No caching (Phase 1)
- Every request creates new Onshape document
- No cached CAD patterns or common features

**Rationale**: Premature optimization, designs likely unique
**Trade-offs**: ✅ Simple, always fresh | ❌ Potential duplicate work
**Future Enhancement**: Cache common features (standard hole patterns, fillets)
**Revisit**: If users request same/similar designs frequently

---

## Rollback Mechanism

**Decision**: Onshape version history
- Onshape automatically versions documents
- Manual rollback via Onshape UI
- No automated rollback in n8n

**Rationale**: Onshape provides built-in version control
**Trade-offs**: ✅ Leverage existing feature | ❌ Manual process
**Procedure**: If error, access Onshape document → History → Restore version
**Revisit**: If automated rollback needed for programmatic workflows

---

## Decision Status

| Area | Status | Revisit Trigger |
|------|--------|----------------|
| Agent Pattern | ✅ Decided | v0.2.0 or complexity increase |
| Tool Selection | ✅ Decided | After 10 CAD generations |
| Memory Architecture | ✅ Decided | User requests past design access |
| HITL Design | ✅ Decided | Phase 2 - trusted ops |
| Testing Framework | ⚠️ Minimal | After first regression |
| RAG/Data Pipeline | ❌ Not Needed | Pattern reuse requests |
| Multi-Agent | 📋 Deferred | Single agent proves insufficient |

---

## Open Questions

1. **Export Format Preferences**: Which formats most common? (STL, STEP, IGES, others?)
2. **Design Iteration**: How to handle "modify this design" multi-turn requests?
3. **Batch Processing**: Will users want to generate multiple designs in one request?
4. **Design Library**: Should we build a library of reusable components?
5. **Collaboration**: Multi-user workflows or single-user focus?

---

## References

- [Onshape Integration Guide](ONSHAPE_INTEGRATION.md)
- [Workflow Documentation](WORKFLOWS.md)
- [n8n Instance](https://n8n-production-7a2c.up.railway.app)
- [GitHub Repository](https://github.com/1seansean1/AgentCAD)

---

**Revision History**
- 2026-01-11: Initial architecture decisions (v0.1.0)
