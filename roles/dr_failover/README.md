# DR Failover Role

## Overview

The `dr_failover` role is part of the **NetApp Trident Protect Validated Content Collection**.
Fail over the replicated applications/VMs to the destination OpenShift cluster after a disaster on the source cluster.

## Role Order / Prerequisites

This role is **step 3** of the DR (AppMirrorRelationship) workflow and assumes
[`dr_amr_prerequisites`](../dr_amr_prerequisites/README.md) and
[`dr_amr_config`](../dr_amr_config/README.md) have already been run and
replication is healthy. See the
[collection-level workflow overview](../../README.md#disaster-recovery-appmirrorrelationship-workflow)
for the full ordered list of DR roles.

Next step: [`dr_reverse_resync_prerequisites`](../dr_reverse_resync_prerequisites/README.md).

## Requirements

* Ansible v2.16.0 or newer.
* Python 3.12 or newer.
* The [kubernetes.core](https://docs.ansible.com/ansible/latest/collections/kubernetes/core/index.html) Ansible collection is installed.
* OpenShift/Kubernetes cluster(s) reachable via API URL and a valid API token.
* NetApp Trident and NetApp Trident Protect installed and configured in the cluster(s).

## Role Variables

The role does not ship opinionated defaults. The caller must provide the
following variables (typically via `-e @your_vars.yml` or under `vars:` in the
playbook):

| Variable | Description | Default |
|----------|-------------|---------|
| `src_oc_api_url` | Source OpenShift cluster API server URL (DR scenarios). | Required |
| `src_oc_api_token` | Source OpenShift cluster bearer token (DR scenarios). | Required |
| `dst_oc_api_url` | Destination OpenShift cluster API server URL (DR scenarios). | Required |
| `dst_oc_api_token` | Destination OpenShift cluster bearer token (DR scenarios). | Required |
| `validate_certs` | Whether to validate TLS certificates when connecting to the OpenShift/Kubernetes API. | `false` |
| `appmirrorrelationship_specs` | AMR specs (uses `name`). | Required |
| `src_vm_namespace` | Source namespace (used during simulated disaster). | Required |
| `dst_vm_namespace` | Destination namespace where VMs are failed over. | Required |
| `src_vm_list` | List of source VMs. | Required |
| `src_vm_label` | Label used to filter source VMs/PVCs. | Required |
| `simulate_disaster` | Set to `true` to stop and delete source VMs to simulate a disaster. | `false` |

> Note: Sensitive values (API tokens, S3 credentials) should be stored in an
> Ansible Vault file rather than committed in plain text.

## Example Playbook

```yaml
---
- name: Run dr_failover
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    src_oc_api_url: "https://api.src.example.openshift.com:6443"
    src_oc_api_token: "{{ SRC_OC_API_TOKEN }}"
    # ... add the role-specific variables listed above ...
  roles:
    - dr_failover
```

