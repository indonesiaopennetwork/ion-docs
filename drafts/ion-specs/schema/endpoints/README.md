# `schema/endpoints/` — ION-introduced endpoints

This folder holds the request/response schemas for the two endpoints
ION adds to Beckn:

- `/raise` and its callbacks — grievance and complaint endpoint
- `/reconcile` and its callbacks — financial true-up endpoint

Both endpoints follow Beckn's request/callback pattern with the same
Context, signing, and acknowledgement semantics as upstream Beckn. They
are additions, not replacements; every Beckn endpoint behaves exactly
as upstream Beckn specifies.

See the README for why each endpoint exists and what it does:
[../../README.md](../../README.md#beckn-extensions-ion-introduces).

## Planned schemas

```
schema/endpoints/
├── raise/v1/
│   ├── attributes.yaml      ← request/response schema
│   ├── README.md            ← what /raise does, when to use it
│   └── examples/
│       ├── raise-grievance.json
│       ├── on-raise-ack.json
│       └── ...
└── reconcile/v1/
    ├── attributes.yaml
    ├── README.md
    └── examples/
        ├── reconcile-request.json
        ├── on-reconcile-final.json
        └── ...
```

Each endpoint specification will include:
- The full request schema
- The acknowledgement (Ack/Nack) schema
- The callback request schema (`/on_raise`, `/on_reconcile`)
- Companion endpoints if any (`/raise_status`, `/raise_details`, etc.)
- Worked example payloads for common scenarios

## What is here today

The folder structure is in place; the schemas inside are not yet
drafted.
