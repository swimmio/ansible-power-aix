---
title: Understanding Security Demos in Playbooks
---
# Overview of Security Demos

Security Demos showcase various security-related tasks that can be automated using Ansible modules for IBM Power Systems AIX. These demos are essential for understanding how to implement and manage security features on AIX systems using Ansible.

# How to Use Security Demos

Security Demos are used by running specific playbooks that demonstrate tasks such as managing PKS, converting volumes to encrypted states, displaying encryption status, and managing security settings. Each playbook is designed to showcase a particular aspect of security management on AIX systems.

# Examples of Security Demos

The <SwmPath>[playbooks/demo_hdcrypt_pks.yml](playbooks/demo_hdcrypt_pks.yml)</SwmPath> playbook demonstrates how to manage PKS (Public Key Security) on AIX systems. It includes tasks for adding PKS to a filesystem, displaying PKS key status, exporting and importing PKS keys, and cleaning invalid PKS keys. The <SwmPath>[playbooks/demo_hdcrypt_conv.yml](playbooks/demo_hdcrypt_conv.yml)</SwmPath> playbook focuses on converting logical volumes (<SwmToken path="playbooks/demo_hdcrypt_conv.yml" pos="24:13:13" line-data="    - name: &quot;Encrypt all the LVs present in a VG&quot;">`LVs`</SwmToken>) and physical volumes (<SwmToken path="playbooks/demo_hdcrypt_conv.yml" pos="54:10:10" line-data="    - name: Encrypt multiple PVs">`PVs`</SwmToken>) to encrypted and unencrypted states. The <SwmPath>[playbooks/demo_hdcrypt_facts.yml](playbooks/demo_hdcrypt_facts.yml)</SwmPath> playbook is used to display encryption status information. The <SwmPath>[playbooks/demo_chsec.yml](playbooks/demo_chsec.yml)</SwmPath> playbook demonstrates how to manage security settings on AIX systems.

<SwmSnippet path="/playbooks/demo_hdcrypt_pks.yml" line="1">

---

The <SwmPath>[playbooks/demo_hdcrypt_pks.yml](playbooks/demo_hdcrypt_pks.yml)</SwmPath> playbook demonstrates how to manage PKS (Public Key Security) on AIX systems. It includes tasks for adding PKS to a filesystem, displaying PKS key status, exporting and importing PKS keys, and cleaning invalid PKS keys.

```yaml
---
- name: "Demo hdcrypt PKS"
  hosts: "{{ hosts_val }}"
  gather_facts: false
  vars:
```

---

</SwmSnippet>

<SwmSnippet path="/playbooks/demo_hdcrypt_conv.yml" line="1">

---

The <SwmPath>[playbooks/demo_hdcrypt_conv.yml](playbooks/demo_hdcrypt_conv.yml)</SwmPath> playbook focuses on converting logical volumes (<SwmToken path="playbooks/demo_hdcrypt_conv.yml" pos="24:13:13" line-data="    - name: &quot;Encrypt all the LVs present in a VG&quot;">`LVs`</SwmToken>) and physical volumes (<SwmToken path="playbooks/demo_hdcrypt_conv.yml" pos="54:10:10" line-data="    - name: Encrypt multiple PVs">`PVs`</SwmToken>) to encrypted and unencrypted states. It includes tasks for encrypting and decrypting individual <SwmToken path="playbooks/demo_hdcrypt_conv.yml" pos="24:13:13" line-data="    - name: &quot;Encrypt all the LVs present in a VG&quot;">`LVs`</SwmToken>, all <SwmToken path="playbooks/demo_hdcrypt_conv.yml" pos="24:13:13" line-data="    - name: &quot;Encrypt all the LVs present in a VG&quot;">`LVs`</SwmToken> in a volume group (VG), and multiple <SwmToken path="playbooks/demo_hdcrypt_conv.yml" pos="54:10:10" line-data="    - name: Encrypt multiple PVs">`PVs`</SwmToken>.

```yaml
---
- name: "Demo hdcrypt conv"
  hosts: "{{ hosts_val }}"
  gather_facts: false
  vars:
```

---

</SwmSnippet>

<SwmSnippet path="/playbooks/demo_hdcrypt_facts.yml" line="1">

---

The <SwmPath>[playbooks/demo_hdcrypt_facts.yml](playbooks/demo_hdcrypt_facts.yml)</SwmPath> playbook is used to display encryption status information. It includes tasks for displaying the encryption status of <SwmToken path="playbooks/demo_hdcrypt_conv.yml" pos="24:13:13" line-data="    - name: &quot;Encrypt all the LVs present in a VG&quot;">`LVs`</SwmToken>, <SwmToken path="playbooks/demo_hdcrypt_facts.yml" pos="21:20:20" line-data="    - name: Display VG encryption status of all the VGs">`VGs`</SwmToken>, and <SwmToken path="playbooks/demo_hdcrypt_conv.yml" pos="54:10:10" line-data="    - name: Encrypt multiple PVs">`PVs`</SwmToken>, as well as meta facts and active or stopped conversions.

```yaml
---
- name: "Demo hdcrypt facts"
  hosts: "{{ hosts_val }}"
  gather_facts: false
  vars:
```

---

</SwmSnippet>

<SwmSnippet path="/playbooks/demo_chsec.yml" line="1">

---

The <SwmPath>[playbooks/demo_chsec.yml](playbooks/demo_chsec.yml)</SwmPath> playbook demonstrates how to manage security settings on AIX systems. It includes tasks for adding registry files for a user, changing login times, removing registry attributes, locking system user accounts, changing CPU time limits, allowing users to switch using the <SwmToken path="playbooks/demo_chsec.yml" pos="53:24:24" line-data="    - name: Allow other users to switch to this user using su command">`su`</SwmToken> command, and setting password rules.

```yaml
---
- name: CHSEC on AIX
  hosts: "{{host_name}}"
  gather_facts: false
  vars:
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
