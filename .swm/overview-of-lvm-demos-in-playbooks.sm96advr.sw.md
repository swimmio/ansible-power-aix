---
title: Overview of LVM Demos in Playbooks
---
# Overview of LVM Demos in Playbooks

LVM Demos in Playbooks showcase various tasks related to Logical Volume Management (LVM) on AIX systems. These tasks include creating, extending, and removing volume groups and logical volumes.

# Usage in Playbooks

The <SwmPath>[playbooks/demo_lvg.yml](playbooks/demo_lvg.yml)</SwmPath> playbook demonstrates how to create scalable volume groups with mirror pools, add multiple physical volumes to a volume group, extend a volume group, and varyon/varyoff volume groups. The <SwmPath>[playbooks/demo_lvm_facts.yml](playbooks/demo_lvm_facts.yml)</SwmPath> playbook focuses on gathering LVM-related information, such as volume group (VG), logical volume (LV), and physical volume (PV) facts, and updating existing LVM facts. The <SwmPath>[playbooks/demo_lvol.yml](playbooks/demo_lvol.yml)</SwmPath> playbook illustrates the creation of logical volumes with various configurations, including specifying size, placement policies, and mirror pools. It also covers extending, renaming, and removing logical volumes.

## <SwmPath>[playbooks/demo_lvm_facts.yml](playbooks/demo_lvm_facts.yml)</SwmPath>

The <SwmPath>[playbooks/demo_lvm_facts.yml](playbooks/demo_lvm_facts.yml)</SwmPath> playbook focuses on gathering LVM-related information, such as volume group (VG), logical volume (LV), and physical volume (PV) facts, and updating existing LVM facts.

<SwmSnippet path="/playbooks/demo_lvm_facts.yml" line="1">

---

The <SwmPath>[playbooks/demo_lvm_facts.yml](playbooks/demo_lvm_facts.yml)</SwmPath> playbook focuses on gathering LVM-related information, such as volume group (VG), logical volume (LV), and physical volume (PV) facts, and updating existing LVM facts.

```yaml
---
- name: Print the LVM related information
  hosts: "{{ hosts_val }}"
  gather_facts: true
  vars:
    hosts_val: all
    log_file: /tmp/ansible_lvmfacts_debug.log

  tasks:
    - name: Gather all lvm facts
      ibm.power_aix.lvm_facts:
    - name: Gather VG facts
      ibm.power_aix.lvm_facts:
        name: all
        component: vg
    - name: Gather LV facts
      ibm.power_aix.lvm_facts:
        name: all
        component: lv
    - name: Update PV facts to existing LVM facts
      ibm.power_aix.lvm_facts:
```

---

</SwmSnippet>

## <SwmPath>[playbooks/demo_lvol.yml](playbooks/demo_lvol.yml)</SwmPath>

The <SwmPath>[playbooks/demo_lvol.yml](playbooks/demo_lvol.yml)</SwmPath> playbook illustrates the creation of logical volumes with various configurations, including specifying size, placement policies, and mirror pools. It also covers extending, renaming, and removing logical volumes. Tasks include creating a logical volume of a specified size, creating a logical volume with multiple disks, creating a logical volume with a minimum placement policy, creating a logical volume with extra options like mirror pools, extending a logical volume, renaming a logical volume, and removing a logical volume.

<SwmSnippet path="/playbooks/demo_lvol.yml" line="1">

---

The <SwmPath>[playbooks/demo_lvol.yml](playbooks/demo_lvol.yml)</SwmPath> playbook illustrates the creation of logical volumes with various configurations, including specifying size, placement policies, and mirror pools. It also covers extending, renaming, and removing logical volumes.

```yaml
---
- name: LVOL on AIX
  hosts: "{{ host_name }}"
  gather_facts: false
  vars:
    host_name: all
    vg_name: rootvg
    lv_name: testlv
    size_val: 64M
    increase_size_val:
  tasks:
    - name: Create a logical volume of 64M
      ibm.power_aix.lvol:
        state: present
        vg: "{{ vg_name }}"
        lv: testrootlv
        size: "{{ size_val }}"
    - name: Create a logical volume of 10 logical partitions with disks hdisk0 and hdisk1
      ibm.power_aix.lvol:
        state: present
        vg: "{{ vg_name }}"
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
