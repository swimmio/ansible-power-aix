---
title: NIM Overview
---
# NIM Overview

NIM stands for Network Installation Management, which is a system management framework used to manage the installation and maintenance of AIX operating systems. NIM allows administrators to perform various operations on NIM clients, such as software or Base Operating System (BOS) installation, Service Pack (SP) or Technology Level (TL) updates, and reboots.

# NIM Master Server

The NIM master server is responsible for managing NIM clients and resources. It can be configured and managed using the NIM module. The NIM module provides functionality to perform operations like setting up the NIM master server, managing NIM objects, and executing commands on NIM clients.

# NIM Module Actions

The module includes various actions such as `update`, `master_setup`, `check`, `compare`, `script`, `allocate`, `deallocate`, `bos_inst`, `define_script`, `remove`, `reset`, `reboot`, `maintenance`, `show`, and `register_client` to manage NIM operations.

# NIM Module Requirements

The NIM module requires AIX version 7.1 TL3 or newer, Python 3.6 or newer, and a user with root authority to run the NIM command.

# Example Usage of NIM Module

Here are some examples of how to use the NIM module:

- Install using group resource:

```
- name: Install using group resource
  nim:
    action: bos_inst
    targets: nimclient01
    group: basic_res_grp
```

- Check the Cstate of all NIM clients:

```
- name: Check the Cstate of all NIM clients
  nim:
    action: check
```

- Define a script resource on NIM master:

```
- name: Define a script resource on NIM master
  nim:
    action: define_script
    resource: myscript
    location: /tmp/myscript.sh
```

- Execute a script on all NIM clients synchronously:

```
- name: Execute a script on all NIM clients synchronously
  nim:
    action: script
    targets: all
    script: /tmp/myscript.sh
```

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
