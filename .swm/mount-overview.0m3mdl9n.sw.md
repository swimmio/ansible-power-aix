---
title: Mount Overview
---
# Mount Overview

Mount refers to the process of making a filesystem or device available for use at a specified location, known as the mount point. The module can perform actions such as mounting, unmounting, and listing mounted filesystems. It supports various options like specifying the directory path to <SwmToken path="plugins/modules/mount.py" pos="46:13:15" line-data="    - Specifies the directory path to mount/unmount.">`mount/unmount`</SwmToken>, the remote node holding the <SwmToken path="plugins/modules/mount.py" pos="56:15:19" line-data="    - Specifies the remote node holding the directory/filesystem/device to be mounted/unmounted.">`directory/filesystem/device`</SwmToken>, and mounting all filesystems in the <SwmToken path="plugins/modules/mount.py" pos="61:1:4" line-data="      /etc/filesystems file with stanzas that contain the true mount attribute.">`/etc/filesystems`</SwmToken> file. The module requires privileged user authorizations to manage filesystems and devices.

## Mount Function

The <SwmToken path="plugins/modules/mount.py" pos="114:5:5" line-data="  ibm.power_aix.mount:">`mount`</SwmToken> function constructs and executes the appropriate command to mount the specified filesystem or device, handling various parameters and options.

<SwmSnippet path="/plugins/modules/mount.py" line="317">

---

The <SwmToken path="plugins/modules/mount.py" pos="317:2:2" line-data="def mount(module):">`mount`</SwmToken> function constructs the command based on the provided parameters and executes it. It handles options such as alternate filesystems, <SwmToken path="plugins/modules/mount.py" pos="85:15:17" line-data="    - Mounts a file system as a read-only file system.">`read-only`</SwmToken> mounts, and mounting over a directory.

```python
def mount(module):
    """
    Mount the specified device/filesystem
    arguments:
        module  (dict): Ansible module argument spec.
    note:
        Exits with fail_json in case of error
    return:
        none
    """

    cmd = "/usr/sbin/mount "
    alternate_fs = module.params['alternate_fs']
    if alternate_fs:
        cmd += f"-F {alternate_fs} "
    if module.params['removable_fs']:
        cmd += "-p "
    if module.params['read_only']:
        cmd += "-r "
    vfsname = module.params['vfsname']
    if vfsname:
```

---

</SwmSnippet>

## Unmount Function

The <SwmToken path="plugins/modules/mount.py" pos="404:2:2" line-data="def umount(module):">`umount`</SwmToken> function handles the unmounting process, ensuring that the specified filesystem or device is properly unmounted.

<SwmSnippet path="/plugins/modules/mount.py" line="404">

---

The <SwmToken path="plugins/modules/mount.py" pos="404:2:2" line-data="def umount(module):">`umount`</SwmToken> function constructs the command to unmount the specified device or filesystem and executes it. It handles parameters such as force unmount and unmounting all remote filesystems.

```python
def umount(module):
    """
    Unmount the specified device/filesystem
    arguments:
        module  (dict): Ansible module argument spec.
    note:
        Exits with fail_json in case of error
    return:
        none
    """

    mount_all = module.params['mount_all']
    fs_type = module.params['fs_type']
    mount_over_dir = module.params['mount_over_dir']
    node = module.params['node']
    force = module.params['force']

    cmd = "/usr/sbin/umount "
    if force:
        cmd += "-f "
    if fs_type:
```

---

</SwmSnippet>

## List Mounted Filesystems

The <SwmToken path="plugins/modules/mount.py" pos="293:2:2" line-data="def fs_list(module):">`fs_list`</SwmToken> function lists all currently mounted filesystems, providing details about each mount point.

<SwmSnippet path="/plugins/modules/mount.py" line="293">

---

The <SwmToken path="plugins/modules/mount.py" pos="293:2:2" line-data="def fs_list(module):">`fs_list`</SwmToken> function constructs the command to list all mounted filesystems and executes it. It captures the output and handles any errors.

```python
def fs_list(module):
    """
    List mounted filesystems
    arguments:
        module  (dict): Ansible module argument spec.
    note:
        Exits with fail_json in case of error
    return:
        none
    """

    cmd = "/usr/sbin/mount"
    rc, stdout, stderr = module.run_command(cmd)
    result['cmd'] = cmd
    result['rc'] = rc
    result['stdout'] = stdout
    result['stderr'] = stderr
    if rc != 0:
        result['msg'] = "Failed to list mounted filesystems."
        module.fail_json(**result)
```

---

</SwmSnippet>

## Usage Example

Examples of how to use the mount module to list, mount, and unmount filesystems.

<SwmSnippet path="/plugins/modules/mount.py" line="112">

---

This section provides examples of using the mount module to list mounted filesystems, mount filesystems, and mount filesystems provided by a node.

```python
EXAMPLES = r'''
- name: List mounted filesystems
  ibm.power_aix.mount:
    state: show

- name: Mount filesystems
  ibm.power_aix.mount:
    state: mount
    mount_dir: /mnt/tesfs

- name: Mount filesystems provided by a node
  ibm.power_aix.mount:
    state: mount
    node: ansible-test1
    mount_dir: /mnt/servnfs
    mount_over_dir: /mnt/clientnfs
    options: "vers=4"

- name: Mount all filesystems from the 'local' mount group
  ibm.power_aix.mount:
    state: mount
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
