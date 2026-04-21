================================================================
 Release Notes: Ansible Validated Content for NetApp Trident Protect
================================================================

.. contents:: Topics

v1.0.0
======

Release Summary
---------------

This is the first major release of the ``netapp.trident_protect`` collection.
It provides Ansible Day2 use-cases involving Trident Protect in a Red Hat
OpenShift Virtualization environment.

New Roles/Tasks
---------------

- netapp.trident_protect.trident_protect_common - Common tasks shared across the Trident Protect roles.
- netapp.trident_protect.backup_and_restore_scenario - Perform backup and restore of VMs using Trident Protect.
- netapp.trident_protect.create_snapshot_schedule - Create snapshot schedules for VMs managed by Trident Protect.
- netapp.trident_protect.snapshot_and_restore_scenario - Perform snapshot and restore of VMs using Trident Protect.
- netapp.trident_protect.dr_amr_prerequisites - Validate and set up prerequisites for AMR-based disaster recovery.
- netapp.trident_protect.dr_amr_config - Configure AppMirrorRelationship (AMR) for disaster recovery.
- netapp.trident_protect.dr_failover - Perform disaster recovery failover to the secondary site.
- netapp.trident_protect.dr_reverse_resync_prerequisites - Validate and set up prerequisites for reverse resync.
- netapp.trident_protect.dr_reverse_resync_config - Configure reverse resync for disaster recovery.
- netapp.trident_protect.dr_failback_promote - Promote the source site back to primary during failback.
- netapp.trident_protect.dr_failback_prepare_forward_amr - Prepare the source site for forward AMR replication during failback.
- netapp.trident_protect.dr_failback_establish_forward_amr - Re-establish forward AMR replication as part of failback workflow.