---
title: HDCrypt PKS Module Overview
---
# Overview

HDCrypt PKS refers to the Platform Key Store (PKS) management functionality provided by the <SwmToken path="plugins/modules/hdcrypt_pks.py" pos="22:14:14" line-data="- This module is a wrapper around hdcryptmgr command.">`hdcryptmgr`</SwmToken> command. This module allows for the addition of PKS as an authentication method to a device and manages the PKS keys.

# Supported Actions

The module supports several actions:

- Adding PKS authentication (<SwmToken path="plugins/modules/hdcrypt_pks.py" pos="76:4:4" line-data="        action: addpks">`addpks`</SwmToken>)
- Displaying PKS keys and their status (<SwmToken path="plugins/modules/hdcrypt_pks.py" pos="82:4:4" line-data="        action: show">`show`</SwmToken>)
- Exporting PKS keys to a file (<SwmToken path="plugins/modules/hdcrypt_pks.py" pos="86:4:4" line-data="        action: export">`export`</SwmToken>)
- Importing PKS keys from a file (`import`)
- Cleaning invalid PKS keys (<SwmToken path="plugins/modules/hdcrypt_pks.py" pos="35:3:3" line-data="      C(clean) removes an invalid key from the PKS;">`clean`</SwmToken>)

# Module Functionality

The <SwmToken path="plugins/modules/hdcrypt_pks.py" pos="75:5:5" line-data="    ibm.power_aix.hdcrypt_pks:">`hdcrypt_pks`</SwmToken> module is a wrapper around the <SwmToken path="plugins/modules/hdcrypt_pks.py" pos="22:14:14" line-data="- This module is a wrapper around hdcryptmgr command.">`hdcryptmgr`</SwmToken> command, which is used to manage PKS keys on AIX systems. It ensures that the PKS is enabled on the system before performing any actions.

# Usage Examples

The <SwmToken path="plugins/modules/hdcrypt_pks.py" pos="73:0:0" line-data="EXAMPLES = r&#39;&#39;&#39;">`EXAMPLES`</SwmToken> constant in the <SwmPath>[plugins/modules/hdcrypt_pks.py](plugins/modules/hdcrypt_pks.py)</SwmPath> file provides usage examples for adding PKS to a filesystem, displaying PKS keys status, exporting PKS keys to a file, importing PKS keys from a file, and cleaning invalid PKS keys.

<SwmSnippet path="/plugins/modules/hdcrypt_pks.py" line="71">

---

The <SwmToken path="plugins/modules/hdcrypt_pks.py" pos="73:0:0" line-data="EXAMPLES = r&#39;&#39;&#39;">`EXAMPLES`</SwmToken> constant in the <SwmPath>[plugins/modules/hdcrypt_pks.py](plugins/modules/hdcrypt_pks.py)</SwmPath> file provides usage examples for adding PKS to a filesystem, displaying PKS keys status, exporting PKS keys to a file, importing PKS keys from a file, and cleaning invalid PKS keys.

```python
'''

EXAMPLES = r'''
- name: Add PKS to filesystem
    ibm.power_aix.hdcrypt_pks:
        action: addpks
        device: testlv1
        method_name: initpks

- name: Display PKS keys status
    ibm.power_aix.hdcrypt_pks:
        action: show

- name: Export PKS key to a file
    ibm.power_aix.hdcrypt_pks:
        action: export
        device: testlv1
        location: /tmp/file123
        passphrase: abc1234
    no_log: True
```

---

</SwmSnippet>

<SwmSnippet path="/playbooks/demo_hdcrypt_pks.yml" line="16">

---

The <SwmToken path="playbooks/demo_hdcrypt_pks.yml" pos="17:4:4" line-data="        action: addpks">`addpks`</SwmToken> action is used in the <SwmPath>[playbooks/demo_hdcrypt_pks.yml](playbooks/demo_hdcrypt_pks.yml)</SwmPath> playbook to add PKS authentication to a device.

```yaml
      ibm.power_aix.hdcrypt_pks:
        action: addpks
        device: "{{ lv_val }}"
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
