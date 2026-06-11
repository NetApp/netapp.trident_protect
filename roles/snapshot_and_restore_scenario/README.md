# Snapshot And Restore Scenario Role

## Overview

The `snapshot_and_restore_scenario` role is part of the **NetApp Trident Protect Validated Content Collection**.
This role restores VMs from the latest snapshot produced by a Trident Protect snapshot schedule using a SnapshotInplaceRestore.

## Role Order / Prerequisites

This role expects the AppVault and Application CR to already exist, and a
Trident Protect `Schedule` to have produced at least one snapshot for
`application_name`. Run the
[`trident_protect_common`](../trident_protect_common/README.md) and
[`create_snapshot_schedule`](../create_snapshot_schedule/README.md) roles
first, then wait long enough for the schedule to take its first snapshot
before running this role.

Typical workflow:

1. `trident_protect_common` – create Secret, AppVault, Application, and label VMs/PVCs.
2. `create_snapshot_schedule` – create the periodic snapshot schedule.
3. *(wait for at least one snapshot to be produced)*
4. `snapshot_and_restore_scenario` – restore VMs from the latest snapshot.

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
| `oc_api_url` | OpenShift/Kubernetes API server URL of the target cluster. | Required |
| `oc_api_token` | Bearer token used to authenticate against the OpenShift API. | Required |
| `validate_certs` | Whether to validate TLS certificates when connecting to the OpenShift/Kubernetes API. Set to `false` only for clusters with self-signed certificates; disabling validation exposes credentials to MITM attacks. | `true` |
| `appvault_name` | AppVault used by the snapshot/restore. | Required |
| `application_name` | Application whose snapshots are restored. | Required |
| `vm_namespace` | Namespace where VMs are restored in place. | Required |
| `vm_list` | List of VM names that will be deleted before restore. | Required |
| `pvc_list` | List of PVC names that will be deleted before restore. | Required |
| `vm_label` | Label value used to filter VMs/PVCs during verification. | Required |
| `snapshotinplacerestore_name` | Name of the SnapshotInplaceRestore CR. | Required |

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
    - snapshot_and_restore_scenario   # run after the first snapshot exists
```

