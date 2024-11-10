---
title: NIM Update IOS Overview
---
# NIM Update IOS Overview

NIM Update IOS refers to the process of using the Network Installation Management (NIM) to perform updates and customization on Virtual <SwmToken path="plugins/modules/nim_updateios.py" pos="19:25:27" line-data="short_description: Use NIM to update a single or a pair of Virtual I/O Servers.">`I/O`</SwmToken> Server (VIOS) targets. This module allows for updating a single VIOS or a pair of <SwmToken path="plugins/modules/nim_updateios.py" pos="110:17:17" line-data="- name: Preview updateios on a pair of VIOSes">`VIOSes`</SwmToken> together, ensuring that the <SwmToken path="plugins/modules/nim_updateios.py" pos="110:17:17" line-data="- name: Preview updateios on a pair of VIOSes">`VIOSes`</SwmToken> in a pair are on the same cluster and their node states are OK.

## Update Process

The update process involves checking the status of previous operations, stopping the cluster before updating, performing the update, and then restarting the cluster. The module supports actions such as installing new filesets, committing uncommitted updates, cleaning up incomplete installations, and removing specified filesets.

## Features

The module includes features like previewing the update operation without making changes, managing clusters during updates, and specifying a time limit for the update operation.

## Usage Example

The <SwmToken path="plugins/modules/nim_updateios.py" pos="111:1:1" line-data="  nim_updateios:">`nim_updateios`</SwmToken> module is used to perform updates on VIOS targets. For example, to preview an update on a pair of <SwmToken path="plugins/modules/nim_updateios.py" pos="110:17:17" line-data="- name: Preview updateios on a pair of VIOSes">`VIOSes`</SwmToken>, you can use the following playbook snippet: `- `<SwmToken path="plugins/modules/nim_updateios.py" pos="110:2:2" line-data="- name: Preview updateios on a pair of VIOSes">`name`</SwmToken>`: `<SwmToken path="plugins/modules/nim_updateios.py" pos="110:5:5" line-data="- name: Preview updateios on a pair of VIOSes">`Preview`</SwmToken>` `<SwmToken path="plugins/modules/nim_updateios.py" pos="110:7:7" line-data="- name: Preview updateios on a pair of VIOSes">`updateios`</SwmToken>` `<SwmToken path="plugins/modules/nim_updateios.py" pos="110:9:9" line-data="- name: Preview updateios on a pair of VIOSes">`on`</SwmToken>` a `<SwmToken path="plugins/modules/nim_updateios.py" pos="110:13:13" line-data="- name: Preview updateios on a pair of VIOSes">`pair`</SwmToken>` `<SwmToken path="plugins/modules/nim_updateios.py" pos="110:15:15" line-data="- name: Preview updateios on a pair of VIOSes">`of`</SwmToken>` `<SwmToken path="plugins/modules/nim_updateios.py" pos="110:17:17" line-data="- name: Preview updateios on a pair of VIOSes">`VIOSes`</SwmToken>` `<SwmToken path="plugins/modules/nim_updateios.py" pos="111:1:1" line-data="  nim_updateios:">`nim_updateios`</SwmToken>`: `<SwmToken path="plugins/modules/nim_updateios.py" pos="112:1:1" line-data="    targets: &#39;nimvios01, nimvios02&#39;">`targets`</SwmToken>`: 'nimvios01, `<SwmToken path="plugins/modules/nim_updateios.py" pos="112:8:9" line-data="    targets: &#39;nimvios01, nimvios02&#39;">`nimvios02'`</SwmToken>` `<SwmToken path="plugins/modules/nim_updateios.py" pos="113:1:1" line-data="    action: install">`action`</SwmToken>`: `<SwmToken path="plugins/modules/nim_updateios.py" pos="113:4:4" line-data="    action: install">`install`</SwmToken>` `<SwmToken path="plugins/modules/nim_updateios.py" pos="114:1:1" line-data="    lpp_source: 723lpp_res">`lpp_source`</SwmToken>`: `<SwmToken path="plugins/modules/nim_updateios.py" pos="114:4:4" line-data="    lpp_source: 723lpp_res">`723lpp_res`</SwmToken>` `<SwmToken path="plugins/modules/nim_updateios.py" pos="115:1:1" line-data="    preview: true">`preview`</SwmToken>`: true`.

<SwmSnippet path="/plugins/modules/nim_updateios.py" line="107">

---

This code snippet shows how to use the <SwmToken path="plugins/modules/nim_updateios.py" pos="111:1:1" line-data="  nim_updateios:">`nim_updateios`</SwmToken> module to preview an update on a pair of <SwmToken path="plugins/modules/nim_updateios.py" pos="110:17:17" line-data="- name: Preview updateios on a pair of VIOSes">`VIOSes`</SwmToken>, update <SwmToken path="plugins/modules/nim_updateios.py" pos="110:17:17" line-data="- name: Preview updateios on a pair of VIOSes">`VIOSes`</SwmToken> as a pair and a VIOS alone, and remove a fileset of a VIOS.

```python
'''

EXAMPLES = r'''
- name: Preview updateios on a pair of VIOSes
  nim_updateios:
    targets: 'nimvios01, nimvios02'
    action: install
    lpp_source: 723lpp_res
    preview: true
- name: Update VIOSes as a pair and a VIOS alone discarding cluster
  nim_updateios:
    targets:
      - nimvios01,nimvios02
      - nimvios03
    action: install
    lpp_source: 723lpp_res
    time_limit: '07/21/2020 17:02'
    manage_cluster: false
    preview: false
- name: Remove a fileset of a VIOS
  nim_updateios:
```

---

</SwmSnippet>

## Main Functions

There are several main functions in this folder. Some of them are <SwmToken path="plugins/modules/nim_updateios.py" pos="304:2:2" line-data="def nim_exec(module, node, command):">`nim_exec`</SwmToken>, <SwmToken path="plugins/modules/nim_updateios.py" pos="462:2:2" line-data="def check_vios_targets(module, targets):">`check_vios_targets`</SwmToken>, and <SwmToken path="plugins/modules/nim_updateios.py" pos="111:1:1" line-data="  nim_updateios:">`nim_updateios`</SwmToken>. We will dive a little into <SwmToken path="plugins/modules/nim_updateios.py" pos="304:2:2" line-data="def nim_exec(module, node, command):">`nim_exec`</SwmToken> and <SwmToken path="plugins/modules/nim_updateios.py" pos="111:1:1" line-data="  nim_updateios:">`nim_updateios`</SwmToken>.

### <SwmToken path="plugins/modules/nim_updateios.py" pos="304:2:2" line-data="def nim_exec(module, node, command):">`nim_exec`</SwmToken>

The <SwmToken path="plugins/modules/nim_updateios.py" pos="304:2:2" line-data="def nim_exec(module, node, command):">`nim_exec`</SwmToken> function executes a specified command on a specified NIM client using <SwmToken path="plugins/modules/nim_updateios.py" pos="306:21:21" line-data="    Execute the specified command on the specified nim client using c_rsh.">`c_rsh`</SwmToken>. It constructs the command, runs it, and returns the result code, stdout, and stderr.

<SwmSnippet path="/plugins/modules/nim_updateios.py" line="304">

---

The <SwmToken path="plugins/modules/nim_updateios.py" pos="304:2:2" line-data="def nim_exec(module, node, command):">`nim_exec`</SwmToken> function constructs the command, runs it, and returns the result code, stdout, and stderr.

```python
def nim_exec(module, node, command):
    """
    Execute the specified command on the specified nim client using c_rsh.

    arguments:
        module      (dict): The Ansible module
        node        (dict): nim client to execute the command on to
        command     (list): command to execute
    return:
        rc      (int) return code of the command
        stdout  (str) stdout of the command
        stderr  (str) stderr of the command
    """

    cmd = ' '.join(command)
    rcmd = f'( LC_ALL=C {cmd} ); echo rc=$?'
    cmd = ['/usr/lpp/bos.sysmgt/nim/methods/c_rsh', node, rcmd]

    rc, stdout, stderr = module.run_command(cmd)
    if rc != 0:
        return (rc, stdout, stderr)
```

---

</SwmSnippet>

### <SwmToken path="plugins/modules/nim_updateios.py" pos="462:2:2" line-data="def check_vios_targets(module, targets):">`check_vios_targets`</SwmToken>

The <SwmToken path="plugins/modules/nim_updateios.py" pos="462:2:2" line-data="def check_vios_targets(module, targets):">`check_vios_targets`</SwmToken> function checks the list of VIOS targets to ensure each target can be reached. It validates the target names and ensures they are known by the NIM master.

<SwmSnippet path="/plugins/modules/nim_updateios.py" line="462">

---

The <SwmToken path="plugins/modules/nim_updateios.py" pos="462:2:2" line-data="def check_vios_targets(module, targets):">`check_vios_targets`</SwmToken> function validates the target names and ensures they are known by the NIM master.

```python
def check_vios_targets(module, targets):
    """
    Check the list of VIOS targets.
    Check that each target can be reached.

    A target name can be of the following form:
        vios1,vios2 or vios3

    arguments:
        module  (dict): the Ansible module
        targets (list): list of tuple of NIM name of vios machine
    return:
        res_list    (list): The list of the existing vios tuple matching the target list
    """

    vios_list = []
    res_list = []

    # Build targets list
    for elems in targets:
        module.debug(f'Checking elems: {elems}')
```

---

</SwmSnippet>

### <SwmToken path="plugins/modules/nim_updateios.py" pos="111:1:1" line-data="  nim_updateios:">`nim_updateios`</SwmToken>

The <SwmToken path="plugins/modules/nim_updateios.py" pos="111:1:1" line-data="  nim_updateios:">`nim_updateios`</SwmToken> function executes the updateios command for each VIOS tuple. It retrieves the previous status, checks the cluster name and node status, stops the cluster if necessary, performs the update, waits for the copy to finish, and starts the cluster if necessary.

<SwmSnippet path="/plugins/modules/nim_updateios.py" line="795">

---

The <SwmToken path="plugins/modules/nim_updateios.py" pos="795:2:2" line-data="def nim_updateios(module, targets_list, vios_status, time_limit):">`nim_updateios`</SwmToken> function performs the updateios operation and manages the cluster during the update process.

```python
def nim_updateios(module, targets_list, vios_status, time_limit):
    """
    Execute the updateios command
    For each VIOS tuple,
    - retrieve the previous status if any (looking for SUCCESS-HC and SUCCESS-UPDT)
    - for each VIOS of the tuple, check the cluster name and node status
    - stop the cluster if necessary
    - perform the updateios operation
    - wait for the copy to finish
    - start the cluster if necessary

    arguments:
        module          (dict): The Ansible module
        targets_list    (list): Target tuple list of VIOS
        vios_status     (dict): provided previous status for each tuple
        time_limit       (str): Date and time to perform tuple update
    note:
        Set the update status in results['status'][vios_key].
    return:
        none
    """
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
