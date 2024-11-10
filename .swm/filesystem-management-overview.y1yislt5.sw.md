---
title: Filesystem Management Overview
---
# Filesystem Overview

The filesystem module in the IBM Power Systems AIX Collection manages local and NFS filesystems. It allows for the creation, modification, and removal of these filesystems, supporting various attributes such as mount points, volume groups, and filesystem types. Additionally, it includes options for setting permissions, auto-mounting, and accounting subsystems.

# Filesystem Management

The module ensures that specified actions are performed on the filesystems, such as mounting, unmounting, or removing them. Below is an example of how to use the module to create, modify, and remove filesystems.

<SwmSnippet path="/plugins/modules/filesystem.py" line="116">

---

Examples of creating, modifying, and removing filesystems using the module.

```python
EXAMPLES = r'''
- name: Creation of a JFS2 filesystem
  ibm.power_aix.filesystem:
    state: present
    filesystem: /mnt3
    fs_type: jfs2
    attributes: size=32768,isnapshot='no'
    mount_group: test
    vg: rootvg
- name: Increase size of a filesystem
  ibm.power_aix.filesystem:
    filesystem: /mnt3
    state: present
    attributes: size=+5M
- name: Remove a NFS filesystem
  ibm.power_aix.filesystem:
    filesystem: /mnt
    state: absent
    rm_mount_point: true
'''
```

---

</SwmSnippet>

# Main Functions

The filesystem module includes several main functions that handle different aspects of filesystem management. These functions include <SwmToken path="plugins/modules/filesystem.py" pos="160:2:2" line-data="def is_nfs(module, filesystem):">`is_nfs`</SwmToken>, <SwmToken path="plugins/modules/filesystem.py" pos="180:2:2" line-data="def valid_attributes(module):">`valid_attributes`</SwmToken>, <SwmToken path="plugins/modules/filesystem.py" pos="202:2:2" line-data="def fs_state(module, filesystem):">`fs_state`</SwmToken>, <SwmToken path="plugins/modules/filesystem.py" pos="231:2:2" line-data="def compare_attrs(module):">`compare_attrs`</SwmToken>, <SwmToken path="plugins/modules/filesystem.py" pos="362:2:2" line-data="def nfs_opts(module):">`nfs_opts`</SwmToken>, <SwmToken path="plugins/modules/filesystem.py" pos="389:2:2" line-data="def fs_opts(module):">`fs_opts`</SwmToken>, <SwmToken path="plugins/modules/filesystem.py" pos="182:13:13" line-data="    Returns list of valid attributes for chfs command">`chfs`</SwmToken>, <SwmToken path="plugins/modules/filesystem.py" pos="464:2:2" line-data="def mkfs(module, filesystem):">`mkfs`</SwmToken>, <SwmToken path="plugins/modules/filesystem.py" pos="521:2:2" line-data="def rmfs(module, filesystem):">`rmfs`</SwmToken>, and <SwmToken path="plugins/modules/filesystem.py" pos="560:2:2" line-data="def main():">`main`</SwmToken>.

## <SwmToken path="plugins/modules/filesystem.py" pos="160:2:2" line-data="def is_nfs(module, filesystem):">`is_nfs`</SwmToken>

The <SwmToken path="plugins/modules/filesystem.py" pos="160:2:2" line-data="def is_nfs(module, filesystem):">`is_nfs`</SwmToken> function determines if a filesystem is of NFS type or not. It runs the <SwmToken path="plugins/modules/filesystem.py" pos="167:7:10" line-data="    cmd = f&quot;lsfs -l { filesystem }&quot;">`lsfs -l`</SwmToken> command and checks the type of the filesystem.

<SwmSnippet path="/plugins/modules/filesystem.py" line="160">

---

The <SwmToken path="plugins/modules/filesystem.py" pos="160:2:2" line-data="def is_nfs(module, filesystem):">`is_nfs`</SwmToken> function implementation.

```python
def is_nfs(module, filesystem):
    """
    Determines if a filesystem is NFS or not
    param module: Ansible module argument spec.
    param filesystem: filesystem name.
    return: True - filesystem is NFS type / False - filesystem is not NFS type
    """
    cmd = f"lsfs -l { filesystem }"
    rc, stdout = module.run_command(cmd)[:2]
    if rc != 0:
        return None

    ln = stdout.splitlines()[1:][0]
    type = ln.split()[3]

    if type == "nfs":
        return True
    return False
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/filesystem.py" pos="180:2:2" line-data="def valid_attributes(module):">`valid_attributes`</SwmToken>

The <SwmToken path="plugins/modules/filesystem.py" pos="180:2:2" line-data="def valid_attributes(module):">`valid_attributes`</SwmToken> function returns a list of valid attributes for the <SwmToken path="plugins/modules/filesystem.py" pos="182:13:13" line-data="    Returns list of valid attributes for chfs command">`chfs`</SwmToken> command. It filters out attributes that are not supported.

<SwmSnippet path="/plugins/modules/filesystem.py" line="180">

---

The <SwmToken path="plugins/modules/filesystem.py" pos="180:2:2" line-data="def valid_attributes(module):">`valid_attributes`</SwmToken> function implementation.

```python
def valid_attributes(module):
    """
    Returns list of valid attributes for chfs command
    param:
        module - Ansible module argument spec.
    return:
        valid_attrs (list) - List of valid attributes among the provided ones.
    """
    attr = module.params['attributes']
    if attr is None:
        return True

    valid_attrs = []

    for attributes in attr:
        if attributes.split("=")[0] in crfs_specific_attributes:
            continue
        valid_attrs.append(attributes)

    return valid_attrs
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/filesystem.py" pos="202:2:2" line-data="def fs_state(module, filesystem):">`fs_state`</SwmToken>

The <SwmToken path="plugins/modules/filesystem.py" pos="202:2:2" line-data="def fs_state(module, filesystem):">`fs_state`</SwmToken> function determines the current state of a filesystem. It checks if the filesystem is mounted, unmounted, or does not exist.

<SwmSnippet path="/plugins/modules/filesystem.py" line="202">

---

The <SwmToken path="plugins/modules/filesystem.py" pos="202:2:2" line-data="def fs_state(module, filesystem):">`fs_state`</SwmToken> function implementation.

```python
def fs_state(module, filesystem):
    """
    Determines the current state of filesystem.
    param module: Ansible module argument spec.
    param filesystem: filesystem name.
    return: True - filesystem in mounted state / False - filesystem in unmounted state /
             None - filesystem does not exist
    """

    cmd = f"lsfs -l { filesystem }"
    rc = module.run_command(cmd)[0]
    if rc != 0:
        return None

    cmd = "df"
    rc, stdout = module.run_command(cmd)[:2]
    if rc != 0:
        module.fail_json(f"Command { cmd } failed.")

    if stdout:
        mdirs = []
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/filesystem.py" pos="231:2:2" line-data="def compare_attrs(module):">`compare_attrs`</SwmToken>

The <SwmToken path="plugins/modules/filesystem.py" pos="231:2:2" line-data="def compare_attrs(module):">`compare_attrs`</SwmToken> function compares the provided attributes with the existing attributes of a filesystem. It returns a list of attributes that need to be updated.

<SwmSnippet path="/plugins/modules/filesystem.py" line="231">

---

The <SwmToken path="plugins/modules/filesystem.py" pos="231:2:2" line-data="def compare_attrs(module):">`compare_attrs`</SwmToken> function implementation.

```python
def compare_attrs(module):
    """
    Helper function to compare the provided and already existing attributes of a filesystem
    params:
        module - Ansible module argument spec.
    return:
        updated_attrs (list) - List of updated attributes and their values, that need to be changed
    """

    fs_mount_pt = module.params['filesystem']
    cmd1 = f"lsfs -c {fs_mount_pt}"
    cmd2 = f"lsfs -q {fs_mount_pt}"

    rc1, stdout1, stderr1 = module.run_command(cmd1)

    if rc1:
        result['stdout'] = stdout1
        result['cmd'] = cmd1
        result['stderr'] = stderr1
        result['msg'] = "Could not get information about the provided filesystem."
        module.fail_json(**result)
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/filesystem.py" pos="362:2:2" line-data="def nfs_opts(module):">`nfs_opts`</SwmToken>

The <SwmToken path="plugins/modules/filesystem.py" pos="362:2:2" line-data="def nfs_opts(module):">`nfs_opts`</SwmToken> function builds NFS parameters for the <SwmToken path="plugins/modules/filesystem.py" pos="364:15:15" line-data="    Helper function to build NFS parameters for mknfsmnt and chnfsmnt.">`mknfsmnt`</SwmToken> and <SwmToken path="plugins/modules/filesystem.py" pos="364:19:19" line-data="    Helper function to build NFS parameters for mknfsmnt and chnfsmnt.">`chnfsmnt`</SwmToken> commands. It constructs options based on the module parameters.

<SwmSnippet path="/plugins/modules/filesystem.py" line="362">

---

The <SwmToken path="plugins/modules/filesystem.py" pos="362:2:2" line-data="def nfs_opts(module):">`nfs_opts`</SwmToken> function implementation.

```python
def nfs_opts(module):
    """
    Helper function to build NFS parameters for mknfsmnt and chnfsmnt.
    """
    amount = module.params["auto_mount"]
    perms = module.params["permissions"]
    mgroup = module.params["mount_group"]
    nfs_soft_mount = module.params["nfs_soft_mount"]

    opts = ""
    if amount == "yes":
        opts += "-A "
    elif amount == "no":
        opts += "-a "

    if nfs_soft_mount:
        opts += "-S "

    if perms:
        opts += f"-t { perms } "
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/filesystem.py" pos="389:2:2" line-data="def fs_opts(module):">`fs_opts`</SwmToken>

The <SwmToken path="plugins/modules/filesystem.py" pos="389:2:2" line-data="def fs_opts(module):">`fs_opts`</SwmToken> function builds filesystem parameters for the <SwmToken path="plugins/modules/filesystem.py" pos="391:15:15" line-data="    Helper function to build filesystem parameters for crfs and chfs.">`crfs`</SwmToken> and <SwmToken path="plugins/modules/filesystem.py" pos="182:13:13" line-data="    Returns list of valid attributes for chfs command">`chfs`</SwmToken> commands. It constructs options based on the module parameters.

<SwmSnippet path="/plugins/modules/filesystem.py" line="389">

---

The <SwmToken path="plugins/modules/filesystem.py" pos="389:2:2" line-data="def fs_opts(module):">`fs_opts`</SwmToken> function implementation.

```python
def fs_opts(module):
    """
    Helper function to build filesystem parameters for crfs and chfs.
    """
    amount = module.params["auto_mount"]
    perms = module.params["permissions"]
    mgroup = module.params["mount_group"]
    attrs = module.params["attributes"]
    acct_sub_sys = module.params["account_subsystem"]

    opts = ""
    if amount:
        opts += f"-A {amount} "

    if attrs:
        opts += "-a " + ' -a '.join(attrs) + " "
    else:
        opts += ""

    if mgroup:
        opts += f"-u { mgroup } "
```

---

</SwmSnippet>

## chfs

The <SwmToken path="plugins/modules/filesystem.py" pos="182:13:13" line-data="    Returns list of valid attributes for chfs command">`chfs`</SwmToken> function changes the attributes of a filesystem. It constructs and runs the appropriate command to modify the filesystem.

<SwmSnippet path="/plugins/modules/filesystem.py" line="420">

---

The <SwmToken path="plugins/modules/filesystem.py" pos="420:2:2" line-data="def chfs(module, filesystem):">`chfs`</SwmToken> function implementation.

```python
def chfs(module, filesystem):
    """
    Changes the attributes of the filesystem.
    param module: Ansible module argument spec.
    param filesystem: Filesystem name.
    return: changed - True/False(filesystem state modified or not),
            msg - message
    """
    amount = module.params["auto_mount"]
    perms = module.params["permissions"]
    mgroup = module.params["mount_group"]
    acct_sub_sys = module.params["account_subsystem"]

    # compare initial and the provided attributes. Exit if no change is required.
    if module.params['attributes'] or amount or perms or mgroup or acct_sub_sys:
        compare_attrs(module)

    opts = ""
    nfs = is_nfs(module, filesystem)
    if nfs:
        opts = nfs_opts(module)
```

---

</SwmSnippet>

## mkfs

The <SwmToken path="plugins/modules/filesystem.py" pos="464:2:2" line-data="def mkfs(module, filesystem):">`mkfs`</SwmToken> function creates a filesystem. It constructs and runs the appropriate command to create either a local or NFS filesystem.

<SwmSnippet path="/plugins/modules/filesystem.py" line="464">

---

The <SwmToken path="plugins/modules/filesystem.py" pos="464:2:2" line-data="def mkfs(module, filesystem):">`mkfs`</SwmToken> function implementation.

```python
def mkfs(module, filesystem):
    """
    Create filesystem.
    param module: Ansible module argument spec.
    param filesystem: filesystem name.
    return: changed - True/False(filesystem state created or not),
            msg - message
    """

    nfs_server = module.params['nfs_server']
    device = module.params['device']
    if device:
        device = f"-d { device } "
    else:
        device = ""

    if nfs_server:
        # Create NFS Filesystem
        opts = nfs_opts(module)

        cmd = f"mknfsmnt -f { filesystem } { device } -h { nfs_server } { opts } -w bg "
```

---

</SwmSnippet>

## rmfs

The <SwmToken path="plugins/modules/filesystem.py" pos="521:2:2" line-data="def rmfs(module, filesystem):">`rmfs`</SwmToken> function removes a filesystem. It constructs and runs the appropriate command to remove either a local or NFS filesystem.

<SwmSnippet path="/plugins/modules/filesystem.py" line="521">

---

The <SwmToken path="plugins/modules/filesystem.py" pos="521:2:2" line-data="def rmfs(module, filesystem):">`rmfs`</SwmToken> function implementation.

```python
def rmfs(module, filesystem):
    """
    Remove the filesystem
    param module: Ansible module argument spec.
    param filesystem: filesystem name.
    return: changed - True/False(filesystem state modified or not),
            msg - message
    """

    rm_mount_point = module.params["rm_mount_point"]

    if is_nfs(module, filesystem):
        if rm_mount_point:
            cmd = "rmnfsmnt -B -f "
        else:
            cmd = "rmnfsmnt -I -f "
    else:
        if rm_mount_point:
            cmd = "rmfs -r "
        else:
            cmd = "rmfs "
```

---

</SwmSnippet>

## main

The <SwmToken path="plugins/modules/filesystem.py" pos="560:2:2" line-data="def main():">`main`</SwmToken> function is the entry point of the module. It handles the creation, modification, and removal of filesystems based on the specified state.

<SwmSnippet path="/plugins/modules/filesystem.py" line="560">

---

The <SwmToken path="plugins/modules/filesystem.py" pos="560:2:2" line-data="def main():">`main`</SwmToken> function implementation.

```python
def main():
    global result

    module = AnsibleModule(
        supports_check_mode=False,
        argument_spec=dict(
            attributes=dict(type='list', elements='str'),
            account_subsystem=dict(type='str', choices=['yes', 'no']),
            auto_mount=dict(type='str', choices=['yes', 'no']),
            device=dict(type='str'),
            vg=dict(type='str'),
            fs_type=dict(type='str', default='jfs2'),
            permissions=dict(type='str', choices=['rw', 'ro']),
            mount_group=dict(type='str'),
            nfs_server=dict(type='str'),
            nfs_soft_mount=dict(type='bool', default='False'),
            state=dict(type='str', default='present', choices=['absent', 'present']),
            rm_mount_point=dict(type='bool', default='false'),
            filesystem=dict(type='str', required=True),
        ),
    )
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
