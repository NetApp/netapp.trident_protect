# Create Snapshot Schedule Role

## Overview

The `create_snapshot_schedule` role is part of the **NetApp Trident Protect Validated Content Collection**.
Create a Trident Protect Schedule resource for periodic snapshots of an Application (set of VMs).

## Role Order / Prerequisites

This role expects the AppVault and Application CR referenced by
`appvault_name` / `application_name` to already exist on the cluster. Run the
[`trident_protect_common`](../trident_protect_common/README.md) role first to
create them.

Typical workflow:

1. `trident_protect_common` – create Secret, AppVault, Application, and label VMs/PVCs.
2. `create_snapshot_schedule` – create the periodic snapshot Schedule CR.

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
| `appvault_name` | AppVault referenced by the Schedule CR. |
| `application_name` | Trident Protect Application targeted by the schedule. |
| `vm_namespace` | Namespace where the Schedule CR is created. |
| `snapshot_schedule_name` | Name of the Schedule CR to create. |
| `snapshot_retention_count` | Number of snapshots to retain. |
| `snapshot_granularity` | Granularity of the snapshot schedule (Hourly, Daily, Weekly, Monthly). |
| `snapshot_hour` | Hour of day for Daily/Weekly/Monthly snapshots (0-23). |
| `snapshot_minute` | Minute of hour for snapshots (0-59). |
| `snapshot_day_of_month` | Day of the month for Monthly snapshots (1-31). |
| `snapshot_day_of_week` | Day of the week for Weekly snapshots (0-6). |

> Note: Sensitive values (API tokens, S3 credentials) should be stored in an
> Ansible Vault file rather than committed in plain text.

## Example Playbook

```yaml
---
- name: Run create_snapshot_schedule
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    # ... add the role-specific variables listed above ...
  roles:
    - trident_protect_common
    - create_snapshot_schedule
```

## Related Roles

- [`trident_protect_common`](../trident_protect_common/README.md) – prerequisite
  that creates the AppVault and Application CR referenced by this role.
- [`snapshot_and_restore_scenario`](../snapshot_and_restore_scenario/README.md) –
  after the schedule has produced at least one snapshot, use this role to
  restore VMs from the most recent snapshot.


