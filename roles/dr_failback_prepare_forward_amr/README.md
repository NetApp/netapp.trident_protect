# DR Failback Prepare Forward AMR Role

## Overview

The `dr_failback_prepare_forward_amr` role is part of the **NetApp Trident Protect Validated Content Collection**.
Prepare the system to re-establish forward replication from the original primary to the destination cluster as part of the failback workflow.

## Role Order / Prerequisites

This role is **step 7** of the DR (AppMirrorRelationship) workflow and assumes
[`dr_failback_promote`](../dr_failback_promote/README.md) has completed. See
the
[collection-level workflow overview](../../README.md#disaster-recovery-appmirrorrelationship-workflow)
for the full ordered list of DR roles.

Next step: [`dr_failback_establish_forward_amr`](../dr_failback_establish_forward_amr/README.md).

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
| `src_appvault_name` | AppVault on the original source cluster. |
| `dst_appvault_name` | AppVault on the original destination cluster. |
| `src_vm_namespace` | Original source namespace. |
| `dst_vm_namespace` | Original destination namespace. |
| `src_application_name` | Source application name. |

> Note: Sensitive values (API tokens, S3 credentials) should be stored in an
> Ansible Vault file rather than committed in plain text.

## Example Playbook

```yaml
---
- name: Run dr_failback_prepare_forward_amr
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    # ... add the role-specific variables listed above ...
  roles:
    - dr_failback_prepare_forward_amr
```

