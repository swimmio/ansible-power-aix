---
title: Tunables Overview
---
# Tunables Overview

Tunables refer to parameters that can be modified, reset, or displayed for various components on AIX systems. The module allows users to specify actions such as showing, modifying, or resetting the values of these tunables. Components like 'vmo', 'ioo', 'schedo', 'no', 'raso', 'nfso', and 'asoo' have their own set of tunables that can be managed.

## Requirements

The module requires root user access and supports AIX versions <SwmToken path="plugins/modules/tunables.py" pos="20:6:8" line-data="- AIX &gt;= 7.1 TL3">`7.1`</SwmToken> <SwmToken path="plugins/modules/tunables.py" pos="20:10:10" line-data="- AIX &gt;= 7.1 TL3">`TL3`</SwmToken> and above. Users can specify whether the changes should apply to the current values, reboot values, or both. The module also supports handling restricted tunables, which require special permissions to modify.

## Main Functions

There are several main functions in this module. Some of them are <SwmToken path="plugins/modules/tunables.py" pos="257:2:2" line-data="def create_tunables_dict(module):">`create_tunables_dict`</SwmToken>, <SwmToken path="plugins/modules/tunables.py" pos="287:2:2" line-data="def get_valid_tunables(module):">`get_valid_tunables`</SwmToken>, <SwmToken path="plugins/modules/tunables.py" pos="104:4:4" line-data="    action: show">`show`</SwmToken>, <SwmToken path="plugins/modules/tunables.py" pos="179:4:4" line-data="    action: reset">`reset`</SwmToken>, and <SwmToken path="plugins/modules/tunables.py" pos="88:4:4" line-data="    action: modify">`modify`</SwmToken>. We will dive a little into <SwmToken path="plugins/modules/tunables.py" pos="257:2:2" line-data="def create_tunables_dict(module):">`create_tunables_dict`</SwmToken> and <SwmToken path="plugins/modules/tunables.py" pos="287:2:2" line-data="def get_valid_tunables(module):">`get_valid_tunables`</SwmToken>.

<SwmSnippet path="/plugins/modules/tunables.py" line="257">

---

### <SwmToken path="plugins/modules/tunables.py" pos="257:2:2" line-data="def create_tunables_dict(module):">`create_tunables_dict`</SwmToken>

The <SwmToken path="plugins/modules/tunables.py" pos="257:2:2" line-data="def create_tunables_dict(module):">`create_tunables_dict`</SwmToken> function is used to create a dictionary of tunables and their values for validation purposes. It runs a command to fetch the current tunables and converts the output into a dictionary.

```python
def create_tunables_dict(module):
    '''
    Utility function to create tunables dictionary with values

    arguments:
        module  (dict): The Ansible module
    note:
        Exits with fail_json in case of error
    return:
        creates a dictionary with all tunables and their values
    '''

    component = module.params['component']
    global tunables_dict
    cmd = component + ' -F -x'

    rc, std_out, std_err = module.run_command(cmd, use_unsafe_shell=True)

    if rc != 0:
        # In case command returns non zero return code, fail case
        results['msg'] = "Failed to get tunables existing values for validation."
```

---

</SwmSnippet>

## Actions

The module supports three main actions: <SwmToken path="plugins/modules/tunables.py" pos="104:4:4" line-data="    action: show">`show`</SwmToken>, <SwmToken path="plugins/modules/tunables.py" pos="88:4:4" line-data="    action: modify">`modify`</SwmToken>, and <SwmToken path="plugins/modules/tunables.py" pos="179:4:4" line-data="    action: reset">`reset`</SwmToken>. Each action is handled by its respective function.

<SwmSnippet path="/plugins/modules/tunables.py" line="405">

---

### show

The <SwmToken path="plugins/modules/tunables.py" pos="405:2:2" line-data="def show(module):">`show`</SwmToken> function handles the action of displaying tunable parameters. It forms a command based on the specified component and tunable parameters, executes it, and updates the global results dictionary with the output.

```python
def show(module):
    '''
    Handles the show action

    arguments:
        module  (dict): The Ansible module
    note:
        Exits with fail_json in case of error
    return:
        updated global results dictionary.
    '''
    tunable_params = module.params['tunable_params']
    restricted_tunables = module.params['restricted_tunables']
    component = module.params['component']
    tunables_to_show = ''
    cmd = ''

    # according to the component form the initial command
    cmd += component + ' '

    # Force display of the restricted tunable parameters
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/tunables.py" line="534">

---

### modify

The <SwmToken path="plugins/modules/tunables.py" pos="534:2:2" line-data="def modify(module):">`modify`</SwmToken> function handles the action of modifying tunable parameters. It validates the new values, forms a command based on the specified component and tunable parameters, executes it, and updates the global results dictionary with the output.

```python
def modify(module):
    '''
    Handles the modify action

    arguments:
        module  (dict): The Ansible module
    note:
        Exits with fail_json in case of error
    return:
        Message for successful command along with display standard output
    '''

    component = module.params['component']
    bosboot_tunables = module.params['bosboot_tunables']
    change_type = module.params['change_type']
    restricted_tunables = module.params['restricted_tunables']
    tunable_params_with_value = module.params['tunable_params_with_value']
    parameters = ''
    changed_tunables = ''
    cmd = ''
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/tunables.py" line="453">

---

### reset

The <SwmToken path="plugins/modules/tunables.py" pos="453:2:2" line-data="def reset(module):">`reset`</SwmToken> function handles the action of resetting tunable parameters to their default values. It forms a command based on the specified component and tunable parameters, executes it, and updates the global results dictionary with the output.

```python
def reset(module):
    '''
    Handles the reset action

    arguments:
        module  (dict): The Ansible module
    note:
        Exits with fail_json in case of error
    return:
        Updated global results dictionary
    '''

    component = module.params['component']
    tunable_params = module.params['tunable_params']
    bosboot_tunables = module.params['bosboot_tunables']
    change_type = module.params['change_type']
    restricted_tunables = module.params['restricted_tunables']
    changed_tunables = ''
    cmd = ''

    # use of -r and -p together is prohibited. Reason explained in the message.
```

---

</SwmSnippet>

## Examples

Here are some examples demonstrating how to use the <SwmToken path="plugins/modules/tunables.py" pos="87:5:5" line-data="  ibm.power_aix.tunables:">`tunables`</SwmToken> module for different actions.

<SwmSnippet path="/plugins/modules/tunables.py" line="86">

---

### Modify Tunables

This example demonstrates how to modify <SwmToken path="plugins/modules/tunables.py" pos="86:8:8" line-data="- name: &quot;Modify vmo tunable parameters&quot;">`vmo`</SwmToken> tunable parameters using the <SwmToken path="plugins/modules/tunables.py" pos="87:5:5" line-data="  ibm.power_aix.tunables:">`tunables`</SwmToken> module.

```python
- name: "Modify vmo tunable parameters"
  ibm.power_aix.tunables:
    action: modify
    component: vmo
    tunable_params_with_value:
      lgpg_regions: 10
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/tunables.py" line="103">

---

### Show Tunables

This example demonstrates how to display information of all dynamic tunable parameters using the <SwmToken path="plugins/modules/tunables.py" pos="103:5:5" line-data="  ibm.power_aix.tunables:">`tunables`</SwmToken> module.

```python
  ibm.power_aix.tunables:
    action: show
    component: vmo
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/tunables.py" line="175">

---

### Reset Tunables

This example demonstrates how to reset <SwmToken path="plugins/modules/tunables.py" pos="175:8:8" line-data="- name: &quot;Reset bosboot tunables/ restricted bosboot tunables&quot;">`bosboot`</SwmToken> tunables using the <SwmToken path="plugins/modules/tunables.py" pos="175:10:10" line-data="- name: &quot;Reset bosboot tunables/ restricted bosboot tunables&quot;">`tunables`</SwmToken> module.

```python
- name: "Reset bosboot tunables/ restricted bosboot tunables"
  # this will only change the REBOOT VALUE and needs bosboot and reboot.
  # example of one restricted and one non restricted bosboot tunable
  ibm.power_aix.tunables:
    action: reset
    component: vmo
    restricted_tunables: true # because of restricted tunables otherwise default is false
    bosboot_tunables: true # because of bosboot tunables otherwise default is false
    tunable_params: ['kernel_heap_psize', 'batch_tlb']
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
