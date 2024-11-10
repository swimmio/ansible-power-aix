---
title: MKFILT Overview
---
# MKFILT Overview

The `MKFILT` module is used to manage filter rules on AIX systems. It allows administrators to activate or deactivate filter rules, control filter logging, and monitor network traffic. The module supports actions such as adding, checking, changing, importing, and exporting filter rules.

The module requires AIX version <SwmToken path="plugins/modules/mkfilt.py" pos="26:6:8" line-data="- AIX &gt;= 7.1 TL3">`7.1`</SwmToken> <SwmToken path="plugins/modules/mkfilt.py" pos="26:10:10" line-data="- AIX &gt;= 7.1 TL3">`TL3`</SwmToken> or higher and Python <SwmToken path="plugins/modules/mkfilt.py" pos="27:6:8" line-data="- Python &gt;= 3.6">`3.6`</SwmToken> or higher. It ensures that the necessary devices, <SwmToken path="plugins/modules/mkfilt.py" pos="645:5:5" line-data="    Make sure ipsec_v4 and ipsec_v6 devices are Available.">`ipsec_v4`</SwmToken> and <SwmToken path="plugins/modules/mkfilt.py" pos="645:9:9" line-data="    Make sure ipsec_v4 and ipsec_v6 devices are Available.">`ipsec_v6`</SwmToken>, are available before performing any actions.

## Main Functions

The `MKFILT` module includes several main functions: <SwmToken path="plugins/modules/mkfilt.py" pos="296:2:2" line-data="def list_rules(module, version):">`list_rules`</SwmToken>, <SwmToken path="plugins/modules/mkfilt.py" pos="397:2:2" line-data="def add_change_rules(module, params, version):">`add_change_rules`</SwmToken>, <SwmToken path="plugins/modules/mkfilt.py" pos="609:2:2" line-data="def import_rules(module, params):">`import_rules`</SwmToken>, <SwmToken path="plugins/modules/mkfilt.py" pos="620:2:2" line-data="def export_rules(module, params):">`export_rules`</SwmToken>, <SwmToken path="plugins/modules/mkfilt.py" pos="632:2:2" line-data="def check_rules(module):">`check_rules`</SwmToken>, <SwmToken path="plugins/modules/mkfilt.py" pos="643:2:2" line-data="def make_devices(module):">`make_devices`</SwmToken>, and <SwmToken path="plugins/modules/mkfilt.py" pos="652:2:2" line-data="def main():">`main`</SwmToken>. Each function plays a specific role in managing filter rules.

<SwmSnippet path="/plugins/modules/mkfilt.py" line="296">

---

### <SwmToken path="plugins/modules/mkfilt.py" pos="296:2:2" line-data="def list_rules(module, version):">`list_rules`</SwmToken>

The <SwmToken path="plugins/modules/mkfilt.py" pos="296:2:2" line-data="def list_rules(module, version):">`list_rules`</SwmToken> function retrieves the current filter rules for either <SwmToken path="plugins/modules/mkfilt.py" pos="56:7:7" line-data="    - Specifies the IPv4 filter module state and rules.">`IPv4`</SwmToken> or <SwmToken path="plugins/modules/mkfilt.py" pos="229:7:7" line-data="    - Specifies the IPv6 filter module state and rules.">`IPv6`</SwmToken>. It runs the <SwmToken path="plugins/modules/mkfilt.py" pos="298:3:3" line-data="    Sample lsfilt output:">`lsfilt`</SwmToken> command and parses its output to create a list of filter rules.

```python
def list_rules(module, version):
    """
    Sample lsfilt output:

    1|permit|0.0.0.0|0.0.0.0|0.0.0.0|0.0.0.0|no|udp|eq|4001|eq|4001|both|both|no|all
    packets|0|all|0|||Default Rule
    2|permit|0.0.0.0|0.0.0.0|0.0.0.0|0.0.0.0|yes|all|any|0|eq|5989|both|inbound|no|all
    packets|0|all|0|||allow port 5989
    3|permit|0.0.0.0|0.0.0.0|0.0.0.0|0.0.0.0|yes|all|any|0|eq|5988|both|inbound|no|all
    packets|0|all|0|||allow port 5988
    4|permit|0.0.0.0|0.0.0.0|0.0.0.0|0.0.0.0|yes|all|any|0|eq|5987|both|inbound|no|all
    packets|0|all|0|||allow port 5987
    5|permit|0.0.0.0|0.0.0.0|0.0.0.0|0.0.0.0|yes|all|eq|657|any|0|both|inbound|no|all
    packets|0|all|0|||allow port 657
    6|permit|0.0.0.0|0.0.0.0|0.0.0.0|0.0.0.0|yes|all|any|0|eq|657|both|inbound|no|all
    packets|0|all|0|||allow port 657
    """

    vopt = '-v4' if version != 'ipv6' else '-v6'

    cmd = ['lsfilt', vopt, '-O']
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/mkfilt.py" line="397">

---

### <SwmToken path="plugins/modules/mkfilt.py" pos="397:2:2" line-data="def add_change_rules(module, params, version):">`add_change_rules`</SwmToken>

The <SwmToken path="plugins/modules/mkfilt.py" pos="397:2:2" line-data="def add_change_rules(module, params, version):">`add_change_rules`</SwmToken> function adds new filter rules or changes existing ones. It uses the <SwmToken path="plugins/modules/mkfilt.py" pos="417:7:7" line-data="            cmd = [&#39;genfilt&#39;]">`genfilt`</SwmToken> command to add rules and the <SwmToken path="plugins/modules/mkfilt.py" pos="412:7:7" line-data="            cmd = [&#39;chfilt&#39;]">`chfilt`</SwmToken> command to change rules.

```python
def add_change_rules(module, params, version):
    """
    Adds a new filter rule or changes an existing one.
    """

    vopt = '-v4' if version == 'ipv4' else '-v6'

    if not params[version]:
        return True
    if 'rules' not in params[version]:
        return True

    # Add or change rules
    for rule in params[version]['rules']:
        if params['action'] == 'change':
            cmd = ['chfilt']
            if not rule['id']:
                results['msg'] = 'Could not change rule without rule id'
                module.fail_json(**results)
        else:
            cmd = ['genfilt']
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/mkfilt.py" line="609">

---

### <SwmToken path="plugins/modules/mkfilt.py" pos="609:2:2" line-data="def import_rules(module, params):">`import_rules`</SwmToken>

The <SwmToken path="plugins/modules/mkfilt.py" pos="609:2:2" line-data="def import_rules(module, params):">`import_rules`</SwmToken> function imports filter rules from an export file using the <SwmToken path="plugins/modules/mkfilt.py" pos="614:7:7" line-data="    cmd = [&#39;impfilt&#39;, &#39;-f&#39;, params[&#39;directory&#39;]]">`impfilt`</SwmToken> command.

```python
def import_rules(module, params):
    """
    Imports filter rules from an export file.
    """

    cmd = ['impfilt', '-f', params['directory']]
    module.run_command(cmd, check_rc=True)
    results['msg'] = "Rules imported successfully."
    results['changed'] = True
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/mkfilt.py" line="620">

---

### <SwmToken path="plugins/modules/mkfilt.py" pos="620:2:2" line-data="def export_rules(module, params):">`export_rules`</SwmToken>

The <SwmToken path="plugins/modules/mkfilt.py" pos="620:2:2" line-data="def export_rules(module, params):">`export_rules`</SwmToken> function exports filter rules to an export file using the <SwmToken path="plugins/modules/mkfilt.py" pos="624:7:7" line-data="    cmd = [&#39;expfilt&#39;, &#39;-f&#39;, params[&#39;directory&#39;]]">`expfilt`</SwmToken> command.

```python
def export_rules(module, params):
    """
    Exports filter rules to an export file.
    """
    cmd = ['expfilt', '-f', params['directory']]
    if params['rawexport']:
        cmd += ['-r']
    module.run_command(cmd, check_rc=True)
    results['msg'] = "Rules exported successfully."
    results['changed'] = True
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/mkfilt.py" line="632">

---

### <SwmToken path="plugins/modules/mkfilt.py" pos="632:2:2" line-data="def check_rules(module):">`check_rules`</SwmToken>

The <SwmToken path="plugins/modules/mkfilt.py" pos="632:2:2" line-data="def check_rules(module):">`check_rules`</SwmToken> function checks the syntax of filter rules using the <SwmToken path="plugins/modules/mkfilt.py" pos="637:7:7" line-data="    cmd = [&#39;ckfilt&#39;]">`ckfilt`</SwmToken> command.

```python
def check_rules(module):
    """
    Checks the syntax of filter rules.
    """

    cmd = ['ckfilt']
    ret, stdout, stderr = module.run_command(cmd, check_rc=True)
    results['stdout'] = stdout
    results['stderr'] = stderr
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/mkfilt.py" line="643">

---

### <SwmToken path="plugins/modules/mkfilt.py" pos="643:2:2" line-data="def make_devices(module):">`make_devices`</SwmToken>

The <SwmToken path="plugins/modules/mkfilt.py" pos="643:2:2" line-data="def make_devices(module):">`make_devices`</SwmToken> function ensures that the <SwmToken path="plugins/modules/mkfilt.py" pos="645:5:5" line-data="    Make sure ipsec_v4 and ipsec_v6 devices are Available.">`ipsec_v4`</SwmToken> and <SwmToken path="plugins/modules/mkfilt.py" pos="645:9:9" line-data="    Make sure ipsec_v4 and ipsec_v6 devices are Available.">`ipsec_v6`</SwmToken> devices are available by running the <SwmToken path="plugins/modules/mkfilt.py" pos="648:7:7" line-data="        cmd = [&#39;mkdev&#39;, &#39;-l&#39;, &#39;ipsec&#39;, &#39;-t&#39;, version]">`mkdev`</SwmToken> command.

```python
def make_devices(module):
    """
    Make sure ipsec_v4 and ipsec_v6 devices are Available.
    """
    for version in ['4', '6']:
        cmd = ['mkdev', '-l', 'ipsec', '-t', version]
        module.run_command(cmd, check_rc=True)
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/mkfilt.py" line="652">

---

### main

The <SwmToken path="plugins/modules/mkfilt.py" pos="652:2:2" line-data="def main():">`main`</SwmToken> function is the entry point of the module. It sets up the module parameters, ensures necessary devices are available, and calls the appropriate function based on the specified action (add, change, import, export, check).

```python
def main():
    global results

    operations = ['lt', 'le', 'gt', 'ge', 'eq', 'neq']

    ipcommon = dict(
        type='dict',
        options=dict(
            default=dict(type='str', choices=['permit', 'deny']),
            log=dict(type='bool'),
            force=dict(type='bool', default=False),
            rules=dict(
                type='list', elements='dict',
                options=dict(
                    action=dict(type='str', choices=['permit', 'deny', 'shun_host',
                                                     'shun_port', 'if', 'else', 'endif', 'remove', 'move']),
                    id=dict(type='str'),
                    new_id=dict(type='str'),
                    direction=dict(type='str', choices=['inbound', 'outbound', 'both'], default='both'),
                    s_addr=dict(type='str'),
                    s_mask=dict(type='str'),
```

---

</SwmSnippet>

## Example Usage

This example demonstrates how to allow SSH activity through interface <SwmToken path="plugins/modules/mkfilt.py" pos="238:15:15" line-data="- name: Allow SSH activity through interface en0">`en0`</SwmToken> using the <SwmToken path="plugins/modules/mkfilt.py" pos="239:1:1" line-data="  mkfilt:">`mkfilt`</SwmToken> module. It specifies the <SwmToken path="plugins/modules/mkfilt.py" pos="56:7:7" line-data="    - Specifies the IPv4 filter module state and rules.">`IPv4`</SwmToken> filter module state and rules, enabling logging, setting the default action to deny, and defining rules to permit inbound and outbound SSH traffic.

<SwmSnippet path="/plugins/modules/mkfilt.py" line="237">

---

Example usage of the <SwmToken path="plugins/modules/mkfilt.py" pos="239:1:1" line-data="  mkfilt:">`mkfilt`</SwmToken> module to allow SSH activity through interface <SwmToken path="plugins/modules/mkfilt.py" pos="238:15:15" line-data="- name: Allow SSH activity through interface en0">`en0`</SwmToken>.

```python
EXAMPLES = r'''
- name: Allow SSH activity through interface en0
  mkfilt:
    ipv4:
      log: true
      default: deny
      rules:
        - action: permit
          direction: inbound
          d_opr: eq
          d_port: 22
          interface: en0
          description: permit SSH requests from any clients
        - action: permit
          direction: outbound
          s_opr: eq
          s_port: 22
          interface: en0
          description: permit SSH answers to any clients

- name: Remove all user-defined and auto-generated filter rules
```

---

</SwmSnippet>

&nbsp;

*This is an* <SwmToken path="plugins/modules/mkfilt.py" pos="257:15:17" line-data="- name: Remove all user-defined and auto-generated filter rules">`auto-generated`</SwmToken> *document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
