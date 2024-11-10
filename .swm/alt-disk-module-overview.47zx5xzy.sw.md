---
title: Alt Disk Module Overview
---
# Alt Disk Overview

Alt Disk refers to the management of an alternate root volume group (rootvg) disk. It involves copying the rootvg to an alternate disk or cleaning up an existing one on a logical partition (LPAR).

# Operations on Alt Disk

The alternate disk can be used for various operations such as copying the rootvg, cleaning up an existing alternate disk copy, or installing filesets and fixes on an existing alternate disk. The module provides options to specify the target disks, choose a disk based on size policy, and perform actions like copying, cleaning, or installing on the alternate disk.

# Ensuring Validity

The <SwmToken path="plugins/modules/alt_disk.py" pos="145:1:1" line-data="  alt_disk:">`alt_disk`</SwmToken> module ensures that the rootvg is properly checked, and valid alternate disks are found and validated before performing the operations. This ensures that the operations are performed on the correct and intended disks.

# Configuration Options

The module also includes options to handle specific configurations such as running bootlist after the copy, copying specific files, resetting device configurations, and running customization scripts during the initial boot of the alternate rootvg.

<SwmSnippet path="/plugins/modules/alt_disk.py" line="144">

---

Example of performing an alternate disk copy of the rootvg to <SwmToken path="plugins/modules/alt_disk.py" pos="144:23:23" line-data="- name: Perform an alternate disk copy of the rootvg to hdisk1">`hdisk1`</SwmToken>.

```python
- name: Perform an alternate disk copy of the rootvg to hdisk1
  alt_disk:
    action: copy
    targets: hdisk1
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/alt_disk.py" line="149">

---

Example of performing an alternate disk copy of the rootvg to the smallest disk that can be selected.

```python
- name: Perform an alternate disk copy of the rootvg to the smallest disk that can be selected
  alt_disk:
    action: copy
    disk_size_policy: minimize

```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/alt_disk.py" line="155">

---

Example of performing a cleanup of any existing alternate disk copy.

```python
  alt_disk:
    action: clean

```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/alt_disk.py" line="159">

---

Example of performing a cleanup of any existing alternate disk copy and old rootvg.

```python
  alt_disk:
    action: clean
    allow_old_rootvg: true
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
