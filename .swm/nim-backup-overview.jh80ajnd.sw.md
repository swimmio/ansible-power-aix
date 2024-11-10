---
title: NIM Backup Overview
---
# NIM Backup Overview

NIM Backup refers to the process of creating, listing, viewing, and restoring backup images on LPAR and VIOS clients using the Network Installation Management (NIM) system. The backup operations are performed through the NIM master, which defines <SwmToken path="plugins/modules/nim_backup.py" pos="695:17:17" line-data="    Perform a NIM define operation to create a mksysb">`mksysb`</SwmToken> or <SwmToken path="plugins/modules/nim_backup.py" pos="882:26:26" line-data="    Perform a define NIM operation to create a backup of a VIOS (ios_backup)">`ios_backup`</SwmToken> resources depending on the type of client.

The NIM Backup process includes creating a database backup file, transferring and restoring the database backup on the NIM master, and unconfiguring the NIM master machine that needs to be migrated. The backup files are stored in specified locations, and the process can handle different types of backups such as <SwmToken path="plugins/modules/nim_backup.py" pos="695:17:17" line-data="    Perform a NIM define operation to create a mksysb">`mksysb`</SwmToken>, <SwmToken path="plugins/modules/nim_backup.py" pos="700:29:29" line-data="        objtype  (str): the type of mksysb to create, can be mksysb or ios_mksysb">`ios_mksysb`</SwmToken>, <SwmToken path="plugins/modules/nim_backup.py" pos="882:26:26" line-data="    Perform a define NIM operation to create a backup of a VIOS (ios_backup)">`ios_backup`</SwmToken>, and <SwmToken path="plugins/modules/nim_backup.py" pos="52:5:5" line-data="    - C(savevg) specifies to operate volume group files backup image of LPAR target; that is not">`savevg`</SwmToken>.

# Main Functions

There are several main functions in this folder. Some of them are <SwmToken path="plugins/modules/nim_backup.py" pos="693:2:2" line-data="def nim_mksysb_create(module, target, objtype, params):">`nim_mksysb_create`</SwmToken>, <SwmToken path="plugins/modules/nim_backup.py" pos="757:2:2" line-data="def nim_mksysb_restore(module, target, params):">`nim_mksysb_restore`</SwmToken>, <SwmToken path="plugins/modules/nim_backup.py" pos="880:2:2" line-data="def nim_iosbackup_create(module, target, params):">`nim_iosbackup_create`</SwmToken>, and <SwmToken path="plugins/modules/nim_backup.py" pos="932:2:2" line-data="def nim_iosbackup_restore(module, target, params):">`nim_iosbackup_restore`</SwmToken>. We will dive a little into <SwmToken path="plugins/modules/nim_backup.py" pos="693:2:2" line-data="def nim_mksysb_create(module, target, objtype, params):">`nim_mksysb_create`</SwmToken> and <SwmToken path="plugins/modules/nim_backup.py" pos="880:2:2" line-data="def nim_iosbackup_create(module, target, params):">`nim_iosbackup_create`</SwmToken>.

<SwmSnippet path="/plugins/modules/nim_backup.py" line="693">

---

## <SwmToken path="plugins/modules/nim_backup.py" pos="693:2:2" line-data="def nim_mksysb_create(module, target, objtype, params):">`nim_mksysb_create`</SwmToken>

The <SwmToken path="plugins/modules/nim_backup.py" pos="693:2:2" line-data="def nim_mksysb_create(module, target, objtype, params):">`nim_mksysb_create`</SwmToken> function performs a NIM define operation to create a <SwmToken path="plugins/modules/nim_backup.py" pos="695:17:17" line-data="    Perform a NIM define operation to create a mksysb">`mksysb`</SwmToken> backup. It builds the name for the backup, computes the backup location, and constructs the NIM command to create the <SwmToken path="plugins/modules/nim_backup.py" pos="695:17:17" line-data="    Perform a NIM define operation to create a mksysb">`mksysb`</SwmToken> resource. If the module is not in check mode, it executes the command and updates the results with the status of the operation.

```python
def nim_mksysb_create(module, target, objtype, params):
    """
    Perform a NIM define operation to create a mksysb

    arguments:
        module  (dict): the module variable
        target   (str): the NIM Client to backup
        objtype  (str): the type of mksysb to create, can be mksysb or ios_mksysb
        params  (dict): the NIM command parameters
    note:
        set results['status'][target] with the status
    return:
        True if backup succeeded or skipped
        False otherwise
    """

    name = build_name(target, params['name'], params['name_prefix'], params['name_postfix'])

    # compute backup location
    if not os.path.exists(params['location']):
        os.makedirs(params['location'])
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/nim_backup.py" line="880">

---

## <SwmToken path="plugins/modules/nim_backup.py" pos="880:2:2" line-data="def nim_iosbackup_create(module, target, params):">`nim_iosbackup_create`</SwmToken>

The <SwmToken path="plugins/modules/nim_backup.py" pos="880:2:2" line-data="def nim_iosbackup_create(module, target, params):">`nim_iosbackup_create`</SwmToken> function performs a define NIM operation to create a backup of a VIOS (<SwmToken path="plugins/modules/nim_backup.py" pos="882:26:26" line-data="    Perform a define NIM operation to create a backup of a VIOS (ios_backup)">`ios_backup`</SwmToken>). It builds the name for the backup, computes the backup location, and constructs the NIM command to create the <SwmToken path="plugins/modules/nim_backup.py" pos="882:26:26" line-data="    Perform a define NIM operation to create a backup of a VIOS (ios_backup)">`ios_backup`</SwmToken> resource. If the module is not in check mode, it executes the command and updates the results with the status of the operation.

```python
def nim_iosbackup_create(module, target, params):
    """
    Perform a define NIM operation to create a backup of a VIOS (ios_backup)

    arguments:
        module  (dict): the module variable
        target   (str): the VIOS NIM Client to backup
        params  (dict): the NIM command parameters
    note:
        set results['status'][target] with the status
    return:
        True if restore succeeded or skipped
        False otherwise
    """

    name = build_name(target, params['name'], params['name_prefix'], params['name_postfix'])

    # compute backup location
    if not os.path.exists(params['location']):
        os.makedirs(params['location'])
    location = os.path.join(params['location'], name)
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
