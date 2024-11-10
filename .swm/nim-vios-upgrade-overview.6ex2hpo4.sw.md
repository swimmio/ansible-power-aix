---
title: NIM VIOS Upgrade Overview
---
# Overview

NIM VIOS Upgrade refers to the process of upgrading Virtual <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="23:15:17" line-data="  and logical configuration data of the Virtual I/O Server (VIOS) from the NIM master.">`I/O`</SwmToken> Servers (VIOS) using the Network Installation Management (NIM) tool from a NIM master. The <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="18:3:3" line-data="module: nim_viosupgrade">`nim_viosupgrade`</SwmToken> module is used to perform this upgrade, which includes backing up virtual and logical configuration data, installing the specified image, and restoring the configuration data.

# Supported Actions

The module supports different actions such as <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="38:5:5" line-data="    - C(bosinst) to perform a new and fresh installation on the current rootvg disk.">`bosinst`</SwmToken> for a new installation on the current rootvg disk, <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="39:5:5" line-data="    - C(altdisk) to perform a new installation on the alternative disk. The current rootvg disk on">`altdisk`</SwmToken> for installation on an alternative disk, and <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="37:5:5" line-data="    - C(get_status) to get the status of an ongoing upgrade operation.">`get_status`</SwmToken> to check the status of an ongoing upgrade operation.

# Requirements

The module requires AIX version <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="29:6:8" line-data="- AIX &gt;= 7.2 TL3">`7.2`</SwmToken> <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="29:10:10" line-data="- AIX &gt;= 7.2 TL3">`TL3`</SwmToken> or later, Python <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="30:6:8" line-data="- Python &gt;= 3.6">`3.6`</SwmToken> or later, and a privileged user with specific authorizations.

# Module Updates

The <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="18:3:3" line-data="module: nim_viosupgrade">`nim_viosupgrade`</SwmToken> module has been updated to fix issues related to altdisk and bosinst operations, ensuring a smoother upgrade process.

# Main Functions

There are several main functions in this module. Some of them are <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="299:2:2" line-data="def param_one_of(one_of_list, required=True, exclusive=True):">`param_one_of`</SwmToken>, <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="328:2:2" line-data="def refresh_nim_node(module, type):">`refresh_nim_node`</SwmToken>, <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="494:2:2" line-data="def viosupgrade_query(module, params_flags):">`viosupgrade_query`</SwmToken>, and <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="513:3:3" line-data="    # viosupgrade -q { [-n hostname | -f filename] }">`viosupgrade`</SwmToken>. We will dive a little into <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="299:2:2" line-data="def param_one_of(one_of_list, required=True, exclusive=True):">`param_one_of`</SwmToken> and <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="513:3:3" line-data="    # viosupgrade -q { [-n hostname | -f filename] }">`viosupgrade`</SwmToken>.

## <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="299:2:2" line-data="def param_one_of(one_of_list, required=True, exclusive=True):">`param_one_of`</SwmToken>

The <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="299:2:2" line-data="def param_one_of(one_of_list, required=True, exclusive=True):">`param_one_of`</SwmToken> function checks that at least one parameter from a given list is defined in the module's parameters dictionary. It ensures that either one or none of the parameters is defined based on the <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="299:7:7" line-data="def param_one_of(one_of_list, required=True, exclusive=True):">`required`</SwmToken> and <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="299:12:12" line-data="def param_one_of(one_of_list, required=True, exclusive=True):">`exclusive`</SwmToken> flags.

<SwmSnippet path="/plugins/modules/nim_viosupgrade.py" line="299">

---

The <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="299:2:2" line-data="def param_one_of(one_of_list, required=True, exclusive=True):">`param_one_of`</SwmToken> function ensures that at least one parameter from a given list is defined in the module's parameters dictionary.

```python
def param_one_of(one_of_list, required=True, exclusive=True):
    """
    Check that parameter of one_of_list is defined in module.params dictionary.

    arguments:
        one_of_list (list) list of parameter to check
        required    (bool) at least one parameter has to be defined.
        exclusive   (bool) only one parameter can be defined.
    note:
        Ansible might have this embedded in some version: require_if 4th parameter.
        Exits with fail_json in case of error
    """

    count = 0
    for param in one_of_list:
        if module.params[param] is not None and module.params[param]:
            count += 1
            break
    action = module.params['action']
    if count == 0 and required:
        results['msg'] = f'Missing parameter: action is {action} but one of the following is missing: '
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="328:2:2" line-data="def refresh_nim_node(module, type):">`refresh_nim_node`</SwmToken>

The <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="328:2:2" line-data="def refresh_nim_node(module, type):">`refresh_nim_node`</SwmToken> function retrieves NIM client information of a specified type and updates the <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="330:19:19" line-data="    Get nim client information of provided type and update nim_node dictionary.">`nim_node`</SwmToken> dictionary with this information. It ensures that the NIM node information is current and accurate.

<SwmSnippet path="/plugins/modules/nim_viosupgrade.py" line="328">

---

The <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="328:2:2" line-data="def refresh_nim_node(module, type):">`refresh_nim_node`</SwmToken> function retrieves NIM client information of a specified type and updates the <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="330:19:19" line-data="    Get nim client information of provided type and update nim_node dictionary.">`nim_node`</SwmToken> dictionary with this information.

```python
def refresh_nim_node(module, type):
    """
    Get nim client information of provided type and update nim_node dictionary.

    arguments:
        module  (dict): The Ansible module
        type     (str): type of the nim object to get information
    note:
        Exits with fail_json in case of error
    return:
        none
    """

    if module.params['nim_node']:
        results['nim_node'] = module.params['nim_node']

    nim_info = get_nim_type_info(module, type)

    if type not in results['nim_node']:
        results['nim_node'].update({type: nim_info})
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="494:2:2" line-data="def viosupgrade_query(module, params_flags):">`viosupgrade_query`</SwmToken>

The <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="494:2:2" line-data="def viosupgrade_query(module, params_flags):">`viosupgrade_query`</SwmToken> function queries the status of the VIOS upgrade. It constructs and runs the appropriate command to get the status and updates the results dictionary with the command output and status information.

<SwmSnippet path="/plugins/modules/nim_viosupgrade.py" line="494">

---

The <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="494:2:2" line-data="def viosupgrade_query(module, params_flags):">`viosupgrade_query`</SwmToken> function queries the status of the VIOS upgrade and updates the results dictionary with the command output and status information.

```python
def viosupgrade_query(module, params_flags):
    """
    Query to get the status of the upgrade .

    arguments:
        module        (dict): The Ansible module
        params_flags  (dict): Supported parameter flags.
    module.param used:
        target_file   (optional) filename with targets info
        targets       (required if not target_file)
        viosupgrade_params  (required)
    note:
        Set the upgrade status in results['status'][vios] or results['status']['all'].
    return:
        ret     (int) the number of error
    """

    ret = 0

    # viosupgrade -q { [-n hostname | -f filename] }
    cmd = ['/usr/sbin/viosupgrade', '-q']
```

---

</SwmSnippet>

## viosupgrade

The <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="513:3:3" line-data="    # viosupgrade -q { [-n hostname | -f filename] }">`viosupgrade`</SwmToken> function performs the actual upgrade of each VIOS. It constructs the command based on the provided parameters, runs the command, and updates the results dictionary with the command output and status information.

<SwmSnippet path="/plugins/modules/nim_viosupgrade.py" line="569">

---

The <SwmToken path="plugins/modules/nim_viosupgrade.py" pos="569:2:2" line-data="def viosupgrade(module, params_flags):">`viosupgrade`</SwmToken> function performs the actual upgrade of each VIOS and updates the results dictionary with the command output and status information.

```python
def viosupgrade(module, params_flags):
    """
    Upgrade each VIOS.

    arguments:
        module        (dict): The Ansible module
        params_flags  (dict): Supported parameter flags.
    module.param used:
        action              (required)
        target_file         (optional) filename with targets info
        targets             (required if not target_file)
        viosupgrade_params  (required)
    note:
        Set the upgrade status in results['status'][vios] or results['status']['all'].
    return:
        ret     (int) the number of error
    """

    ret = 0

    cmd = ['/usr/sbin/viosupgrade']
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
