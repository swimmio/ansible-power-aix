---
title: Overview of Dynamic Playbooks
---
# Overview of Dynamic Playbooks

Dynamic Playbooks refer to playbooks that are designed to handle various configurations and tasks dynamically based on the variables and conditions defined within them. They leverage Ansible's templating and conditional capabilities to execute tasks that can change based on the input parameters or the state of the system.

## Dynamic Playbooks in <SwmPath>[playbooks/dynamic_mktun.yml](playbooks/dynamic_mktun.yml)</SwmPath>

The playbook <SwmPath>[playbooks/dynamic_mktun.yml](playbooks/dynamic_mktun.yml)</SwmPath> demonstrates the use of dynamic playbooks to create, activate, export, import, and remove <SwmToken path="playbooks/dynamic_mktun.yml" pos="17:16:16" line-data="    - name: Create and activate a manual IPv4 tunnel">`IPv4`</SwmToken> tunnels based on the variables provided. It uses the <SwmToken path="playbooks/dynamic_mktun.yml" pos="18:1:5" line-data="      ibm.power_aix.mktun:">`ibm.power_aix.mktun`</SwmToken> module to manage these tunnels dynamically.

<SwmSnippet path="/playbooks/dynamic_mktun.yml" line="1">

---

This snippet from <SwmPath>[playbooks/dynamic_mktun.yml](playbooks/dynamic_mktun.yml)</SwmPath> shows how to create and activate a manual <SwmToken path="playbooks/dynamic_mktun.yml" pos="17:16:16" line-data="    - name: Create and activate a manual IPv4 tunnel">`IPv4`</SwmToken> tunnel using the <SwmToken path="playbooks/dynamic_mktun.yml" pos="18:1:5" line-data="      ibm.power_aix.mktun:">`ibm.power_aix.mktun`</SwmToken> module.

```yaml
---
- name: Demo for mktun module
  hosts: all
  gather_facts: true
  vars:
    log_file: /tmp/ansible_mktun_debug.log
    ip_v4_address_src: 10.10.11.72
    ip_v4_address_dest: 10.10.11.98
    var_ah_algo: HMAC_MD5
    var_esp_algo: DES_CBC_8
    var_esp_spi: 12345
    var_id_1: 3
    var_id_2: 4
    var_id_3: 5

  tasks:
    - name: Create and activate a manual IPv4 tunnel
      ibm.power_aix.mktun:
        manual:
          ipv4:
            - src:
```

---

</SwmSnippet>

## Dynamic Playbooks in <SwmPath>[playbooks/dynamic_tunables.yml](playbooks/dynamic_tunables.yml)</SwmPath>

The playbook <SwmPath>[playbooks/dynamic_tunables.yml](playbooks/dynamic_tunables.yml)</SwmPath> demonstrates the use of dynamic playbooks to display, modify, and reset tunable parameters for AIX systems. It uses the <SwmToken path="playbooks/dynamic_tunables.yml" pos="16:1:5" line-data="      ibm.power_aix.tunables:">`ibm.power_aix.tunables`</SwmToken> module to manage these parameters dynamically.

<SwmSnippet path="/playbooks/dynamic_tunables.yml" line="1">

---

This snippet from <SwmPath>[playbooks/dynamic_tunables.yml](playbooks/dynamic_tunables.yml)</SwmPath> shows how to display VM tunable parameters using the <SwmToken path="playbooks/dynamic_tunables.yml" pos="16:1:5" line-data="      ibm.power_aix.tunables:">`ibm.power_aix.tunables`</SwmToken> module.

```yaml
---
- name: "Display vm tunables parameters"
  hosts: all
  gather_facts: false
  vars:
    component_name: "vmo"
    var_tunable_params: ["vm_mmap_bmap", "vmm_default_pspa"]
    var_tunable_params_dict: {"lgpg_regions": 10, "lgpg_size": 16777216}
    var_change_type: "reboot"
    bool_restricted_tunables: true
    bool_bosboot_tunables: true
    log_file: "/tmp/ansible_tunables_debug.log"

  tasks:
    - name: "Show all tunable parameters excluding restricted parameters"
      ibm.power_aix.tunables:
        action: show
        component: "{{ component_name }}"
      register: tunables_result

    - name: "Debug: tunables_result"
```

---

</SwmSnippet>

## Dynamic Playbooks in <SwmPath>[playbooks/dynamic_tunfile_mgmt.yml](playbooks/dynamic_tunfile_mgmt.yml)</SwmPath>

The <SwmPath>[playbooks/dynamic_tunfile_mgmt.yml](playbooks/dynamic_tunfile_mgmt.yml)</SwmPath> playbook showcases how to manage tunable files dynamically, including saving, validating, modifying, and restoring tunable files. It uses the <SwmToken path="playbooks/dynamic_tunfile_mgmt.yml" pos="17:1:5" line-data="      ibm.power_aix.tunfile_mgmt:">`ibm.power_aix.tunfile_mgmt`</SwmToken> module for these operations.

<SwmSnippet path="/playbooks/dynamic_tunfile_mgmt.yml" line="1">

---

This snippet from <SwmPath>[playbooks/dynamic_tunfile_mgmt.yml](playbooks/dynamic_tunfile_mgmt.yml)</SwmPath> shows how to save all tunables to a file using the <SwmToken path="playbooks/dynamic_tunfile_mgmt.yml" pos="17:1:5" line-data="      ibm.power_aix.tunfile_mgmt:">`ibm.power_aix.tunfile_mgmt`</SwmToken> module.

```yaml
---
- name: Tunfile manager for AIX
  hosts: all
  gather_facts: false
  vars:
    var_set_default: true
    var_filename: "/tunfile_mgmt_test"
    component_name: "schedo"
    bool_save_all_tunables: false
    var_validation_type: "reboot"
    var_tunables_with_values: {"nfso": {"client_delegation": 1, "nfs_rfc1323": 1}}
    bool_make_nextboot: true
    log_file: "/tmp/ansible_tunfile_debug.log"

  tasks:
    - name: "Save all tunables to a file"
      ibm.power_aix.tunfile_mgmt:
        action: save
        filename: "{{ var_filename }}"
      register: tunfile_result
    - ansible.builtin.debug:
```

---

</SwmSnippet>

## Dynamic Playbooks in <SwmPath>[playbooks/dynamic_mpio.yml](playbooks/dynamic_mpio.yml)</SwmPath>

In the <SwmPath>[playbooks/dynamic_mpio.yml](playbooks/dynamic_mpio.yml)</SwmPath> playbook, dynamic playbooks are used to gather and print MPIO (Multipath I/O) related information for specific devices. It uses the <SwmToken path="playbooks/dynamic_mpio.yml" pos="12:1:5" line-data="      ibm.power_aix.mpio:">`ibm.power_aix.mpio`</SwmToken> module to collect this information.

<SwmSnippet path="/playbooks/dynamic_mpio.yml" line="1">

---

This snippet from <SwmPath>[playbooks/dynamic_mpio.yml](playbooks/dynamic_mpio.yml)</SwmPath> shows how to gather and print MPIO related information using the <SwmToken path="playbooks/dynamic_mpio.yml" pos="12:1:5" line-data="      ibm.power_aix.mpio:">`ibm.power_aix.mpio`</SwmToken> module.

```yaml
---
- name: Print the mpio related information
  hosts: all
  gather_facts: true
  vars:
    device: IBMSVC
    absent_device: ansibleNegativeTest
    log_file: /tmp/ansible_mpio_debug.log

  tasks:
    - name: Gather the mpio info
      ibm.power_aix.mpio:

    - name: Gather specific device  mpio info
      ibm.power_aix.mpio:
        device: "{{ device }}"

    - name: Gather specific absent device  mpio info
      ibm.power_aix.mpio:
        device: "{{ absent_device }}"
```

---

</SwmSnippet>

## Main Functions

There are several main functions in this folder. Some of them are creating, activating, exporting, importing, and removing <SwmToken path="playbooks/dynamic_mktun.yml" pos="17:16:16" line-data="    - name: Create and activate a manual IPv4 tunnel">`IPv4`</SwmToken> tunnels, displaying, modifying, resetting tunable parameters, managing tunable files, and gathering and printing MPIO related information. We will dive a little into each of these functions.

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
