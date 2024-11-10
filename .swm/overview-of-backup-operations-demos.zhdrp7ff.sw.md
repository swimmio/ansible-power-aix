---
title: Overview of Backup Operations Demos
---
# Overview

Backup Demos in the playbooks directory provide examples of how to perform backup operations on AIX systems using Ansible. These demos include tasks for creating volume groups, logical volumes, and filesystems, as well as mounting and unmounting filesystems. They also demonstrate how to create, view, and move backup images.

# Backup Operations on rootvg

The <SwmPath>[playbooks/demo_backup_rootvg.yml](playbooks/demo_backup_rootvg.yml)</SwmPath> playbook demonstrates how to back up the root volume group (rootvg) using the <SwmToken path="playbooks/demo_backup_rootvg.yml" pos="2:16:16" line-data="- name: &quot;Backup operations on rootvg using mksysb and alt_disk_mksysb for AIX&quot;">`mksysb`</SwmToken> and <SwmToken path="playbooks/demo_backup_rootvg.yml" pos="2:20:20" line-data="- name: &quot;Backup operations on rootvg using mksysb and alt_disk_mksysb for AIX&quot;">`alt_disk_mksysb`</SwmToken> commands. This playbook includes tasks for creating a <SwmToken path="playbooks/demo_backup_rootvg.yml" pos="2:16:16" line-data="- name: &quot;Backup operations on rootvg using mksysb and alt_disk_mksysb for AIX&quot;">`mksysb`</SwmToken> image, viewing the image, and moving it to a different location.

<SwmSnippet path="/playbooks/demo_backup_rootvg.yml" line="2">

---

The playbook <SwmPath>[playbooks/demo_backup_rootvg.yml](playbooks/demo_backup_rootvg.yml)</SwmPath> starts by defining the hosts and remote user for the backup operations.

```yaml
- name: "Backup operations on rootvg using mksysb and alt_disk_mksysb for AIX"
  hosts: ansible-test1
  remote_user: root
```

---

</SwmSnippet>

# Backup Operations on datavg

The <SwmPath>[playbooks/demo_backup_datavg.yml](playbooks/demo_backup_datavg.yml)</SwmPath> playbook demonstrates backup operations on a data volume group (datavg) using the <SwmToken path="playbooks/demo_backup_datavg.yml" pos="2:16:16" line-data="- name: &quot;Backup operations on datavg using savevg and restvg for AIX&quot;">`savevg`</SwmToken> and <SwmToken path="playbooks/demo_backup_datavg.yml" pos="2:20:20" line-data="- name: &quot;Backup operations on datavg using savevg and restvg for AIX&quot;">`restvg`</SwmToken> commands. This playbook includes tasks for creating and removing volume groups, logical volumes, and filesystems, as well as mounting and unmounting filesystems. It also shows how to create a <SwmToken path="playbooks/demo_backup_datavg.yml" pos="2:16:16" line-data="- name: &quot;Backup operations on datavg using savevg and restvg for AIX&quot;">`savevg`</SwmToken> backup image, view the image, and restore it using the <SwmToken path="playbooks/demo_backup_datavg.yml" pos="2:20:20" line-data="- name: &quot;Backup operations on datavg using savevg and restvg for AIX&quot;">`restvg`</SwmToken> command.

# Main Functions

There are several main functions in the backup demos, including creating volume groups, creating logical volumes, creating filesystems, mounting filesystems, creating backup images, viewing backup images, moving backup images, unmounting filesystems, deleting volume groups, and restoring backup images. Below, we will dive into creating, viewing, and restoring backup images.

## Create Backup Images

The <SwmToken path="playbooks/demo_backup_rootvg.yml" pos="65:4:4" line-data="        action: create">`create`</SwmToken>` `<SwmToken path="playbooks/demo_backup_rootvg.yml" pos="64:5:5" line-data="      ibm.power_aix.backup:">`backup`</SwmToken>` images` function is used to create a <SwmToken path="playbooks/demo_backup_rootvg.yml" pos="2:16:16" line-data="- name: &quot;Backup operations on rootvg using mksysb and alt_disk_mksysb for AIX&quot;">`mksysb`</SwmToken> image of the root volume group (rootvg). This task uses the <SwmToken path="playbooks/demo_backup_rootvg.yml" pos="64:1:5" line-data="      ibm.power_aix.backup:">`ibm.power_aix.backup`</SwmToken> module with the <SwmToken path="playbooks/demo_backup_rootvg.yml" pos="65:4:4" line-data="        action: create">`create`</SwmToken> action to generate the backup image at the specified location.

<SwmSnippet path="/playbooks/demo_backup_rootvg.yml" line="63">

---

The task for creating a <SwmToken path="playbooks/demo_backup_rootvg.yml" pos="63:10:10" line-data="    - name: Create a mksysb image of rootvg">`mksysb`</SwmToken> image of rootvg is defined in the <SwmPath>[playbooks/demo_backup_rootvg.yml](playbooks/demo_backup_rootvg.yml)</SwmPath> playbook.

```yaml
    - name: Create a mksysb image of rootvg
      ibm.power_aix.backup:
        action: create
        type: "{{ type }}"
        location: "{{ location_create }}"
        exclude_files: "{{ exclude_files }}"
        extend_fs: "{{ extend_fs }}"
        exclude_packing_files: "{{ exclude_packing_files }}"
        flags: "{{ mksysb_flags }}"
        force: "{{ force }}"
      register: result
```

---

</SwmSnippet>

## View Backup Images

The <SwmToken path="playbooks/demo_backup_rootvg.yml" pos="80:4:4" line-data="        action: view">`view`</SwmToken>` `<SwmToken path="playbooks/demo_backup_rootvg.yml" pos="64:5:5" line-data="      ibm.power_aix.backup:">`backup`</SwmToken>` images` function is used to view the details of the created <SwmToken path="playbooks/demo_backup_rootvg.yml" pos="2:16:16" line-data="- name: &quot;Backup operations on rootvg using mksysb and alt_disk_mksysb for AIX&quot;">`mksysb`</SwmToken> image. This task uses the <SwmToken path="playbooks/demo_backup_rootvg.yml" pos="64:1:5" line-data="      ibm.power_aix.backup:">`ibm.power_aix.backup`</SwmToken> module with the <SwmToken path="playbooks/demo_backup_rootvg.yml" pos="80:4:4" line-data="        action: view">`view`</SwmToken> action to display the backup image information.

<SwmSnippet path="/playbooks/demo_backup_rootvg.yml" line="78">

---

The task for viewing the <SwmToken path="playbooks/demo_backup_rootvg.yml" pos="78:10:10" line-data="    - name: View the mksysb image">`mksysb`</SwmToken> image is defined in the <SwmPath>[playbooks/demo_backup_rootvg.yml](playbooks/demo_backup_rootvg.yml)</SwmPath> playbook.

```yaml
    - name: View the mksysb image
      ibm.power_aix.backup:
        action: view
        type: "{{ type }}"
        location: "{{ location_create }}"
      register: result
    - ansible.builtin.debug:
        var: result
```

---

</SwmSnippet>

## Restore Backup Images

The <SwmToken path="playbooks/demo_backup_datavg.yml" pos="105:10:10" line-data="    - name: Restvg to restore the backup image">`restore`</SwmToken>` `<SwmToken path="playbooks/demo_backup_rootvg.yml" pos="64:5:5" line-data="      ibm.power_aix.backup:">`backup`</SwmToken>` images` function is used to restore a <SwmToken path="playbooks/demo_backup_datavg.yml" pos="2:16:16" line-data="- name: &quot;Backup operations on datavg using savevg and restvg for AIX&quot;">`savevg`</SwmToken> backup image of the data volume group (datavg). This task uses the <SwmToken path="playbooks/demo_backup_rootvg.yml" pos="64:1:5" line-data="      ibm.power_aix.backup:">`ibm.power_aix.backup`</SwmToken> module with the <SwmToken path="playbooks/demo_backup_datavg.yml" pos="105:10:10" line-data="    - name: Restvg to restore the backup image">`restore`</SwmToken> action to restore the backup image to the specified disk.

<SwmSnippet path="/playbooks/demo_backup_datavg.yml" line="105">

---

The task for restoring a <SwmToken path="playbooks/demo_backup_datavg.yml" pos="2:16:16" line-data="- name: &quot;Backup operations on datavg using savevg and restvg for AIX&quot;">`savevg`</SwmToken> backup image of datavg is defined in the <SwmPath>[playbooks/demo_backup_datavg.yml](playbooks/demo_backup_datavg.yml)</SwmPath> playbook.

```yaml
    - name: Restvg to restore the backup image
      ibm.power_aix.backup:
        action: restore
        type: "{{ type_v }}"
        name: "{{ disk_name_v }}"
        location: "{{ location_v }}"
        data_file: "{{ data_file_v }}"
        exclude_data: "{{ exclude_data_v }}"
        minimize_lv_size: "{{ minimize_lv_size_v }}"
        flags: "{{ restvg_flags_v }}"
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
