# DR Reverse Resync Prerequisites Role

## Overview

The `dr_reverse_resync_prerequisites` role is part of the **NetApp Trident Protect Validated Content Collection**.
Validate and set up prerequisites on the new source (original destination) cluster to reverse resync the failed-over relationship.

## Role Order / Prerequisites

This role is **step 4** of the DR (AppMirrorRelationship) workflow and assumes
[`dr_failover`](../dr_failover/README.md) has completed. See the
[collection-level workflow overview](../../README.md#disaster-recovery-appmirrorrelationship-workflow)
for the full ordered list of DR roles.

Next step: [`dr_reverse_resync_config`](../dr_reverse_resync_config/README.md).

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
| `src_ontap_s3_specs` | Source ONTAP S3 specs dict (re-used to recreate Secret/AppVault on the new source cluster). | Required |
| `dst_ontap_s3_specs` | Destination ONTAP S3 specs dict. | Required |
| `src_appvault_name` | AppVault name for the source application. | Required |
| `dst_appvault_name` | AppVault name for the destination application. | Required |
| `src_application_name` | Source application name. | Required |
| `src_vm_namespace` | Original source namespace. | Required |
| `dst_vm_namespace` | Destination namespace (new source after failover). | Required |
| `src_on_demand_snapshot` | Set to `true` for an on-demand snapshot of the new source. | `false` |
| `src_on_demand_snapshot_specs` | On-demand snapshot specs dict (`name`, `reclaim_policy`). Required when `src_on_demand_snapshot` is `true`. | — |
| `src_scheduled_snapshot` | Set to `true` to create a snapshot Schedule on the new source. | `false` |
| `src_snapshot_schedule_specs` | Snapshot schedule specs dict (`name`, `snapshot_reclaim_policy`, `retention_count`, `recurrence_rule`). Required when `src_scheduled_snapshot` is `true`. | — |
| `shutdown_snapshot_name` | Name of the shutdown snapshot to create on the new source before failback. | Required |

> Note: Sensitive values (API tokens, S3 credentials) should be stored in an
> Ansible Vault file rather than committed in plain text.

## Example Playbook

```yaml
---
- name: Run dr_reverse_resync_prerequisites
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    src_oc_api_url: "https://api.src.example.openshift.com:6443"
    src_oc_api_token: "{{ SRC_OC_API_TOKEN }}"
    # ... add the role-specific variables listed above ...
  roles:
    - dr_reverse_resync_prerequisites
```

