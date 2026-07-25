# AI Request Triage Template

Use this before accepting a request that asks for a specific API, field,
entity, event, dashboard, or integration.

```text
The user proposed this solution: [REQUEST].

Do request triage before proposing implementation.

Read:
1. IceBot-Product/delivery/playbooks/REQUEST_TRIAGE.md
2. Relevant FLOW-* entries and product journey
3. Linked backend flow/API/IoT contract
4. Target repository only after identifying the likely owning boundary

Do not create or recommend a new endpoint merely because the requested shape
does not appear in OPERATION_CATALOG.json.

Return exactly:
1. Plain-language operating goal, actor, and resource scope.
2. Existing flow/contract that may already satisfy it.
3. Why the proposed solution fits or bypasses ownership, authorization,
   lifecycle, idempotency, integrity, or audit requirements.
4. Recommendation: use existing flow | missing read model | missing command |
   open product decision.
5. Only if genuinely missing: smallest contract proposal and the decisions
   required before implementation.
```

## Related Sources

- [Request Triage Protocol](REQUEST_TRIAGE.md)
- [AI Implementation Request Template](AI_IMPLEMENTATION_REQUEST.md)
