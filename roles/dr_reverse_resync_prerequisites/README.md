# Dr Reverse Resync Prerequisites Role

## Overview

The `dr_reverse_resync_prerequisites` role is part of the **NetApp Trident Protect Validated Content Collection**.
Validate and set up prerequisites on the new source (original destination) cluster to reverse resync the failed-over relationship.

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
| `src_ontap_s3_specs` | Source ONTAP S3 specs dict (re-used to recreate Secret/AppVault on the new source cluster). |
| `dst_ontap_s3_specs` | Destination ONTAP S3 specs dict. |
| `src_appvault_name` | AppVault name for the source application. |
| `dst_appvault_name` | AppVault name for the destination application. |
| `src_application_name` | Source application name. |
| `src_vm_namespace` | Original source namespace. |
| `dst_vm_namespace` | Destination namespace (new source after failover). |
| `src_on_demand_snapshot` | Set to `true` for an on-demand snapshot of the new source. |
| `src_on_demand_snapshot_specs` | On-demand snapshot specs dict. |
| `src_scheduled_snapshot` | Set to `true` to create a snapshot Schedule on the new source. |
| `src_snapshot_schedule_specs` | Snapshot schedule specs dict. |
| `shutdown_snapshot_name` | Name of the shutdown snapshot to create on the new source before failback. |

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
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    # ... add the role-specific variables listed above ...
  roles:
    - netapp.trident_protect.dr_reverse_resync_prerequisites
```

## License

Apache-2.0

## Author Information

NetApp - Kamini Singh
