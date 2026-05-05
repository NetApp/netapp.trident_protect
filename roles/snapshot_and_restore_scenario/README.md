# Snapshot And Restore Scenario Role

## Overview

The `snapshot_and_restore_scenario` role is part of the **NetApp Trident Protect Validated Content Collection**.
Restore VMs from the latest snapshot produced by a Trident Protect snapshot schedule using a SnapshotInplaceRestore.

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
| `oc_api_url` | OpenShift/Kubernetes API server URL of the target cluster. |
| `oc_api_token` | Bearer token used to authenticate against the OpenShift API. |
| `appvault_name` | AppVault used by the snapshot/restore. |
| `application_name` | Application whose snapshots are restored. |
| `vm_namespace` | Namespace where VMs are restored in place. |
| `vm_list` | List of VM names that will be deleted before restore. |
| `pvc_list` | List of PVC names that will be deleted before restore. |
| `vm_label` | Label value used to filter VMs/PVCs during verification. |
| `snapshotinplacerestore_name` | Name of the SnapshotInplaceRestore CR. |

> Note: Sensitive values (API tokens, S3 credentials) should be stored in an
> Ansible Vault file rather than committed in plain text.

## Example Playbook

```yaml
---
- name: Run snapshot_and_restore_scenario
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    # ... add the role-specific variables listed above ...
  roles:
    - netapp.trident_protect.snapshot_and_restore_scenario
```

## License

Apache-2.0

## Author Information

NetApp - Kamini Singh
