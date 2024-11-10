---
title: LVOL Overview
---
# LVOL Overview

LVOL refers to the logical volume management module in AIX systems. It is used to create, remove, and modify attributes of logical volumes.

# How to Use LVOL

The module supports various actions such as creating a logical volume, removing a logical volume, and modifying the attributes of an existing logical volume. It requires specifying the volume group name, logical volume name, and other attributes like size, type, and policy. The module ensures that the logical volume is created with the specified attributes or modified if it already exists. If the state is set to 'absent', the module will remove the specified logical volume. The module also supports additional options like specifying the strip size, number of copies, and extra options for the <SwmToken path="plugins/modules/lvol.py" pos="71:23:23" line-data="    - Any other options to be passed by the user to mklv or chlv command">`mklv`</SwmToken> or <SwmToken path="plugins/modules/lvol.py" pos="71:27:27" line-data="    - Any other options to be passed by the user to mklv or chlv command">`chlv`</SwmToken> commands. It is important to note that using the 'absent' state will destroy all data in the specified logical volumes.

# Example Usage

This section provides examples of how to use the LVOL module to create, extend, and remove logical volumes.

<SwmSnippet path="/plugins/modules/lvol.py" line="127">

---

Examples of creating logical volumes with different attributes.

```python
EXAMPLES = r'''
- name: Create a logical volume of 64M
  ibm.power_aix.lvol:
    vg: test1vg
    lv: test1lv
    size: "64M"
    state: present
- name: Create a logical volume of 10 logical partitions with disks testdisk1 and testdisk2
  ibm.power_aix.lvol:
    vg: test2vg
    lv: test2lv
    size: "10"
    pv_list: testdisk1, testdisk2
    state: present
- name: Create a logical volume of 32M with a minimum placement policy
  ibm.power_aix.lvol:
    vg: rootvg
    lv: test4lv
    size: "32M"
    policy: minimum
    state: present
```

---

</SwmSnippet>

# Main Functions

There are several main functions in this module. Some of them are <SwmToken path="plugins/modules/lvol.py" pos="221:2:2" line-data="def create_lv(module, name):">`create_lv`</SwmToken>, <SwmToken path="plugins/modules/lvol.py" pos="282:2:2" line-data="def extend_lv(module, name, init_props):">`extend_lv`</SwmToken>, <SwmToken path="plugins/modules/lvol.py" pos="385:2:2" line-data="def modify_lv(module, name, init_props):">`modify_lv`</SwmToken>, and <SwmToken path="plugins/modules/lvol.py" pos="449:2:2" line-data="def remove_lv(module, name):">`remove_lv`</SwmToken>. We will dive a little into <SwmToken path="plugins/modules/lvol.py" pos="221:2:2" line-data="def create_lv(module, name):">`create_lv`</SwmToken> and <SwmToken path="plugins/modules/lvol.py" pos="282:2:2" line-data="def extend_lv(module, name, init_props):">`extend_lv`</SwmToken>.

## <SwmToken path="plugins/modules/lvol.py" pos="221:2:2" line-data="def create_lv(module, name):">`create_lv`</SwmToken>

The <SwmToken path="plugins/modules/lvol.py" pos="221:2:2" line-data="def create_lv(module, name):">`create_lv`</SwmToken> function is used to create a logical volume with the attributes provided in the <SwmToken path="plugins/modules/lvol.py" pos="224:1:1" line-data="    lv_attributes field.">`lv_attributes`</SwmToken> field. It constructs the command to create the logical volume and executes it.

<SwmSnippet path="/plugins/modules/lvol.py" line="221">

---

The <SwmToken path="plugins/modules/lvol.py" pos="221:2:2" line-data="def create_lv(module, name):">`create_lv`</SwmToken> function constructs and executes the command to create a logical volume.

```python
def create_lv(module, name):
    """
    Creates a logical volume with the attributes provided in the
    lv_attributes field.
    arguments:
        module (dict): The Ansible module
        name (str): Logical Volume Name
    note:
        Exits with fail_json in case of error
    return:
        none
    """

    opts = ''
    lv_type = module.params['lv_type']
    strip_size = module.params['strip_size']
    copies = module.params['copies']
    policy = module.params['policy']
    vg = module.params['vg']
    num_log_part = module.params['size']
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/lvol.py" pos="282:2:2" line-data="def extend_lv(module, name, init_props):">`extend_lv`</SwmToken>

The <SwmToken path="plugins/modules/lvol.py" pos="282:2:2" line-data="def extend_lv(module, name, init_props):">`extend_lv`</SwmToken> function is used to extend a logical volume with the given new size. It fetches the current properties of the logical volume and calculates the new size before executing the command to extend the logical volume.

<SwmSnippet path="/plugins/modules/lvol.py" line="282">

---

The <SwmToken path="plugins/modules/lvol.py" pos="282:2:2" line-data="def extend_lv(module, name, init_props):">`extend_lv`</SwmToken> function fetches current properties and calculates the new size before extending the logical volume.

```python
def extend_lv(module, name, init_props):
    """
    Extend a logical volume with the given new size.
    arguments:
        module (dict): The Ansible module
        name (str): Logical Volume Name
        init_props (str): Initial properties of the logical volume
    note:
        The new size must be large than the original size.
    return:
        none
    """

    # get lvid to fetch additonal information
    pattern = r"^LV IDENTIFIER:\s+(\w+\.\d+)"
    lvid = re.search(pattern, init_props, re.MULTILINE).group(1)

    # get the physical partition size (PP size) for converting
    # size with prefixes B/b, K/k, M/m, and G/g into corresponding
    # number of logical partitions
    # also fetch the current lv size in logical partions (LPs)
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/lvol.py" pos="385:2:2" line-data="def modify_lv(module, name, init_props):">`modify_lv`</SwmToken>

The <SwmToken path="plugins/modules/lvol.py" pos="385:2:2" line-data="def modify_lv(module, name, init_props):">`modify_lv`</SwmToken> function is used to modify a logical volume with the attributes provided in the <SwmToken path="plugins/modules/lvol.py" pos="224:1:1" line-data="    lv_attributes field.">`lv_attributes`</SwmToken> field. It constructs the command to modify the logical volume and executes it.

<SwmSnippet path="/plugins/modules/lvol.py" line="385">

---

The <SwmToken path="plugins/modules/lvol.py" pos="385:2:2" line-data="def modify_lv(module, name, init_props):">`modify_lv`</SwmToken> function constructs and executes the command to modify a logical volume.

```python
def modify_lv(module, name, init_props):
    """
    Modify a logical volume with the attributes provided in the
    lv_attributes field.
    arguments:
        module (dict): The Ansible module
        name (str): Logical Volume Name
        init_props (str): Initial properties of the logical volume
    note:
        Exits with fail_json in case of error
    return:
        none
    """

    new_name = module.params['lv_new_name']
    copies = module.params['copies']
    policy = module.params['policy']
    lv_type = module.params['lv_type']
    # -a position, -b badblocks, -d schedule, -R preferredRead,
    # -L label, -o y/n, -p permission, -r relocate, -s strict,
    # -u upperbound, -v verify, -w mirrorwriteconsistency, -x maxumum
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/lvol.py" pos="449:2:2" line-data="def remove_lv(module, name):">`remove_lv`</SwmToken>

The <SwmToken path="plugins/modules/lvol.py" pos="449:2:2" line-data="def remove_lv(module, name):">`remove_lv`</SwmToken> function is used to remove the logical volume without confirmation. It constructs the command to remove the logical volume and executes it.

<SwmSnippet path="/plugins/modules/lvol.py" line="449">

---

The <SwmToken path="plugins/modules/lvol.py" pos="449:2:2" line-data="def remove_lv(module, name):">`remove_lv`</SwmToken> function constructs and executes the command to remove a logical volume.

```python
def remove_lv(module, name):
    """
    Remove the logical volume without confirmation.
    arguments:
        module  (dict): The Ansible module
        name (str): Logical Volume Name
    note:
        Exits with fail_json in case of error
    return:
        none
    """

    cmd = f'rmlv -f {name}'
    success_msg = f"Logical volume {name} removed."
    fail_msg = f"Failed to remove the logical volume: {name}"
    lv_run_cmd(module, cmd, success_msg, fail_msg, None)
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
