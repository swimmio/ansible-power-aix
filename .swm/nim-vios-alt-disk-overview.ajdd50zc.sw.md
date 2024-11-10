---
title: NIM VIOS Alt Disk Overview
---
# NIM VIOS Alt Disk Overview

NIM VIOS Alt Disk refers to the process of creating or cleaning an alternate root volume group (rootvg) disk for VIOS clients using Network Installation Management (NIM). This process involves copying the root volume group to an alternate disk or cleaning an existing copy, which can be useful for system migrations or backups.

## Purpose and Functionality

The alternate disk specified for the NIM Alt Disk Migration is used to perform the migration tasks, ensuring that the system can be restored or migrated without affecting the primary root volume group. The module <SwmToken path="plugins/modules/nim_vios_alt_disk.py" pos="94:1:1" line-data="  nim_vios_alt_disk:">`nim_vios_alt_disk`</SwmToken> in the repository provides the functionality to perform these operations, including specifying the target VIOS clients and the disks to be used for the alternate disk copy. The operations supported include <SwmToken path="plugins/modules/nim_vios_alt_disk.py" pos="95:4:4" line-data="    action: alt_disk_copy">`alt_disk_copy`</SwmToken> to perform the alternate disk copy and <SwmToken path="plugins/modules/nim_vios_alt_disk.py" pos="110:4:4" line-data="    action: alt_disk_clean">`alt_disk_clean`</SwmToken> to clean up an existing alternate disk copy.

## Example Usage

This example demonstrates how to perform an alternate disk copy of the rootvg to <SwmToken path="plugins/modules/nim_vios_alt_disk.py" pos="93:23:23" line-data="- name: Perform an alternate disk copy of the rootvg to hdisk1 on nimvios01">`hdisk1`</SwmToken> on <SwmToken path="plugins/modules/nim_vios_alt_disk.py" pos="93:27:27" line-data="- name: Perform an alternate disk copy of the rootvg to hdisk1 on nimvios01">`nimvios01`</SwmToken> using the <SwmToken path="plugins/modules/nim_vios_alt_disk.py" pos="94:1:1" line-data="  nim_vios_alt_disk:">`nim_vios_alt_disk`</SwmToken> module.

<SwmSnippet path="/plugins/modules/nim_vios_alt_disk.py" line="93">

---

This code snippet shows how to perform an alternate disk copy of the rootvg to <SwmToken path="plugins/modules/nim_vios_alt_disk.py" pos="93:23:23" line-data="- name: Perform an alternate disk copy of the rootvg to hdisk1 on nimvios01">`hdisk1`</SwmToken> on <SwmToken path="plugins/modules/nim_vios_alt_disk.py" pos="93:27:27" line-data="- name: Perform an alternate disk copy of the rootvg to hdisk1 on nimvios01">`nimvios01`</SwmToken>.

```python
- name: Perform an alternate disk copy of the rootvg to hdisk1 on nimvios01
  nim_vios_alt_disk:
    action: alt_disk_copy
    targets:
      - nimvios01: [hdisk1]
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/nim_vios_alt_disk.py" line="108">

---

This code snippet shows how to perform a cleanup of any existing alternate disk copy on <SwmToken path="plugins/modules/nim_vios_alt_disk.py" pos="108:25:25" line-data="- name: Perform a cleanup of any existing alternate disk copy on nimvios01">`nimvios01`</SwmToken>.

```python
- name: Perform a cleanup of any existing alternate disk copy on nimvios01
  nim_vios_alt_disk:
    action: alt_disk_clean
```

---

</SwmSnippet>

## Example Playbook

The example playbook shows how to use the `nim_alt_disk_migration` role to perform an alternate disk migration without validating the lpp or spot resources.

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
