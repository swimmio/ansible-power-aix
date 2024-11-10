---
title: NIM Resource Overview
---
# NIM Resource Overview

NIM Resource refers to various types of resources managed by the Network Installation Manager (NIM) on AIX systems. These resources include 'spot', <SwmToken path="plugins/modules/nim_resource.py" pos="64:9:9" line-data="    - supports spot and lpp_source.">`lpp_source`</SwmToken>, 'script', <SwmToken path="plugins/modules/nim_resource.py" pos="53:28:28" line-data="    - The following are examples of the most common choices &quot;pp_source, spot, bosinst_data, mksysb, fb_script and  res_group&quot;">`bosinst_data`</SwmToken>, <SwmToken path="plugins/modules/nim_resource.py" pos="214:2:2" line-data="    &#39;image_data&#39;,">`image_data`</SwmToken>, <SwmToken path="plugins/modules/nim_resource.py" pos="215:2:2" line-data="    &#39;installp_bundle&#39;,">`installp_bundle`</SwmToken>, <SwmToken path="plugins/modules/nim_resource.py" pos="216:2:2" line-data="    &#39;fix_bundle&#39;,">`fix_bundle`</SwmToken>, <SwmToken path="plugins/modules/nim_resource.py" pos="217:2:2" line-data="    &#39;resolv_conf&#39;,">`resolv_conf`</SwmToken>, <SwmToken path="plugins/modules/nim_resource.py" pos="218:2:2" line-data="    &#39;exclude_files&#39;,">`exclude_files`</SwmToken>, and <SwmToken path="plugins/modules/nim_resource.py" pos="219:2:2" line-data="    &#39;adapter_def&#39;,">`adapter_def`</SwmToken>. The module facilitates the display, creation, and deletion of these NIM resource objects.

## Displaying NIM Resources

The <SwmToken path="plugins/modules/nim_resource.py" pos="235:2:2" line-data="def res_show(module):">`res_show`</SwmToken> function is used to display NIM resources, fetching general information about the resource object class.

<SwmSnippet path="/plugins/modules/nim_resource.py" line="235">

---

The <SwmToken path="plugins/modules/nim_resource.py" pos="235:2:2" line-data="def res_show(module):">`res_show`</SwmToken> function displays NIM resources by fetching general information about the resource object class. It constructs a command to list NIM resources and executes it.

```python
def res_show(module):
    '''
    Show nim resources.

    arguments:
        module  (dict): The Ansible module
    note:
        Exits with fail_json in case of error
    return:
        updated results dictionary.
    '''

    cmd = '/usr/sbin/lsnim -l'
    name = module.params['name']
    object_type = module.params['object_type']

    # This module will only show general information about the resource
    # object class.
    if not object_type and not name:
        cmd += ' -c resources'
```

---

</SwmSnippet>

## Creating NIM Resources

The <SwmToken path="plugins/modules/nim_resource.py" pos="301:2:2" line-data="def res_create(nim_cmd, module):">`res_create`</SwmToken> function defines a new NIM resource object, requiring parameters such as name, object type, and attributes.

<SwmSnippet path="/plugins/modules/nim_resource.py" line="301">

---

The <SwmToken path="plugins/modules/nim_resource.py" pos="301:2:2" line-data="def res_create(nim_cmd, module):">`res_create`</SwmToken> function defines a new NIM resource object. It constructs a command to create the resource based on the provided parameters and executes it.

```python
def res_create(nim_cmd, module):
    '''
    Define a NIM resource object.

    arguments:
        module  (dict): The Ansible module
    note:
        Exits with fail_json in case of error
    return:
        updated results dictionary.
    '''

    opts = ""
    name = module.params['name']
    object_type = module.params['object_type']
    attributes = module.params['attributes']

    if object_type:
        if object_type == "res_group":
            cmd = nim_cmd + ' -o define '
        else:
```

---

</SwmSnippet>

## Deleting NIM Resources

The <SwmToken path="plugins/modules/nim_resource.py" pos="363:2:2" line-data="def res_delete(nim_cmd, module):">`res_delete`</SwmToken> function removes a specified NIM resource object.

<SwmSnippet path="/plugins/modules/nim_resource.py" line="363">

---

The <SwmToken path="plugins/modules/nim_resource.py" pos="363:2:2" line-data="def res_delete(nim_cmd, module):">`res_delete`</SwmToken> function removes a specified NIM resource object. It constructs a command to delete the resource and executes it.

```python
def res_delete(nim_cmd, module):
    '''
    Remove a NIM resource object.

    arguments:
        module  (dict): The Ansible module
    note:
        Exits with fail_json in case of error
    return:
        updated results dictionary.
    '''

    name = module.params['name']
    cmd = nim_cmd + f' -o remove {name}'

    if module.check_mode:
        results['msg'] = f'Command \'{cmd}\' in preview mode, execution skipped.'
        return

    return_code, stdout, stderr = module.run_command(cmd)
```

---

</SwmSnippet>

## Fetching NIM Resource Contents

The <SwmToken path="plugins/modules/nim_resource.py" pos="440:2:2" line-data="def res_showres(module, resource, info):">`res_showres`</SwmToken> function fetches the contents of valid NIM object resources, specifically for 'spot' and <SwmToken path="plugins/modules/nim_resource.py" pos="64:9:9" line-data="    - supports spot and lpp_source.">`lpp_source`</SwmToken> types.

<SwmSnippet path="/plugins/modules/nim_resource.py" line="440">

---

The <SwmToken path="plugins/modules/nim_resource.py" pos="440:2:2" line-data="def res_showres(module, resource, info):">`res_showres`</SwmToken> function fetches the contents of valid NIM object resources. It constructs a command to retrieve the resource contents and executes it.

```python
def res_showres(module, resource, info):
    """
    Fetch contents of valid NIM object resource

    arguments:
        module  (dict): The Ansible module
        resource (str): NIM resource name
        info    (info): NIM resource attributes information
    return:
        contents: (dict): NIM resource contents
    """
    fail_msg = f'Unable to fetch contents of {resource}.'
    max_retries = module.params['showres']['max_retries']
    retry_wait_time = module.params['showres']['max_retries']
    contents = {}
    results['testing'] = ""
    # 0042-001 nim: processing error encountered on "master":,
    #     0042-207 m_showres: Unable to allocate the spot_72V_2114 resource to master.
    pattern = r"0042-207"

    if info['type'] in NIM_SHOWRES:
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
