---
title: NIM SUMA Overview
---
# NIM SUMA Overview

NIM SUMA refers to the use of the Service Update Management Assistant (SUMA) to download fixes, service packs (SP), or technology levels (TL) from the IBM Fix Central website. The fixes are downloaded to update specified target systems to a specific technology level or service pack level. The downloaded fixes are then used to create a related NIM resource on the NIM master. This allows users to run another task to install the systems with the new resource. Updates are downloaded based on the lowest technical level of all target systems.

# How to Use NIM SUMA

To use NIM SUMA, you need to specify the action (either 'download' or 'preview'), the target systems, the desired OS level, and optionally the download directory and other parameters. The <SwmToken path="plugins/modules/nim_suma.py" pos="802:2:2" line-data="def suma_download(module, suma_params):">`suma_download`</SwmToken> function handles the process of building the NIM <SwmToken path="plugins/modules/nim_suma.py" pos="64:9:9" line-data="    - Name of the lpp_source NIM resource to create when I(action=download).">`lpp_source`</SwmToken> list, getting the NIM clients, expanding the target list, and computing the necessary parameters for the SUMA command.

# Where NIM SUMA is Used

The NIM SUMA functionality is primarily used in the <SwmToken path="plugins/modules/nim_suma.py" pos="802:2:2" line-data="def suma_download(module, suma_params):">`suma_download`</SwmToken> function within the <SwmPath>[plugins/modules/nim_suma.py](plugins/modules/nim_suma.py)</SwmPath> module.

# Example Usage of NIM SUMA

An example usage of NIM SUMA is to check, download, and create the NIM resource to install the latest updates on a target system. This can be done using the <SwmToken path="plugins/modules/nim_suma.py" pos="126:1:1" line-data="  nim_suma:">`nim_suma`</SwmToken> module with the action set to 'download', specifying the target system and the desired OS level.

<SwmSnippet path="/plugins/modules/nim_suma.py" line="122">

---

This code snippet shows an example of how to use the <SwmToken path="plugins/modules/nim_suma.py" pos="126:1:1" line-data="  nim_suma:">`nim_suma`</SwmToken> module to check, download, and create the NIM resource to install the latest updates on a target system.

```python
'''

EXAMPLES = r'''
- name: Check, download and create the NIM resource to install latest updates
  nim_suma:
    action: download
    targets: nimclient01
    oslevel: Latest
    download_dir: /usr/sys/inst.images
'''
```

---

</SwmSnippet>

# Main Functions

There are several main functions in this folder. Some of them are <SwmToken path="plugins/modules/nim_suma.py" pos="194:2:2" line-data="def min_oslevel(dic):">`min_oslevel`</SwmToken>, <SwmToken path="plugins/modules/nim_suma.py" pos="212:2:2" line-data="def max_oslevel(dic):">`max_oslevel`</SwmToken>, <SwmToken path="plugins/modules/nim_suma.py" pos="230:2:2" line-data="def nim_exec(module, node, command):">`nim_exec`</SwmToken>, <SwmToken path="plugins/modules/nim_suma.py" pos="260:2:2" line-data="def run_oslevel_cmd(module, machine, oslevels):">`run_oslevel_cmd`</SwmToken>, <SwmToken path="plugins/modules/nim_suma.py" pos="293:2:2" line-data="def expand_targets(module, targets, nim_clients):">`expand_targets`</SwmToken>, <SwmToken path="plugins/modules/nim_suma.py" pos="355:2:2" line-data="def get_nim_clients(module):">`get_nim_clients`</SwmToken>, <SwmToken path="plugins/modules/nim_suma.py" pos="385:2:2" line-data="def get_oslevels(module, targets):">`get_oslevels`</SwmToken>, <SwmToken path="plugins/modules/nim_suma.py" pos="412:2:2" line-data="def get_nim_lpp_source(module):">`get_nim_lpp_source`</SwmToken>, <SwmToken path="plugins/modules/nim_suma.py" pos="453:2:2" line-data="def compute_rq_type(module, oslevel, targets_list):">`compute_rq_type`</SwmToken>, <SwmToken path="plugins/modules/nim_suma.py" pos="479:2:2" line-data="def find_sp_version(module, file):">`find_sp_version`</SwmToken>, <SwmToken path="plugins/modules/nim_suma.py" pos="507:2:2" line-data="def compute_rq_name(module, suma_params, rq_type, oslevel, clients_target_oslevel):">`compute_rq_name`</SwmToken>, <SwmToken path="plugins/modules/nim_suma.py" pos="659:2:2" line-data="def compute_filter_ml(module, clients_target_oslevel, rq_name):">`compute_filter_ml`</SwmToken>, <SwmToken path="plugins/modules/nim_suma.py" pos="691:2:2" line-data="def compute_lpp_source_name(module, lpp_source, rq_name):">`compute_lpp_source_name`</SwmToken>, <SwmToken path="plugins/modules/nim_suma.py" pos="715:2:2" line-data="def compute_dl_target(module, download_dir, lpp_source, nim_lpp_sources):">`compute_dl_target`</SwmToken>, <SwmToken path="plugins/modules/nim_suma.py" pos="751:2:2" line-data="def suma_command(module, action, suma_params):">`suma_command`</SwmToken>, and <SwmToken path="plugins/modules/nim_suma.py" pos="802:2:2" line-data="def suma_download(module, suma_params):">`suma_download`</SwmToken>. We will dive a little into <SwmToken path="plugins/modules/nim_suma.py" pos="802:2:2" line-data="def suma_download(module, suma_params):">`suma_download`</SwmToken> and <SwmToken path="plugins/modules/nim_suma.py" pos="751:2:2" line-data="def suma_command(module, action, suma_params):">`suma_command`</SwmToken>.

## <SwmToken path="plugins/modules/nim_suma.py" pos="802:2:2" line-data="def suma_download(module, suma_params):">`suma_download`</SwmToken>

The <SwmToken path="plugins/modules/nim_suma.py" pos="802:2:2" line-data="def suma_download(module, suma_params):">`suma_download`</SwmToken> function handles the download or preview action for SUMA. It validates the input parameters, builds the necessary lists of NIM clients and targets, and retrieves the OS levels of the specified targets. It then computes various parameters required for the SUMA command and finally calls the <SwmToken path="plugins/modules/nim_suma.py" pos="751:2:2" line-data="def suma_command(module, action, suma_params):">`suma_command`</SwmToken> function to execute the SUMA command.

<SwmSnippet path="/plugins/modules/nim_suma.py" line="802">

---

This code snippet shows the <SwmToken path="plugins/modules/nim_suma.py" pos="802:2:2" line-data="def suma_download(module, suma_params):">`suma_download`</SwmToken> function which handles the download or preview action for SUMA.

```python
def suma_download(module, suma_params):
    """
    Dowload (or preview) action

    suma_params['action'] should be set to either 'preview' or 'download'.

    arguments:
        module      (dict): The Ansible module
        suma_params (dict): parameters to build the suma command
    note:
        Exits with fail_json in case of error
    """

    targets_list = suma_params['targets']
    req_oslevel = suma_params['req_oslevel']

    if not targets_list:
        if req_oslevel == 'Latest':
            msg = 'Oslevel target could not be empty or equal "Latest" when' \
                  ' target machine list is empty'
            module.log(msg)
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/nim_suma.py" pos="751:2:2" line-data="def suma_command(module, action, suma_params):">`suma_command`</SwmToken>

The <SwmToken path="plugins/modules/nim_suma.py" pos="751:2:2" line-data="def suma_command(module, action, suma_params):">`suma_command`</SwmToken> function constructs and runs the SUMA command based on the provided parameters. It handles the execution of the SUMA command, captures the output, and handles any errors that occur during the execution.

<SwmSnippet path="/plugins/modules/nim_suma.py" line="751">

---

This code snippet shows the <SwmToken path="plugins/modules/nim_suma.py" pos="751:2:2" line-data="def suma_command(module, action, suma_params):">`suma_command`</SwmToken> function which constructs and runs the SUMA command based on the provided parameters.

```python
def suma_command(module, action, suma_params):
    """
    Run a suma command.

    arguments:
        module      (dict): The Ansible module
        action       (str): preview or download
        suma_params (dict): parameters to build the suma command
    note:
        Exits with fail_json in case of error
    return:
       stdout  suma command output
    """

    rq_type = suma_params['RqType']
    if rq_type == 'Latest':
        rq_type = 'SP'

    cmd_FilterML = suma_params['FilterMl']
    cmd_DLTarget = suma_params['DLTarget']
    cmd_RqName = suma_params['RqName']
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
