---
title: SUMA Overview
---
# SUMA Overview

SUMA stands for Service Update Management Assistant, which sets up an automated interface to download fixes from the IBM Fix Central website to your systems. SUMA provides flexible, <SwmToken path="plugins/modules/suma.py" pos="29:9:11" line-data="- SUMA provides flexible, task-based options to periodically check the availability of specific new">`task-based`</SwmToken> options to periodically check the availability of specific new fixes, technology levels (TL), and service packs (SP). System administrators do not have to manually retrieve maintenance updates as SUMA automates this process.

# How to Use SUMA

SUMA can perform various actions such as downloading and installing fixes, previewing available updates, listing all SUMA tasks, editing existing tasks, running tasks, unscheduling tasks, deleting tasks, and listing global SUMA configuration settings. SUMA tasks can be scheduled to run at specific times, and the results of these tasks are logged in the system for review.

# Requirements

SUMA requires AIX version <SwmToken path="plugins/modules/suma.py" pos="34:6:8" line-data="- AIX &gt;= 7.1 TL3">`7.1`</SwmToken> <SwmToken path="plugins/modules/suma.py" pos="34:10:10" line-data="- AIX &gt;= 7.1 TL3">`TL3`</SwmToken> or higher and Python version <SwmToken path="plugins/modules/suma.py" pos="35:6:8" line-data="- Python &gt;= 3.6">`3.6`</SwmToken> or higher.

# Example Usage

This example demonstrates how to check, download, and install system updates for the current oslevel of the system using SUMA.

<SwmSnippet path="/plugins/modules/suma.py" line="148">

---

The following code snippets show how to use SUMA to check, download, and install system updates for different oslevels.

```python
- name: Check, download and install system updates for the current oslevel of the system
  suma:
    action: download
    oslevel: Latest
    download_dir: /usr/sys/inst.images

- name: Check and download required to update to SP 7.2.3.2
  suma:
    action: download
    oslevel: '7200-03-02'
    download_only: true
    download_dir: /tmp/dl_updt_7200-03-02
  when: ansible_distribution == 'AIX'

- name: Check, download and install to latest SP of TL 7.2.4
  suma:
    action: download
    oslevel: '7200-04'
    last_sp: true
    extend_fs: false
```

---

</SwmSnippet>

# Main Functions

There are several main functions in this folder. Some of them are <SwmToken path="plugins/modules/suma.py" pos="650:2:2" line-data="def suma_download():">`suma_download`</SwmToken>, <SwmToken path="plugins/modules/suma.py" pos="438:2:2" line-data="def suma_list():">`suma_list`</SwmToken>, <SwmToken path="plugins/modules/suma.py" pos="485:2:2" line-data="def suma_edit():">`suma_edit`</SwmToken>, <SwmToken path="plugins/modules/suma.py" pos="537:2:2" line-data="def suma_unschedule():">`suma_unschedule`</SwmToken>, <SwmToken path="plugins/modules/suma.py" pos="560:2:2" line-data="def suma_delete():">`suma_delete`</SwmToken>, <SwmToken path="plugins/modules/suma.py" pos="583:2:2" line-data="def suma_run():">`suma_run`</SwmToken>, <SwmToken path="plugins/modules/suma.py" pos="606:2:2" line-data="def suma_config():">`suma_config`</SwmToken>, and <SwmToken path="plugins/modules/suma.py" pos="628:2:2" line-data="def suma_default():">`suma_default`</SwmToken>. We will dive a little into <SwmToken path="plugins/modules/suma.py" pos="650:2:2" line-data="def suma_download():">`suma_download`</SwmToken> and <SwmToken path="plugins/modules/suma.py" pos="438:2:2" line-data="def suma_list():">`suma_list`</SwmToken>.

## <SwmToken path="plugins/modules/suma.py" pos="650:2:2" line-data="def suma_download():">`suma_download`</SwmToken>

The <SwmToken path="plugins/modules/suma.py" pos="650:2:2" line-data="def suma_download():">`suma_download`</SwmToken> function handles the download and installation (or preview) of updates. It computes all SUMA request options, performs a preview to check for available updates, and if updates are found, it proceeds to download and install them.

<SwmSnippet path="/plugins/modules/suma.py" line="650">

---

The <SwmToken path="plugins/modules/suma.py" pos="650:2:2" line-data="def suma_download():">`suma_download`</SwmToken> function is responsible for downloading and installing updates. It first computes all SUMA request options, performs a preview to check for available updates, and if updates are found, it proceeds to download and install them.

```python
def suma_download():
    """
    Download / Install (or preview) action

    suma_params['action'] should be set to either 'preview' or 'download'.

    First compute all Suma request options. Then preform a Suma preview, parse
    output to check there is something to download, if so, do a suma download
    if needed (if action is Download). If suma download output mentions there
    is downloaded items, then use install_all_updates command to install them.

    note:
        Exits with fail_json in case of error
    """

    # Check oslevel format
    if not suma_params['oslevel'].strip() or suma_params['oslevel'].upper() == 'LATEST':
        suma_params['oslevel'] = 'Latest'
    else:
        if re.match(r"^[0-9]{4}(|-00|-00-00|-00-00-0000)$", suma_params['oslevel']):
            msg_oslevel = suma_params['oslevel']
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/suma.py" pos="438:2:2" line-data="def suma_list():">`suma_list`</SwmToken>

The <SwmToken path="plugins/modules/suma.py" pos="438:2:2" line-data="def suma_list():">`suma_list`</SwmToken> function lists all SUMA tasks or the task associated with a given task ID. It constructs the appropriate command and executes it, capturing the output and handling any errors.

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
