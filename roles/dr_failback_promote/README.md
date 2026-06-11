# DR Failback Promote Role

## Overview

The `dr_failback_promote` role is part of the **NetApp Trident Protect Validated Content Collection**.
Promote the AppMirrorRelationship on the original source cluster to initiate failback of VMs.

## Role Order / Prerequisites

This role is **step 6** of the DR (AppMirrorRelationship) workflow and assumes
[`dr_reverse_resync_config`](../dr_reverse_resync_config/README.md) has
completed. See the
[collection-level workflow overview](../../README.md#disaster-recovery-appmirrorrelationship-workflow)
for the full ordered list of DR roles.

Next step: [`dr_failback_prepare_forward_amr`](../dr_failback_prepare_forward_amr/README.md).

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
| `validate_certs` | Whether to validate TLS certificates when connecting to the OpenShift/Kubernetes API. Set to `false` only for clusters with self-signed certificates; disabling validation exposes credentials to MITM attacks. | `true` |
| `appmirrorrelationship_specs` | AMR specs dict with `name`, `storage_class`, and `recurrence_rule` (`dtstart`, `rrule`). | Required |
| `src_vm_namespace` | Original source namespace. | Required |
| `dst_vm_namespace` | Original destination namespace. | Required |
| `src_appvault_name` | AppVault on the original source cluster. | Required |
| `dst_appvault_name` | AppVault on the original destination cluster. | Required |
| `src_application_name` | Source Application CR name. | Required |
| `shutdown_snapshot_name` | Name of the ShutdownSnapshot CR to create before failback. | Required |
| `src_snapshot_schedule_specs` | Dict with `name` of the Schedule CR to remove on the destination cluster. Required (no default guard). | Required |

> Note: Sensitive values (API tokens, S3 credentials) should be stored in an
> Ansible Vault file rather than committed in plain text.

## Example Playbook

```yaml
---
- name: Run dr_failback_promote
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    src_oc_api_url: "https://api.src.example.openshift.com:6443"
    src_oc_api_token: "{{ SRC_OC_API_TOKEN }}"
    # ... add the role-specific variables listed above ...
  roles:
    - dr_failback_promote
```

