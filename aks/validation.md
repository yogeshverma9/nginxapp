# OCP to AKS Migration Report

- **Repository:** `ocp-source`
- **Mode:** `offline`
- **Generated:** 2026-09-09T03:31:58+00:00

## Environment(s) converted

| Name | Tier | AKS Cluster | Namespace |
|---|---|---|---|
| `dev` | `dev` | `devops-ocp-to-aks-001` | `dev` |

## Verdict: BLOCK

4 blocking finding(s) must be resolved before this workload can be deployed to AKS. No pull request will be opened for deployment. The most severe items are structural defects the OpenShift platform tolerated but AKS will reject.

| Severity | Count |
|---|---|
| BLOCK | 4 |
| HIGH | 2 |
| INFO | 5 |
| NEEDS_INPUT | 1 |

## Transforms applied

| Rule | File | Change |
|---|---|---|
| `T1` | `nginx-app/templates/route.yaml` | Route converted to Ingress |
| `L1` | `nginx-app/templates/hpa.yaml` | LLM reviewed and updated this file |
| `L1` | `nginx-app/templates/ingress.yaml` | LLM reviewed and updated this file |

## Findings requiring action

### [BLOCK] `M1` — Template references undefined value .Values.deployment.namespace

**Location:** `nginx-app/templates/ingress.yaml`

No values file defines 'deployment.namespace'; it renders empty. Checked: ['dev_values.yaml']

**Remediation:** Define deployment.namespace in every values file, or correct the template key name.

### [BLOCK] `M1` — Template references undefined value .Values.route.tlsSecretName

**Location:** `nginx-app/templates/ingress.yaml`

No values file defines 'route.tlsSecretName'; it renders empty. Checked: ['dev_values.yaml']

**Remediation:** Define route.tlsSecretName in every values file, or correct the template key name.

### [BLOCK] `M1` — Template references undefined value .Values.service.label

**Location:** `nginx-app/templates/ingress.yaml`

No values file defines 'service.label'; it renders empty. Checked: ['dev_values.yaml']

**Remediation:** Define service.label in every values file, or correct the template key name.

### [BLOCK] `M1` — Template references undefined value .Values.service.portValue

**Location:** `nginx-app/templates/ingress.yaml`

No values file defines 'service.portValue'; it renders empty. Checked: ['dev_values.yaml']

**Remediation:** Define service.portValue in every values file, or correct the template key name.

### [HIGH] `M7` — Container 'container' has no readinessProbe

**Location:** `nginx-app/templates/hpa.yaml`

helm upgrade --atomic judges rollout success by readiness. Without a probe, --atomic reports success on a broken deploy.

**Remediation:** Add readinessProbe and livenessProbe on the container port.

### [HIGH] `M7` — Container 'container' has no livenessProbe

**Location:** `nginx-app/templates/hpa.yaml`

No livenessProbe defined.

**Remediation:** Add a livenessProbe.

### [INPUT] `T1` — Route TLS termination requires a secretName

**Location:** `nginx-app/templates/route.yaml`

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
