# Trident Protect Common Role

## Overview

The `trident_protect_common` role is part of the **NetApp Trident Protect Validated Content Collection**.
Common Trident Protect tasks (Secret, AppVault, Application, labeling) shared by Backup/Restore and Snapshot/Restore scenarios.

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
| `appvault_secret_name` | Name of the Kubernetes Secret holding ONTAP S3 credentials. |
| `s3_access_key` | ONTAP S3 access key (stored in the Secret). |
| `s3_secret_key` | ONTAP S3 secret key (stored in the Secret). |
| `appvault_name` | Name of the Trident Protect AppVault to create. |
| `ontap_s3_bucket_name` | ONTAP S3 bucket name backing the AppVault. |
| `ontap_s3_endpoint` | ONTAP S3 endpoint (LIF IP/FQDN) backing the AppVault. |
| `vm_namespace` | Namespace where the target VMs live. |
| `vm_list` | List of VM names to label and include in the Application. |
| `pvc_list` | List of PVC names associated with the VMs. |
| `vm_label` | Label value applied to VMs and PVCs (`category=<vm_label>`). |
| `application_name` | Trident Protect Application CR name for the VMs. |

> Note: Sensitive values (API tokens, S3 credentials) should be stored in an
> Ansible Vault file rather than committed in plain text.

## Example Playbook

```yaml
---
- name: Run trident_protect_common
  hosts: localhost
  gather_facts: false
  connection: local
  vars:
    oc_api_url: "https://api.aa02-ocp.example.com:6443"
    oc_api_token: "{{ OC_API_TOKEN }}"
    # ... add the role-specific variables listed above ...
  roles:
    - netapp.trident_protect.trident_protect_common
```

