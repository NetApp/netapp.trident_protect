[![CI](https://github.com/NetApp/netapp.trident_protect/workflows/CI/badge.svg?event=push)](https://github.com/NetApp/netapp.trident_protect/actions)

# Ansible Validated Content Collection for NetApp Trident Protect

This collection includes a variety of validated Ansible Roles to automate and manage Day-2 use cases involving NetApp Trident Protect in a Red Hat OpenShift Virtualization environment. It covers common application data management workflows such as Backup and Restore, Snapshot scheduling and Restore, and Disaster Recovery (SnapMirror Replication, Failover, Failback, and Reverse Resync).

## Dependencies

This collection uses the [kubernetes.core](https://galaxy.ansible.com/ui/repo/published/kubernetes/core)
certified Ansible collection to manage Kubernetes objects such as Applications,
AppVaults, Backups, Snapshots, SnapshotSchedules, and Replication resources via
the NetApp Trident Protect custom resources.

## Tested with Ansible

This collection has been tested with ansible-core >= 2.16.0.

## Using this collection

### Installing the Collection from Ansible Galaxy

Before using this collection, you need to install it with the Ansible Galaxy
command-line tool:

```bash
ansible-galaxy collection install netapp.trident_protect
```

You can also include it in a `requirements.yml` file and install it with
`ansible-galaxy collection install -r requirements.yml`, using the format:

```yaml
---
collections:
  - name: netapp.trident_protect
```

Note that if you install the collection from Ansible Galaxy, it will not be upgraded automatically when you upgrade the `ansible` package. To upgrade the collection to the latest available version, run the following command:

```bash
ansible-galaxy collection install netapp.trident_protect --upgrade
```

You can also install a specific version of the collection, for example, if you need to downgrade when something is broken in the latest version (please report an issue in this repository). Use the following syntax to install a specific version, for example `1.0.0`:

```bash
ansible-galaxy collection install netapp.trident_protect:==1.0.0
```

See [using Ansible collections](https://docs.ansible.com/ansible/devel/user_guide/collections_using.html) for more details.

### Running the playbook

Update `vars/trident_protect_main.yml` with your desired configuration before running the playbook.

To execute common Trident Protect tasks (shared by both Backup/Restore and Snapshot/Restore scenarios):

```bash
ansible-playbook Setup_Trident_Protect_tasks.yml -e @vars/trident_protect_main.yml -t tp_common_tasks
```

To execute the Backup and Restore scenario:

```bash
ansible-playbook Setup_Trident_Protect_tasks.yml -e @vars/trident_protect_main.yml -t tp_backup_and_restore_tasks
```

To create Snapshot schedules for VMs in OpenShift Virtualization:

```bash
ansible-playbook Setup_Trident_Protect_tasks.yml -e @vars/trident_protect_main.yml -t tp_snapshot_schedule_tasks
```

To execute the Snapshot and Restore scenario (ensure the snapshot schedule has produced at least one snapshot first):

```bash
ansible-playbook Setup_Trident_Protect_tasks.yml -e @vars/trident_protect_main.yml -t tp_snapshot_and_restore_tasks
```

Additional Disaster Recovery tags available in `Setup_Trident_Protect_tasks.yml` include roles for DR prerequisites, AMR configuration, failover, failback (prepare/establish/promote), and reverse resync. Refer to the roles under `roles/trident_protect/` for the full list.

## Release notes

See the [changelog](https://github.com/NetApp/netapp.trident_protect/tree/main/changelogs/changelog.rst).

## More information

* [NetApp Trident Protect documentation](https://docs.netapp.com/us-en/trident/trident-protect/)
* [kubernetes.core Ansible collection](https://galaxy.ansible.com/ui/repo/published/kubernetes/core)
* [Ansible user guide](https://docs.ansible.com/ansible/devel/user_guide/index.html)
* [Ansible developer guide](https://docs.ansible.com/ansible/devel/dev_guide/index.html)
* [Ansible collections requirements](https://docs.ansible.com/ansible/devel/community/collection_contributors/collection_requirements.html)
* [Ansible community Code of Conduct](https://docs.ansible.com/ansible/devel/community/code_of_conduct.html)
* [The Bullhorn (the Ansible contributor newsletter)](https://docs.ansible.com/ansible/devel/community/communication.html#the-bullhorn)
* [Important announcements for maintainers](https://github.com/ansible-collections/news-for-maintainers)
