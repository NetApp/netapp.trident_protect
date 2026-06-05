# Backup And Restore Scenario Role

## Overview

The `backup_and_restore_scenario` role is part of the **NetApp Trident Protect Validated Content Collection**.
This role performs on-demand backup and restore of OpenShift Virtualization VMs using Trident Protect.

## Role Order / Prerequisites

This role expects the AppVault, Application CR, and VM/PVC labels to already
exist on the cluster. Run the
[`trident_protect_common`](../trident_protect_common/README.md) role first to
create them.

Typical workflow:

1. `trident_protect_common` – create Secret, AppVault, Application, and label VMs/PVCs.
2. `backup_and_restore_scenario` – perform on-demand backup and restore.

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
| `validate_certs` | Whether to validate TLS certificates when connecting to the OpenShift/Kubernetes API. | `false` |
| `appvault_name` | Trident Protect AppVault used to store the backup. | Required |
| `application_name` | Trident Protect Application that is backed up. | Required |
| `vm_namespace` | Namespace containing the source VMs. | Required |
| `on_demand_backup_name` | Name of the on-demand Backup CR to create. | Required |
| `restore_namespace` | Namespace into which VMs will be restored. | Required |
| `backuprestore_name` | Name of the BackupRestore CR used for restoration. | Required |
| `vm_list` | List of VM names (used during validation/restore). | Required |
| `pvc_list` | List of PVC names associated with the VMs. | Required |

> Note: Sensitive values (API tokens, S3 credentials) should be stored in an
> Ansible Vault file rather than committed in plain text.

## Example Playbook

```yaml
---
- name: Run backup_and_restore_scenario
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    # ... add the role-specific variables listed above ...
  roles:
    - backup_and_restore_scenario
```

