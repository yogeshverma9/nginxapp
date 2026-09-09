# OCP to AKS Migration Report

- **Repository:** `ocp-source`
- **Mode:** `offline`
- **Generated:** 2026-09-06T12:15:54+00:00

## Environment(s) converted

| Name | Tier | AKS Cluster | Namespace |
|---|---|---|---|
| `dev` | `dev` | `devops-ocp-to-aks-001` | `dev` |

## Verdict: NEEDS_REVIEW

No blocking defects, but 1 finding(s) require human judgement and 1 configuration value(s) are still TBC. A pull request will be opened for review; it will not auto-merge.

| Severity | Count |
|---|---|
| MEDIUM | 1 |
| INFO | 3 |
| NEEDS_INPUT | 1 |

## Transforms applied

| Rule | File | Change |
|---|---|---|
| `T1` | `templates/route.yaml` | Route converted to Ingress |

## Findings requiring action

### [MED] `L3` — LLM refinement rejected, reverted to deterministic output

**Location:** `templates/ingress.yaml`

The model's response for this file failed a basic sanity/YAML check and was discarded; the deterministic transform/remediate output was kept.

**Remediation:** Optionally re-run with LLM_FORCE_REFRESH=true, or inspect the cached prompt/response under LLM_CACHE_DIR.

### [INPUT] `T1` — Route TLS termination requires a secretName

**Location:** `templates/route.yaml`

The source Route specified TLS. The emitted Ingress references .Values.route.tlsSecretName, which is not yet defined.

**Remediation:** Provision/import the TLS secret on AKS and set route.tlsSecretName in the environment's values file.

## Artefacts

| File | Contents |
|---|---|
| `inventory.json` | Discovered files with OpenShift coupling scores |
| `findings.json` | Machine-readable findings |
| `verdict.json` | Judge decision and counts |
| `diff.patch` | Unified diff, source to migrated |
| `rendered/` | Migrated artefacts ready for pull request |
