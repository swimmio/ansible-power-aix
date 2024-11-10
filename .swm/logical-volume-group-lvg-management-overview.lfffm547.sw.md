---
title: Logical Volume Group (LVG) Management Overview
---
# LVG Overview

LVG refers to the Logical Volume Group management module in the ansible-power-aix repository. It is used to create, remove, modify attributes, resize, activate, and deactivate volume groups on AIX systems. The module supports various actions such as creating a volume group, extending it by adding physical volumes, and removing physical volumes from it. It also allows for setting specific attributes like the type of volume group, enabling enhanced concurrent capabilities, and specifying critical physical volumes.

# Usage of LVG

The LVG module is used in various scenarios such as creating a scalable volume group with a mirror pool, creating a volume group with multiple physical volumes, extending a volume group, removing a volume group, and varying on or off a volume group. This section provides examples of how to use the LVG module for different operations like creating, extending, and removing volume groups.

<SwmSnippet path="/plugins/modules/lvg.py" line="205">

---

Examples of using the LVG module to create, extend, and remove volume groups.

```python
- name: Create a scalable volume group with a mirror pool
  ibm.power_aix.lvg:
    state: present
    vg_name: datavg
    pvs: hdisk1
    vg_type: scalable
    mirror_pool: mp1

- name: Create a volume group with multiple physical volumes
  ibm.power_aix.lvg:
    state: present
    vg_name: datavg
    pvs: hdisk1 hdisk2 hdisk3

- name: Extend a volume group
  ibm.power_aix.lvg:
    state: present
    vg_name: datavg
    pvs: hdisk1

- name: Remove a volume group
```

---

</SwmSnippet>

# Main Functions

There are several main functions in this folder. Some of them are <SwmToken path="plugins/modules/lvg.py" pos="283:2:2" line-data="def make_vg(module, vg_name):">`make_vg`</SwmToken>, <SwmToken path="plugins/modules/lvg.py" pos="332:2:2" line-data="def extend_vg(module, vg_name, vg_state, init_props):">`extend_vg`</SwmToken>, <SwmToken path="plugins/modules/lvg.py" pos="384:2:2" line-data="def change_vg(module, vg_name, init_props):">`change_vg`</SwmToken>, <SwmToken path="plugins/modules/lvg.py" pos="433:2:2" line-data="def reduce_vg(module, vg_name, vg_state):">`reduce_vg`</SwmToken>, and <SwmToken path="plugins/modules/lvg.py" pos="505:2:2" line-data="def vary_vg(module, state, vg_name, vg_state):">`vary_vg`</SwmToken>. We will dive a little into <SwmToken path="plugins/modules/lvg.py" pos="283:2:2" line-data="def make_vg(module, vg_name):">`make_vg`</SwmToken> and <SwmToken path="plugins/modules/lvg.py" pos="332:2:2" line-data="def extend_vg(module, vg_name, vg_state, init_props):">`extend_vg`</SwmToken>.

## <SwmToken path="plugins/modules/lvg.py" pos="283:2:2" line-data="def make_vg(module, vg_name):">`make_vg`</SwmToken>

The <SwmToken path="plugins/modules/lvg.py" pos="283:2:2" line-data="def make_vg(module, vg_name):">`make_vg`</SwmToken> function is responsible for creating a volume group. It constructs the necessary command options and executes the command to create the volume group.

<SwmSnippet path="/plugins/modules/lvg.py" line="283">

---

The <SwmToken path="plugins/modules/lvg.py" pos="283:2:2" line-data="def make_vg(module, vg_name):">`make_vg`</SwmToken> function constructs the necessary command options and executes the command to create the volume group.

```python
def make_vg(module, vg_name):
    """
    Creates volume group
    arguments:
        module:     Ansible module argument spec.
        vg_name:    Volume group name
    note:
        Exits with fail_json in case of error
    return:
        none
    """

    pvs = module.params['pvs']
    opt = build_vg_opts(module)

    vg_type_opt = {
        "none": '',
        "big": '-B ',
        "scalable": '-S ',
    }
    vg_type = module.params["vg_type"]
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/lvg.py" pos="332:2:2" line-data="def extend_vg(module, vg_name, vg_state, init_props):">`extend_vg`</SwmToken>

The <SwmToken path="plugins/modules/lvg.py" pos="332:2:2" line-data="def extend_vg(module, vg_name, vg_state, init_props):">`extend_vg`</SwmToken> function extends an existing volume group by adding physical volumes to it. It ensures the volume group is active before performing the extension.

<SwmSnippet path="/plugins/modules/lvg.py" line="332">

---

The <SwmToken path="plugins/modules/lvg.py" pos="332:2:2" line-data="def extend_vg(module, vg_name, vg_state, init_props):">`extend_vg`</SwmToken> function ensures the volume group is active before performing the extension.

```python
def extend_vg(module, vg_name, vg_state, init_props):
    """
    Extends a varied on volume group
    arguments:
        module:     Ansible module argument spec.
        vg_name:    Volume group name
        vg_state:   Volume group state
        init_props: Initial properties of the volume group
    note:
        Exits with fail_json in case of error
    return:
        none
    """

    # fail extendvg if vg is varied off
    varied_on = vg_state
    if not varied_on:
        result['rc'] = 1
        result['msg'] = f"Unable to extend volume group { vg_name } because it is not varied on."
        module.fail_json(**result)
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/lvg.py" pos="384:2:2" line-data="def change_vg(module, vg_name, init_props):">`change_vg`</SwmToken>

The <SwmToken path="plugins/modules/lvg.py" pos="384:2:2" line-data="def change_vg(module, vg_name, init_props):">`change_vg`</SwmToken> function modifies the attributes of an existing volume group. It builds the necessary command options and executes the command to change the volume group.

## <SwmToken path="plugins/modules/lvg.py" pos="433:2:2" line-data="def reduce_vg(module, vg_name, vg_state):">`reduce_vg`</SwmToken>

The <SwmToken path="plugins/modules/lvg.py" pos="433:2:2" line-data="def reduce_vg(module, vg_name, vg_state):">`reduce_vg`</SwmToken> function reduces a volume group by removing physical volumes from it. It checks the current state of the volume group and ensures it is active before performing the reduction.

<SwmSnippet path="/plugins/modules/lvg.py" line="433">

---

The <SwmToken path="plugins/modules/lvg.py" pos="433:2:2" line-data="def reduce_vg(module, vg_name, vg_state):">`reduce_vg`</SwmToken> function checks the current state of the volume group and ensures it is active before performing the reduction.

```python
def reduce_vg(module, vg_name, vg_state):
    """
    Reduce volume group
    arguments:
        module: Ansible module argument spec.
        vg_name: Volume group name
        vg_state: Volume group state
    note:
        Exits with fail_json in case of error
    return:
        none
    """

    pvs = module.params['pvs']

    if vg_state is False:
        result['msg'] = f"Volume group {vg_name} is deactivated. Unable to reduce volume group."
        module.fail_json(**result)

    elif vg_state is None:
        result['msg'] = f"Volume group {vg_name} does not exist. Unable to reduce volume group."
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/lvg.py" line="505">

---

The <SwmToken path="plugins/modules/lvg.py" pos="505:2:2" line-data="def vary_vg(module, state, vg_name, vg_state):">`vary_vg`</SwmToken> function checks the current state of the volume group and executes the appropriate command to varyon or varyoff the volume group.

```python
def vary_vg(module, state, vg_name, vg_state):
    """
    Varyon/off volume group
    arguments:
        module: Ansible module argument spec.
        state: requested action, can be varyon or varyoff
        vg_name: Volume group name
        vg_state: Volume group state
    note:
        Exits with fail_json in case of error
    return:
        none
    """

    if vg_state is None:
        result['msg'] = f"Volume group {vg_name} does not exist."
        module.fail_json(**result)

    if state == 'varyon':
        if vg_state is True:
            result['msg'] += f"Volume group {vg_name} is already active. "
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
