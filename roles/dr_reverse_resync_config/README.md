# Dr Reverse Resync Config Role

## Overview

The `dr_reverse_resync_config` role is part of the **NetApp Trident Protect Validated Content Collection**.
Reverse resync the failed-over AppMirrorRelationship to re-establish replication from the new primary back to the original primary cluster.

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
| `appmirrorrelationship_specs` | AMR specs (uses `name`, `storage_class`, `recurrence_rule`). |
| `src_appvault_name` | AppVault on the original source cluster. |
| `dst_appvault_name` | AppVault on the original destination cluster. |
| `src_application_name` | Source application name. |
| `src_vm_namespace` | Original source namespace. |
| `dst_vm_namespace` | Original destination namespace (acts as new source). |

> Note: Sensitive values (API tokens, S3 credentials) should be stored in an
> Ansible Vault file rather than committed in plain text.

## Example Playbook

```yaml
---
- name: Run dr_reverse_resync_config
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    # ... add the role-specific variables listed above ...
  roles:
    - netapp.trident_protect.dr_reverse_resync_config
```

## License

Apache-2.0

## Author Information

NetApp - Kamini Singh
