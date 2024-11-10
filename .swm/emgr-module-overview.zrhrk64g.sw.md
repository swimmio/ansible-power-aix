---
title: EMGR Module Overview
---
# EMGR Overview

EMGR refers to the interim fix manager used to manage system interim fixes on AIX systems. It installs packages created with the <SwmToken path="plugins/modules/emgr.py" pos="189:19:19" line-data="- name: Install ifix package from file generated with epkg">`epkg`</SwmToken> command and maintains the database containing interim fix information. EMGR can perform various operations such as installing, committing, checking, mounting, unmounting, removing, listing interim fixes, and viewing package locks.

# Supported Actions

The module supports actions like install, commit, check, mount, unmount, remove, <SwmToken path="plugins/modules/emgr.py" pos="386:7:7" line-data="                                                             &#39;remove&#39;, &#39;view_package&#39;, &#39;display_ifix&#39;, &#39;list&#39;]),">`view_package`</SwmToken>, <SwmToken path="plugins/modules/emgr.py" pos="386:12:12" line-data="                                                             &#39;remove&#39;, &#39;view_package&#39;, &#39;display_ifix&#39;, &#39;list&#39;]),">`display_ifix`</SwmToken>, and list. It requires AIX version <SwmToken path="plugins/modules/emgr.py" pos="30:6:8" line-data="- AIX &gt;= 7.1 TL3">`7.1`</SwmToken> <SwmToken path="plugins/modules/emgr.py" pos="30:10:10" line-data="- AIX &gt;= 7.1 TL3">`TL3`</SwmToken> or higher and Python <SwmToken path="plugins/modules/emgr.py" pos="31:6:8" line-data="- Python &gt;= 3.6">`3.6`</SwmToken> or newer. The module also requires a privileged user with the authorization <SwmToken path="plugins/modules/emgr.py" pos="32:14:18" line-data="- &#39;Privileged user with authorization: B(aix.system.install)&#39;">`aix.system.install`</SwmToken>.

# How to Use EMGR

To use EMGR, specify the action to be performed (e.g., install, commit, check) and provide the necessary parameters such as <SwmToken path="plugins/modules/emgr.py" pos="192:1:1" line-data="    ifix_package: /usr/sys/inst.images/IJ22714s1a.200212.AIX72TL04SP00-01.epkg.Z">`ifix_package`</SwmToken>, <SwmToken path="plugins/modules/emgr.py" pos="367:1:1" line-data="    ifix_label = ifix_package.split(&#39;/&#39;)[-1].split(&#39;.&#39;)[0]">`ifix_label`</SwmToken>, or <SwmToken path="plugins/modules/emgr.py" pos="393:1:1" line-data="            list_file=dict(type=&#39;path&#39;),">`list_file`</SwmToken>. The module supports various options to control the behavior of the EMGR command.

<SwmSnippet path="/plugins/modules/emgr.py" line="189">

---

# Example Usage of EMGR

An example usage of the EMGR module is to install an ifix package from a file generated with <SwmToken path="plugins/modules/emgr.py" pos="189:19:19" line-data="- name: Install ifix package from file generated with epkg">`epkg`</SwmToken>. This can be done by specifying the action as 'install' and providing the path to the <SwmToken path="plugins/modules/emgr.py" pos="192:1:1" line-data="    ifix_package: /usr/sys/inst.images/IJ22714s1a.200212.AIX72TL04SP00-01.epkg.Z">`ifix_package`</SwmToken>, the working directory, and setting <SwmToken path="plugins/modules/emgr.py" pos="194:1:1" line-data="    from_epkg: true">`from_epkg`</SwmToken> to true.

```python
- name: Install ifix package from file generated with epkg
  emgr:
    action: install
    ifix_package: /usr/sys/inst.images/IJ22714s1a.200212.AIX72TL04SP00-01.epkg.Z
    working_dir: /usr/sys/inst.images
    from_epkg: true
    extend_fs: true
```

---

</SwmSnippet>

# Main Functions

The EMGR module includes several main functions that facilitate its operations.

<SwmSnippet path="/plugins/modules/emgr.py" line="276">

---

## <SwmToken path="plugins/modules/emgr.py" pos="276:2:2" line-data="def param_one_of(one_of_list, required=True, exclusive=True):">`param_one_of`</SwmToken>

The <SwmToken path="plugins/modules/emgr.py" pos="276:2:2" line-data="def param_one_of(one_of_list, required=True, exclusive=True):">`param_one_of`</SwmToken> function checks if at least one parameter from a given list is defined in the module's parameters dictionary. It ensures that the required parameters are provided and that only one parameter is defined if exclusivity is required.

```python
def param_one_of(one_of_list, required=True, exclusive=True):
    """
    Check at parameter of one_of_list is defined in module.params dictionary.

    arguments:
        one_of_list (list) list of parameter to check
        required    (bool) at least one parameter has to be defined.
        exclusive   (bool) only one parameter can be defined.
    note:
        Ansible might have this embedded in some version: require_if 4th parameter.
        Exits with fail_json in case of error
    """

    count = 0
    action = module.params['action']
    for param in one_of_list:
        if module.params[param] is not None and module.params[param]:
            count += 1
            break
    if count == 0 and required:
        results['msg'] = f'Missing parameter: action is { action } but\
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/emgr.py" line="307">

---

## <SwmToken path="plugins/modules/emgr.py" pos="307:2:2" line-data="def parse_ifix_details(output):">`parse_ifix_details`</SwmToken>

The <SwmToken path="plugins/modules/emgr.py" pos="307:2:2" line-data="def parse_ifix_details(output):">`parse_ifix_details`</SwmToken> function parses the output of the <SwmToken path="plugins/modules/emgr.py" pos="309:10:13" line-data="    Parses the output of &quot;emgr -l&quot; command to return the ifixes as a list instead of str.">`emgr -l`</SwmToken> command to return the interim fixes as a list of dictionaries. It extracts relevant information such as ID, state, label, install time, updated by, and abstract from the command output.

```python
def parse_ifix_details(output):
    """
    Parses the output of "emgr -l" command to return the ifixes as a list instead of str.

    argument:
      stdout (str) standard output of the command.

    return:
      List of dictionaries containing information about the iFixes.
    """
    ifix_name = []

    info_list = ["ID", "STATE", "LABEL", "INSTALL TIME", "UPDATED BY", "ABSTRACT"]
    position_dict = {}

    def get_value(line, field):
        field_index = info_list.index(field)
        next_field_index = field_index + 1
        start_position = position_dict[field]

        # End-of-line
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/emgr.py" line="364">

---

## <SwmToken path="plugins/modules/emgr.py" pos="364:2:2" line-data="def is_ifix_installed(module, ifix_package):">`is_ifix_installed`</SwmToken>

The <SwmToken path="plugins/modules/emgr.py" pos="364:2:2" line-data="def is_ifix_installed(module, ifix_package):">`is_ifix_installed`</SwmToken> function checks if a specified interim fix is already installed on the system. It uses the <SwmToken path="plugins/modules/emgr.py" pos="368:6:12" line-data="    cmd = &#39;emgr -c -L&#39; + ifix_label">`emgr -c -L`</SwmToken> command to verify the installation status of the interim fix.

```python
def is_ifix_installed(module, ifix_package):

    # Utility function to check if the ifix is installed in the system.
    ifix_label = ifix_package.split('/')[-1].split('.')[0]
    cmd = 'emgr -c -L' + ifix_label

    rc = module.run_command(cmd)[0]

    if rc == 0:
        return True
    else:
        return False
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/emgr.py" line="378">

---

## main

The <SwmToken path="plugins/modules/emgr.py" pos="378:2:2" line-data="def main():">`main`</SwmToken> function is the entry point of the module. It defines the module's parameters, initializes the results dictionary, and handles the different actions that can be performed by the <SwmToken path="plugins/modules/emgr.py" pos="190:1:1" line-data="  emgr:">`emgr`</SwmToken> command, such as install, commit, check, mount, unmount, remove, <SwmToken path="plugins/modules/emgr.py" pos="386:7:7" line-data="                                                             &#39;remove&#39;, &#39;view_package&#39;, &#39;display_ifix&#39;, &#39;list&#39;]),">`view_package`</SwmToken>, <SwmToken path="plugins/modules/emgr.py" pos="386:12:12" line-data="                                                             &#39;remove&#39;, &#39;view_package&#39;, &#39;display_ifix&#39;, &#39;list&#39;]),">`display_ifix`</SwmToken>, and list.

```python
def main():
    global module
    global results

    module = AnsibleModule(
        supports_check_mode=True,
        argument_spec=dict(
            action=dict(type='str', default='list', choices=['install', 'commit', 'check', 'mount', 'unmount',
                                                             'remove', 'view_package', 'display_ifix', 'list']),
            ifix_package=dict(type='path'),
            ifix_label=dict(type='str'),
            ifix_number=dict(type='str'),
            ifix_vuid=dict(type='str'),
            package=dict(type='str'),
            alternate_dir=dict(type='path'),
            list_file=dict(type='path'),
            working_dir=dict(type='path'),
            from_epkg=dict(type='bool', default=False),
            mount_install=dict(type='bool', default=False),
            commit=dict(type='bool', default=False),
            extend_fs=dict(type='bool', default=False),
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
