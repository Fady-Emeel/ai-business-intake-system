# AI Business Intake & Decision System (Applied AI / AI Automation)

## Overview
This project converts messy customer messages (emails / chat / tickets) into **validated, automation-ready JSON**.
It is designed for real business operations (banking / fintech / enterprise support) where reliability, auditability,
and safe routing are critical.

**Input:** unstructured text  
**Output:** structured JSON with:
- intent
- priority (low/medium/high)
- extracted entities (name, phone, order_id, transaction_id, etc.)
- missing required fields
- recommended next action
- confidence score (0..1)
- trace_id (for audit logs)

## Why this matters (Business value)
Manual triage is slow and inconsistent. This system:
- reduces time spent on ticket classification
- standardizes intake across teams
- improves SLA by prioritizing urgent issues
- routes low-confidence / high-risk cases to humans (Responsible AI)

## Key Features
- **LLM → JSON extraction** with strict schema validation (Pydantic)
- **Retry + normalization** for robustness (handles broken JSON / wrong types)
- **Business rules**: required fields per intent
- **Multi-intent handling**: lowers confidence & guides next actions
- **Responsible AI policy layer**: human review for low confidence, escalation for high-risk intents
- **Trace IDs + logging** for auditability

## Example
### Input
"My transfer failed today and money was deducted. Name: Fady, phone: 01220000000."

### Output (example)
```json
{
  "intent": "payment_failure",
  "priority": "high",
  "entities": {"name":"Fady","phone":"01220000000"},
  "missing_required_fields": ["transaction_id"],
  "recommended_next_action": "investigate_transfer_failure",
  "confidence": 0.8,
  "trace_id": "2b8d2f1c-...."
}
