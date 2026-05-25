# Dr Amr Config Role

## Overview

The `dr_amr_config` role is part of the **NetApp Trident Protect Validated Content Collection**.
Create the AppMirrorRelationship (AMR) custom resource to establish replication of VMs between source and destination OpenShift clusters.

## Role Order / Prerequisites

This role is **step 2** of the DR (AppMirrorRelationship) workflow and assumes
[`dr_amr_prerequisites`](../dr_amr_prerequisites/README.md) has been run on
both clusters. See the
[collection-level workflow overview](../../README.md#disaster-recovery-appmirrorrelationship-workflow)
for the full ordered list of DR roles.

Next step (only if a failover is exercised): [`dr_failover`](../dr_failover/README.md).

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
| `src_appvault_name` | AppVault on the source cluster. |
| `dst_appvault_name` | AppVault on the destination cluster. |
| `src_application_name` | Source Application referenced by the AMR. |
| `src_vm_namespace` | Source namespace. |
| `dst_vm_namespace` | Destination namespace where replicated VMs are materialized. |
| `appmirrorrelationship_specs` | Dict with `name`, `storage_class`, and `recurrence_rule` (`dtstart`, `rrule`) for the AMR CR. |

> Note: Sensitive values (API tokens, S3 credentials) should be stored in an
> Ansible Vault file rather than committed in plain text.

## Example Playbook

```yaml
---
- name: Run dr_amr_config
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    # ... add the role-specific variables listed above ...
  roles:
    - dr_amr_config
```

