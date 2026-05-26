# DR AMR Prerequisites Role

## Overview

The `dr_amr_prerequisites` role is part of the **NetApp Trident Protect Validated Content Collection**.
Set up prerequisites (secrets, AppVaults, Application, snapshots) on source and destination clusters for AppMirrorRelationship-based disaster recovery.

## Role Order / Prerequisites

This role is **step 1** of the DR (AppMirrorRelationship) workflow. See the
[collection-level workflow overview](../../README.md#disaster-recovery-appmirrorrelationship-workflow)
for the full ordered list of DR roles.

Next step: [`dr_amr_config`](../dr_amr_config/README.md).

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
| `src_ontap_s3_specs` | Dict with `secret_name`, `access_key`, `secret_key`, `s3_bucket_name`, `s3_endpoint` for the source ONTAP S3 backend. |
| `dst_ontap_s3_specs` | Dict with `secret_name`, `access_key`, `secret_key`, `s3_bucket_name`, `s3_endpoint` for the destination ONTAP S3 backend. |
| `src_appvault_name` | AppVault name for the source application. |
| `dst_appvault_name` | AppVault name for the destination application. |
| `src_vm_namespace` | Namespace on the source cluster containing VMs to replicate. |
| `dst_vm_namespace` | Namespace on the destination cluster where replicated VMs land. |
| `src_vm_list` | List of source VM names to label/include. |
| `src_pvc_list` | List of PVC names associated with the source VMs. |
| `src_vm_label` | Label value applied to source VMs and PVCs. |
| `src_application_name` | Application CR name for the source VMs. |
| `src_on_demand_snapshot` | Set to `true` to take an on-demand snapshot before AMR. |
| `src_on_demand_snapshot_specs` | Dict with `name` and `reclaim_policy` for the on-demand snapshot. |
| `src_scheduled_snapshot` | Set to `true` to create a Schedule on the source cluster. |
| `src_snapshot_schedule_specs` | Dict with `name`, `snapshot_reclaim_policy`, `retention_count`, `recurrence_rule` (`dtstart`, `rrule`). |

> Note: Sensitive values (API tokens, S3 credentials) should be stored in an
> Ansible Vault file rather than committed in plain text.

## Example Playbook

```yaml
---
- name: Run dr_amr_prerequisites
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    # ... add the role-specific variables listed above ...
  roles:
    - dr_amr_prerequisites
```

