### Claim Schema Vocab – Project Overview

This repository provides the canonical JSON and JSON-LD building blocks for Iden3 credentials, proofs, and related core data structures. It includes:

- Schemas: machine-validated shapes for credential payloads (JSON Schema)
- Contexts: semantic mappings for fields and proof types (JSON-LD)
- Vocab: human-readable definitions and examples for linked terms

### Repository layout

- `credentials/`: Vocabulary docs for common fields and domain-specific credentials (auth, KYC, identity, organization, professional, network). Each markdown file defines terms and examples.
- `proofs/`: Vocabularies for proof suites such as `BJJSignature2021`, `SparseMerkleTreeProof`, `Iden3SparseMerkleTreeProof`, and versions.
- `core/`:
  - `jsonld/`: Core JSON-LD contexts for auth, payment, display, resolution, and generalized proofs.
  - `json/`: Core JSON schemas that define request/permit structures (e.g., Payment Rails, ERC20 variants) and `auth.json`.
  - `vocab/`: Human-readable vocab docs referenced by core contexts (e.g., `payment.md`, `resolution.md`).
- `schemas/`:
  - `json/`: Credential JSON Schemas (e.g., `IdentityCredential-v1.json`, `auth-v1.json`, KYC series: `KYCAgeCredential-v*`, `KYCCountryOfResidenceCredential-v*`, etc.).
  - `json-ld/`: Credential JSON-LD contexts (e.g., `iden3credential.json-ld`, `iden3credential-v2.json-ld`, `kyc-*.json-ld`, `identity-*.json-ld`).

### How things fit together

1) JSON Schemas enforce structure and validation
- Credentials reference a `credentialSchema` with an `id` and `type`.
- Example (`schemas/json/KYCAgeCredential-v4.json`) requires top-level W3C VC fields plus domain-specific `credentialSubject`:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["@context","id","type","issuanceDate","credentialSubject","credentialSchema","credentialStatus","issuer"],
  "properties": {
    "issuer": {"type":["string","object"],"format":"uri"},
    "credentialSubject": {
      "type": "object",
      "required": ["id","birthday","documentType"]
    }
  }
}
```

2) JSON-LD Contexts provide semantic meaning
- Credential contexts map terms to vocabularies and proof suites. For example, `schemas/json-ld/iden3credential-v2.json-ld` defines `Iden3SparseMerkleTreeProof`, `SparseMerkleTreeProof`, and `BJJSignature2021` nested contexts and datatypes.
- Core contexts like `core/jsonld/payment.jsonld` define payment-related request/permit types and link to vocab at `core/vocab/payment.md`.

3) Vocab markdown describes fields for humans
- Files in `credentials/` and `core/vocab/` document each term, expected formats, and examples. These are linked from contexts using anchors.

### Representative components

- Credentials (schemas): `IdentityCredential-v1.json`, `auth-v1.json`, `KYCAgeCredential-v*`, `KYCCountryOfResidenceCredential-v*`, `ProfessionalCredential-v1.json`, `Organization-v1.json`.
- Credential Contexts: `iden3credential.json-ld`, `iden3credential-v2.json-ld`, `kyc-*.json-ld`, `identity-*.json-ld`.
- Proof Contexts: part of `iden3credential-v2.json-ld` and `core/jsonld/iden3proofs.jsonld`.
- Core Payment: contexts in `core/jsonld/payment.jsonld`; schemas in `core/json/*.json`; vocab in `core/vocab/payment.md`.

### Typical usage flow

- Issuers and verifiers:
  - Use JSON-LD contexts to construct semantically rich credential documents and proof objects.
  - Validate credential payloads with the matching JSON Schema.
  - Keep `@context` aligned with the schema’s `$metadata.uris.jsonLdContext` (when present) and keep `credentialSchema.id` pointing at the canonical schema.

### Example snippets

- Minimal structure from `KYCAgeCredential-v4.json` (see file for full spec):

```12:24:/workspace/schemas/json/KYCAgeCredential-v4.json
  "required": [
    "@context",
    "id",
    "type",
    "issuanceDate",
    "credentialSubject",
    "credentialSchema",
    "credentialStatus",
    "issuer"
  ],
```

- A payment request term mapping (from `core/jsonld/payment.jsonld`):

```14:22:/workspace/core/jsonld/payment.jsonld
        "nonce": {
          "@id": "nonce",
          "@type": "xsd:nonNegativeInteger"
        },
```

### Links to key files

- README: `README.md`
- Credential schemas: `schemas/json/`
- Credential contexts: `schemas/json-ld/`
- Core contexts: `core/jsonld/`
- Core schemas: `core/json/`
- Vocabularies: `credentials/`, `core/vocab/`

### Notes

- This repo evolves alongside Iden3 credential model updates; several files have versioned variants (e.g., `-v2`, `-v3`, `-v4`). Always choose the context and schema versions that match your issuer/verifier stack.

