# Dr Failback Promote Role

## Overview

The `dr_failback_promote` role is part of the **NetApp Trident Protect Validated Content Collection**.
Promote the AppMirrorRelationship on the original source cluster to initiate failback of VMs.

## Role Order / Prerequisites

This role is **step 6** of the DR (AppMirrorRelationship) workflow and assumes
[`dr_reverse_resync_config`](../dr_reverse_resync_config/README.md) has
completed. See the
[collection-level workflow overview](../../README.md#disaster-recovery-appmirrorrelationship-workflow)
for the full ordered list of DR roles.

Next step: [`dr_failback_prepare_forward_amr`](../dr_failback_prepare_forward_amr/README.md).

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

| Variable | Description |
|----------|-------------|
| `src_oc_api_url` | Source OpenShift cluster API server URL (DR scenarios). |
| `src_oc_api_token` | Source OpenShift cluster bearer token (DR scenarios). |
| `dst_oc_api_url` | Destination OpenShift cluster API server URL (DR scenarios). |
| `dst_oc_api_token` | Destination OpenShift cluster bearer token (DR scenarios). |
| `appmirrorrelationship_specs` | AMR specs (uses `name`). |
| `src_vm_namespace` | Original source namespace. |
| `dst_vm_namespace` | Original destination namespace. |

> Note: Sensitive values (API tokens, S3 credentials) should be stored in an
> Ansible Vault file rather than committed in plain text.

## Example Playbook

```yaml
---
- name: Run dr_failback_promote
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    # ... add the role-specific variables listed above ...
  roles:
    - dr_failback_promote
```

