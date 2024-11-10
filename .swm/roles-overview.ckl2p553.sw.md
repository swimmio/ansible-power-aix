---
title: Roles Overview
---
# Roles Overview

Roles in the IBM Power Systems AIX collection are used in a playbook to automate tasks on AIX. Ansible executes each role, usually on the remote target node, and collects return values. Roles build on the idea of include files and combine them to form clean, reusable abstractions. Different roles perform different tasks, but their interfaces and responses follow similar patterns.

## Role Reference

Reference material for each role contains documentation on what parameters certain roles accept and what values they expect those parameters to be.

## Running and Debugging

To run a role, you include it in your playbook using the `include_role` directive and specify any necessary variables.

### Debugging Roles

To debug roles, you can use specific options and variables provided by the role. For example, in the `nim_alt_disk_migration` role, you can set `nim_alt_disk_migration_debug_skip_nimadm` to `true` to execute all validations and exit before performing the migration. This is similar to the check mode in Ansible modules and is useful for debugging purposes.

## Endpoints of Roles

Endpoints of roles are specific functions or methods that perform key tasks within the role. These endpoints are crucial for the role's functionality and often interact with external systems or resources.

### <SwmToken path="roles/power_aix_vioshc/files/vioshc.py" pos="234:2:2" line-data="def retrieve_usr_pass(hmc_info):">`retrieve_usr_pass`</SwmToken>

The <SwmToken path="roles/power_aix_vioshc/files/vioshc.py" pos="234:2:2" line-data="def retrieve_usr_pass(hmc_info):">`retrieve_usr_pass`</SwmToken> function retrieves user credentials from the HMC (Hardware Management Console) information. This function is crucial for accessing and managing the HMC, which is a key component in managing AIX systems.

<SwmSnippet path="/roles/power_aix_vioshc/files/vioshc.py" line="234">

---

The <SwmToken path="roles/power_aix_vioshc/files/vioshc.py" pos="234:2:2" line-data="def retrieve_usr_pass(hmc_info):">`retrieve_usr_pass`</SwmToken> function retrieves the username and password from the HMC information. This is essential for accessing and managing the HMC.

```python
def retrieve_usr_pass(hmc_info):
    """
    Retrieve the username and password

    Input: (str) hmc internet address
    Output:(str) username
    Output:(str) password
    """
    if hmc_info is None or 'type' not in hmc_info or 'passwd_file' not in hmc_info:
        write('ERROR: Failed to retrieve user ID and password for {0}'
              .format(hmc_info['hostname']), lvl=0)
        return ("", "")

    decrypt_file = get_decrypt_file(hmc_info['passwd_file'],
                                    hmc_info['type'],
                                    hmc_info['hostname'])
    if decrypt_file != '':
        (user, passwd) = get_usr_passwd(decrypt_file)
    return (user, passwd)
```

---

</SwmSnippet>

### <SwmToken path="roles/power_aix_vioshc/files/vioshc.py" pos="255:2:2" line-data="def get_nim_info(obj_name):">`get_nim_info`</SwmToken>

The <SwmToken path="roles/power_aix_vioshc/files/vioshc.py" pos="255:2:2" line-data="def get_nim_info(obj_name):">`get_nim_info`</SwmToken> function retrieves information about a NIM object. This function is essential for gathering details about NIM resources, which are used in various tasks such as migrations and installations.

<SwmSnippet path="/roles/power_aix_vioshc/files/vioshc.py" line="255">

---

The <SwmToken path="roles/power_aix_vioshc/files/vioshc.py" pos="255:2:2" line-data="def get_nim_info(obj_name):">`get_nim_info`</SwmToken> function retrieves detailed information about a specified NIM object. This is crucial for tasks involving NIM resources.

```python
def get_nim_info(obj_name):
    """
    Get detailed on the provided Network Installation Management (NIM) object

    Input: (str) NIM object name to look for
    Output:(str) hash with NIM info, the associated value can be a list
    """
    info = {}

    cmd = ['/usr/sbin/lsnim', '-l', obj_name]
    (rc, output, errout) = exec_cmd(cmd)
    if rc != 0:
        write('ERROR: Failed to get {0} NIM info: {1} {2}'.format(obj_name, output, errout), lvl=0)
        return None

    for line in output.split('\n'):
        match = re.match(r'^\s*(\S+)\s*=\s*(\S+)\s*$', line)
        if match:
            if match.group(1) not in info:
                info[match.group(1)] = match.group(2)
            elif type(info[match.group(1)]) is list:
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
