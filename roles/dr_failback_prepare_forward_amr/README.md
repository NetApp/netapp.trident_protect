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

| Variable | Description | Default |
|----------|-------------|---------|
| `src_oc_api_url` | Source OpenShift cluster API server URL (DR scenarios). | Required |
| `src_oc_api_token` | Source OpenShift cluster bearer token (DR scenarios). | Required |
| `dst_oc_api_url` | Destination OpenShift cluster API server URL (DR scenarios). | Required |
| `dst_oc_api_token` | Destination OpenShift cluster bearer token (DR scenarios). | Required |
| `validate_certs` | Whether to validate TLS certificates when connecting to the OpenShift/Kubernetes API. | `false` |
| `appmirrorrelationship_specs` | AMR specs dict (uses `name`) - the AMR on the new destination cluster is deleted before re-creating the forward AMR. | Required |
| `src_vm_namespace` | Original source namespace. | Required |
| `dst_vm_namespace` | Original destination namespace. | Required |
| `src_appvault_name` | AppVault on the original source cluster. Required when `src_scheduled_snapshot` or `src_on_demand_snapshot` is `true`. | — |
| `src_application_name` | Source application name. Required when `src_scheduled_snapshot` or `src_on_demand_snapshot` is `true`. | — |
| `src_snapshot_schedule_specs` | Dict with `name` of the Schedule CR to delete on the new destination cluster (used unconditionally). When `src_scheduled_snapshot` is `true`, also requires `snapshot_reclaim_policy`, `retention_count`, `recurrence_rule` (`dtstart`, `rrule`). | Required |
| `src_scheduled_snapshot` | Set to `true` to create a snapshot Schedule on the source cluster before forward AMR. | `false` |
| `src_on_demand_snapshot` | Set to `true` to take an on-demand snapshot before forward AMR. | `false` |
| `src_on_demand_snapshot_specs` | Dict with `name` and `reclaim_policy` for the on-demand snapshot. Required when `src_on_demand_snapshot` is `true`. | — |

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
    src_oc_api_url: "https://api.src.example.openshift.com:6443"
    src_oc_api_token: "{{ SRC_OC_API_TOKEN }}"
    # ... add the role-specific variables listed above ...
  roles:
    - dr_failback_prepare_forward_amr
```

