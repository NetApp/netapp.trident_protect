# Dr Failover Role

## Overview

The `dr_failover` role is part of the **NetApp Trident Protect Validated Content Collection**.
Fail over the replicated application to the destination OpenShift cluster after a disaster on the source cluster.

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
| `appmirrorrelationship_specs` | AMR specs (uses `name`). |
| `src_vm_namespace` | Source namespace (used during simulated disaster). |
| `dst_vm_namespace` | Destination namespace where VMs are failed over. |
| `src_vm_list` | List of source VMs. |
| `src_vm_label` | Label used to filter source VMs/PVCs. |
| `simulate_disaster` | Set to `true` to stop and delete source VMs to simulate a disaster. |

> Note: Sensitive values (API tokens, S3 credentials) should be stored in an
> Ansible Vault file rather than committed in plain text.

## Example Playbook

```yaml
---
- name: Run dr_failover
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    # ... add the role-specific variables listed above ...
  roles:
    - netapp.trident_protect.dr_failover
```

## License

Apache-2.0

## Author Information

NetApp - Kamini Singh
