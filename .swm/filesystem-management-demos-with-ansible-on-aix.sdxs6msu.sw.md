---
title: Filesystem Management Demos with Ansible on AIX
---
# Filesystem Demos Overview

Filesystem Demos in Playbooks showcase various operations that can be performed on filesystems using Ansible modules for AIX. These demos include tasks such as creating, mounting, and unmounting filesystems, as well as modifying filesystem attributes. These playbooks serve as practical examples for users to understand and implement filesystem management tasks on AIX systems using Ansible.

# Usage in <SwmPath>[playbooks/demo_mount.yml](playbooks/demo_mount.yml)</SwmPath>

The <SwmPath>[playbooks/demo_mount.yml](playbooks/demo_mount.yml)</SwmPath> playbook demonstrates how to list, mount, and unmount filesystems on AIX systems. It includes tasks for listing mounted filesystems, mounting filesystems, and unmounting filesystems.

# Usage in <SwmPath>[playbooks/demo_filesystem.yml](playbooks/demo_filesystem.yml)</SwmPath>

The <SwmPath>[playbooks/demo_filesystem.yml](playbooks/demo_filesystem.yml)</SwmPath> playbook illustrates how to create a <SwmToken path="playbooks/demo_filesystem.yml" pos="14:12:12" line-data="    - name: Creation of a JFS2 filesystem">`JFS2`</SwmToken> filesystem, increase its size, and remove an NFS filesystem. It provides practical examples for managing filesystems on AIX systems using Ansible.

<SwmSnippet path="/playbooks/demo_filesystem.yml" line="1">

---

This playbook demonstrates how to create a <SwmToken path="playbooks/demo_filesystem.yml" pos="14:12:12" line-data="    - name: Creation of a JFS2 filesystem">`JFS2`</SwmToken> filesystem, increase its size, and remove an NFS filesystem using the <SwmToken path="playbooks/demo_filesystem.yml" pos="15:1:5" line-data="      ibm.power_aix.filesystem:">`ibm.power_aix.filesystem`</SwmToken> module.

```yaml
---
- name: FILESYSTEM on AIX
  hosts: "{{host_name}}"
  gather_facts: false
  vars:
    host_name: all
    filesystem_name: /mnt4
    fs_type_v: jfs2
    mount_group_v: test
    permissions_v: rw
    vg_v: rw

  tasks:
    - name: Creation of a JFS2 filesystem
      ibm.power_aix.filesystem:
        state: present
        filesystem: "{{ filesystem_name }}"
        fs_type: "{{ fs_type_v }}"
        attributes: size=32768,isnapshot='no'
        mount_group: "{{ mount_group_v }}"
        permissions: "{{ permissions_v }}"
```

---

</SwmSnippet>

## List Mounted Filesystems

The <SwmToken path="playbooks/demo_mount.yml" pos="12:6:10" line-data="    - name: List mounted filesystems">`List mounted filesystems`</SwmToken> function is used to display all currently mounted filesystems on the AIX system. This is useful for verifying the current state of the system's mounts.

<SwmSnippet path="/playbooks/demo_mount.yml" line="12">

---

This task lists all currently mounted filesystems on the AIX system using the <SwmToken path="playbooks/demo_mount.yml" pos="13:1:5" line-data="      ibm.power_aix.mount:">`ibm.power_aix.mount`</SwmToken> module.

```yaml
    - name: List mounted filesystems
      ibm.power_aix.mount:
        state: show
```

---

</SwmSnippet>

## Mount Filesystems

The <SwmToken path="playbooks/demo_mount.yml" pos="16:6:8" line-data="    - name: Mount filesystems">`Mount filesystems`</SwmToken> function mounts a specified filesystem on the AIX system. This is essential for making a filesystem available for use.

<SwmSnippet path="/playbooks/demo_mount.yml" line="16">

---

This task mounts a specified filesystem on the AIX system using the <SwmToken path="playbooks/demo_mount.yml" pos="17:1:5" line-data="      ibm.power_aix.mount:">`ibm.power_aix.mount`</SwmToken> module.

```yaml
    - name: Mount filesystems
      ibm.power_aix.mount:
        state: mount
        mount_dir: "{{ mount_dir_value }}"
```

---

</SwmSnippet>

## Creation of a <SwmToken path="playbooks/demo_filesystem.yml" pos="14:12:12" line-data="    - name: Creation of a JFS2 filesystem">`JFS2`</SwmToken> Filesystem

The <SwmToken path="playbooks/demo_filesystem.yml" pos="14:6:14" line-data="    - name: Creation of a JFS2 filesystem">`Creation of a JFS2 filesystem`</SwmToken> function creates a new <SwmToken path="playbooks/demo_filesystem.yml" pos="14:12:12" line-data="    - name: Creation of a JFS2 filesystem">`JFS2`</SwmToken> filesystem on the AIX system. This is important for setting up new storage areas.

<SwmSnippet path="/playbooks/demo_filesystem.yml" line="14">

---

This task creates a new <SwmToken path="playbooks/demo_filesystem.yml" pos="14:12:12" line-data="    - name: Creation of a JFS2 filesystem">`JFS2`</SwmToken> filesystem on the AIX system using the <SwmToken path="playbooks/demo_filesystem.yml" pos="15:1:5" line-data="      ibm.power_aix.filesystem:">`ibm.power_aix.filesystem`</SwmToken> module.

```yaml
    - name: Creation of a JFS2 filesystem
      ibm.power_aix.filesystem:
        state: present
        filesystem: "{{ filesystem_name }}"
        fs_type: "{{ fs_type_v }}"
        attributes: size=32768,isnapshot='no'
        mount_group: "{{ mount_group_v }}"
        permissions: "{{ permissions_v }}"
```

---

</SwmSnippet>

## Increase Size of a Filesystem

The <SwmToken path="playbooks/demo_filesystem.yml" pos="23:6:14" line-data="    - name: Increase size of a filesystem">`Increase size of a filesystem`</SwmToken> function increases the size of an existing filesystem. This is useful for expanding storage capacity as needed.

<SwmSnippet path="/playbooks/demo_filesystem.yml" line="23">

---

This task increases the size of an existing filesystem on the AIX system using the <SwmToken path="playbooks/demo_filesystem.yml" pos="24:1:5" line-data="      ibm.power_aix.filesystem:">`ibm.power_aix.filesystem`</SwmToken> module.

```yaml
    - name: Increase size of a filesystem
      ibm.power_aix.filesystem:
        filesystem: "{{ filesystem_name }}"
        state: present
        attributes: size=+5M
```

---

</SwmSnippet>

## Remove a NFS Filesystem

The <SwmToken path="playbooks/demo_filesystem.yml" pos="28:6:12" line-data="    - name: Remove a NFS filesystem">`Remove a NFS filesystem`</SwmToken> function removes an existing NFS filesystem from the AIX system. This is necessary for cleaning up or decommissioning storage.

<SwmSnippet path="/playbooks/demo_filesystem.yml" line="28">

---

This task removes an existing NFS filesystem from the AIX system using the <SwmToken path="playbooks/demo_filesystem.yml" pos="29:1:5" line-data="      ibm.power_aix.filesystem:">`ibm.power_aix.filesystem`</SwmToken> module.

```yaml
    - name: Remove a NFS filesystem
      ibm.power_aix.filesystem:
        filesystem: "{{ filesystem_name }}"
        state: absent
        rm_mount_point: true
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
