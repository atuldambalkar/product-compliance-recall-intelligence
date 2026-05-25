# Intent Document — Product Compliance & Recall Intelligence

## 1. System Purpose

Provide an Agentic AI-powered application that enables product safety, regulatory, and supply chain teams at Retail and CPG organizations to rapidly investigate consumer complaint signals, trace multi-hop product-supplier-facility relationships via a knowledge graph, assess recall risk with evidence-grounded reasoning, and produce structured risk assessments — all enforced by Amazon Bedrock Guardrails for compliance, grounding, and safety.

## 2. System Context

### Primary Actors

| Actor | Role |
|-------|------|
| Product Safety Analyst | Investigates complaint spikes, assesses recall risk, initiates investigations |
| Regulatory Compliance Officer | Validates regulatory alignment (FDA/USDA/CPSC), reviews risk assessments |
| Supply Chain Manager | Traces supplier/facility connections, evaluates blast radius across distribution |

### External Systems

| System | Interaction |
|--------|-------------|
| Amazon Bedrock (Strands Agent SDK) | Agentic reasoning, LLM inference, guardrails enforcement |
| Amazon Neptune / Graph Database | Stores and traverses the product-ingredient-supplier-facility knowledge graph |
| Amazon Bedrock Knowledge Base | RAG retrieval of regulatory documents, SOPs, and internal policies |
| openFDA API | External validation of recalls, adverse events, enforcement actions |
| CPSC Complaint Database | Consumer product safety complaint records |
| Front-end (ReactJS) | Web-based UI for analysts to interact with the agent |
| Back-end APIs (Java 21 / Spring Boot / Spring AI) | Middle-tier orchestration, business logic, API gateway |

## 3. Key Behaviors

1. **Signal Detection & Alerting** — Detect consumer complaint spikes for a product SKU in a specific region and surface them to analysts for investigation.

2. **Multi-Hop Graph Traversal** — Trace relationships from a flagged SKU through ingredient lots, supplier facilities, audit histories, and onward to other potentially affected SKUs.

3. **Policy & Regulation Retrieval** — Fetch relevant FDA, USDA, and CPSC regulations along with internal SOPs via RAG to contextualize the investigation.

4. **External Data Validation** — Query openFDA and CPSC databases to cross-reference complaint patterns, existing recalls, and enforcement actions.

5. **Risk Synthesis & Reporting** — Produce a structured risk assessment including blast radius, probable root cause hypothesis, affected product/facility list, and recommended actions.

6. **Guardrails Enforcement** — Ensure every agent response is grounded in retrieved evidence, PII is redacted, hallucinations are blocked, and final recall decisions are explicitly deferred to human reviewers.

7. **Conversational Investigation** — Allow analysts to ask follow-up questions, refine scope, and drill into specific nodes of the knowledge graph through natural language interaction.

## 4. Constraints

### Technical Constraints

- Back-end: Java 21 with Spring Boot and Spring AI
- Front-end: ReactJS
- Agentic AI: Amazon Bedrock Strands Agent SDK
- Knowledge Graph: Amazon Neptune (or compatible graph database)
- RAG: Amazon Bedrock Knowledge Base
- All front-end to back-end API calls must be logged
- Guardrails must be applied to every agent response before surfacing to users

### Operational Constraints

- This is an MVP / workshop demonstration — not production-grade
- Recall decisions are never made autonomously; the system recommends, humans decide
- PII must be redacted from all agent outputs
- Responses must cite source evidence (graph nodes, documents, external records)

## 5. Assumptions

1. A pre-populated knowledge graph exists with representative product, ingredient, supplier, and facility data (synthetic or sample data for MVP).
2. Amazon Bedrock, Neptune, and Knowledge Base services are available in the target AWS region.
3. openFDA and CPSC APIs are publicly accessible and do not require special credentials beyond standard API keys.
4. Users have appropriate IAM/authentication credentials to access the application.
5. The MVP will use synthetic/sample data rather than real proprietary CPG data.
6. A single-tenant deployment is sufficient for the workshop demonstration.
7. The Strands Agent SDK supports the orchestration patterns required (tool use, multi-step reasoning, guardrails integration).

## 6. Success Criteria

| # | Criterion | Measure |
|---|-----------|--------|
| 1 | End-to-end investigation flow | Analyst can input a complaint signal and receive a structured risk assessment within a single conversational session |
| 2 | Multi-hop traversal accuracy | Agent correctly identifies >= 2 hops of connected entities (SKU -> ingredient -> supplier -> other SKUs) |
| 3 | Regulatory grounding | Every risk assessment cites at least one relevant regulation or SOP retrieved via RAG |
| 4 | External validation | Agent successfully queries openFDA or CPSC and incorporates results into the assessment |
| 5 | Guardrails effectiveness | 100% of agent responses pass guardrail checks (grounding, PII redaction, no autonomous recall decisions) |
| 6 | API logging | All front-end to back-end API calls are captured in logs with timestamp, endpoint, and response status |
| 7 | Response time | Agent produces a complete risk assessment within 60 seconds for the MVP dataset |

## 7. Out of Scope

- Production deployment, scaling, and high-availability architecture
- Real-time ingestion of live consumer complaint streams
- Integration with internal ERP, PLM, or QMS systems
- Automated recall execution or regulatory filing
- Multi-tenant access control and role-based permissions beyond basic auth
- Mobile application or offline access
- Training or fine-tuning custom foundation models
- Historical trend analysis and predictive recall modeling
- Internationalization / multi-language support
