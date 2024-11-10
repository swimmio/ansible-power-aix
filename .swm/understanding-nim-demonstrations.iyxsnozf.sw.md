---
title: Understanding NIM Demonstrations
---
# Overview

NIM Demos are playbooks designed to demonstrate various Network Installation Management (NIM) operations on AIX systems. These playbooks cover a wide range of tasks such as updating LPARs, deallocating resources, applying customization scripts, and performing maintenance operations. The playbooks also include operations for creating, deleting, or showing NIM resource objects, checking the status of NIM operations, and performing FLRTVC (Fix Level Recommendation Tool for Virtual Clients) checks. Additionally, there are playbooks for backing up and restoring systems, migrating NIM masters, installing BOS (Base Operating System) using mksysb images, upgrading VIOS (Virtual I/O Server), and updating VIOS through the NIM master.

# Usage of NIM Demos

NIM Demos are used by running the provided playbooks to perform tasks such as updating LPARs, deallocating resources, applying customization scripts, and performing maintenance operations. These playbooks are essential for managing and automating various NIM operations on AIX systems.

# Examples of NIM Demos

NIM Demos are used in various playbooks such as <SwmPath>[playbooks/demo_nim.yml](playbooks/demo_nim.yml)</SwmPath>, <SwmPath>[playbooks/demo_nim_resource.yml](playbooks/demo_nim_resource.yml)</SwmPath>, <SwmPath>[playbooks/demo_nim_check.yml](playbooks/demo_nim_check.yml)</SwmPath>, <SwmPath>[playbooks/demo_nim_flrtvc.yml](playbooks/demo_nim_flrtvc.yml)</SwmPath>, <SwmPath>[playbooks/demo_nim_suma.yml](playbooks/demo_nim_suma.yml)</SwmPath>, <SwmPath>[playbooks/demo_nim_master_migration.yml](playbooks/demo_nim_master_migration.yml)</SwmPath>, <SwmPath>[playbooks/demo_nim_install.yml](playbooks/demo_nim_install.yml)</SwmPath>, <SwmPath>[playbooks/demo_nim_backup.yml](playbooks/demo_nim_backup.yml)</SwmPath>, <SwmPath>[playbooks/demo_nim_viosupgrade.yml](playbooks/demo_nim_viosupgrade.yml)</SwmPath>, and <SwmPath>[playbooks/demo_nim_updateios.yml](playbooks/demo_nim_updateios.yml)</SwmPath>.

<SwmSnippet path="/playbooks/demo_nim.yml" line="1">

---

The playbook <SwmPath>[playbooks/demo_nim.yml](playbooks/demo_nim.yml)</SwmPath> demonstrates various NIM operations on <SwmToken path="playbooks/demo_nim.yml" pos="2:11:13" line-data="- name: NIM operation on AIX/VIOS">`AIX/VIOS`</SwmToken>. It includes tasks such as updating LPARs, deallocating resources, and applying customization scripts.

```yaml
---
- name: NIM operation on AIX/VIOS
  hosts: nimserver2
  remote_user: root
  gather_facts: false
  vars:
    check_targets_v: standalone
    install_targets_v: quimby06
    update_lpp_v: latest_sp
    bos_inst_resgroup_v: basic_res_grp
    bos_inst_script_v: setup_root
    master_setup_device_v: /dev/cd0
    define_resource_v: setup_root
    define_location_v: /export/nim/script_res/yum_install.sh
    apply_script_v: setup_yum
    alloc_resource_v: 7200-03-04-1938-lpp_source
    rm_resource_v: quimby06_svg

  tasks:
    - name: Update a LPAR to the latest level available
      ibm.power_aix.nim:
```

---

</SwmSnippet>

## NIM operations to create, delete or show NIM resource objects

This playbook demonstrates operations for creating, deleting, or showing NIM resource objects.

<SwmSnippet path="/playbooks/demo_nim_resource.yml" line="1">

---

The playbook <SwmPath>[playbooks/demo_nim_resource.yml](playbooks/demo_nim_resource.yml)</SwmPath> demonstrates operations for creating, deleting, or showing NIM resource objects.

```yaml
---
- name: NIM operations to create, delete or show NIM resource objects
  hosts: nim1
  gather_facts: false
  vars:
    spot_name_v: spot_test4
    spot_final_location_v: /nim1/spot_test4_from_lpp_source
    lpp_source_v: lpp_source_test4
    software_lpp_source: /nim1/lpp_source
    lpp_source_location: /nim1/lpp_source_test4

  tasks:
    - name: Create/define an lpp_source NIM resource object from the software in the system.
      ibm.power_aix.nim_resource:
        action: create
        name: "{{ lpp_source_v }}"
        object_type: lpp_source
        attributes:
          source: "{{ software_lpp_source }}"
          location: "{{ lpp_source_location }}"
      register: result
```

---

</SwmSnippet>

## NIM FLRTVC on AIX playbook

This playbook demonstrates how to perform FLRTVC (Fix Level Recommendation Tool for Virtual Clients) checks on AIX.

<SwmSnippet path="/playbooks/demo_nim_flrtvc.yml" line="1">

---

The playbook <SwmPath>[playbooks/demo_nim_flrtvc.yml](playbooks/demo_nim_flrtvc.yml)</SwmPath> demonstrates how to perform FLRTVC checks on AIX.

```yaml
---
- name: NIM FLRTVC on AIX playbook
  hosts: nimserver
  gather_facts: false
  vars:
    apar: all
    vm_targets: quimby01
    path: /export/nim/ansible
    download: false

  tasks:

    - name: FLRTVC
      ibm.power_aix.nim_flrtvc:
        targets: "{{ vm_targets }}"
        path: "{{ path }}"
        verbose: true
        apar: "{{ apar }}"
        force: false
        clean: false
        download_only: "{{ download }}"
```

---

</SwmSnippet>

## NIM check on AIX playbook

This playbook demonstrates how to check the status of NIM operations on AIX.

<SwmSnippet path="/playbooks/demo_nim_check.yml" line="1">

---

The playbook <SwmPath>[playbooks/demo_nim_check.yml](playbooks/demo_nim_check.yml)</SwmPath> demonstrates how to check the status of NIM operations on AIX.

```yaml
---
- name: NIM check on AIX playbook
  hosts: nimserver
  gather_facts: false

  tasks:
    - name: AIX NIM
      nim:
        action: check
      register: result
    - ansible.builtin.debug:
        var: result
        cmd: ""
```

---

</SwmSnippet>

## <SwmToken path="playbooks/demo_nim_master_migration.yml" pos="4:6:6" line-data="- name: &quot;Nim_master_migration demo&quot;">`Nim_master_migration`</SwmToken> demo

This playbook demonstrates how to migrate NIM masters.

<SwmSnippet path="/playbooks/demo_nim_master_migration.yml" line="1">

---

The playbook <SwmPath>[playbooks/demo_nim_master_migration.yml](playbooks/demo_nim_master_migration.yml)</SwmPath> demonstrates how to migrate NIM masters.

```yaml
---
# PREREQUISITES:

- name: "Nim_master_migration demo"
  hosts: "{{ hosts_val }}"
  gather_facts: false
  vars:
    hosts_val: all
    log_file: "/tmp/ansible_mpio_debug.log"
  remote_user: root
  tasks:
    - ansible.builtin.import_role:
        name: nim_master_migration
      vars:
        nim_master_migration_master_a: ansible-nim1
        nim_master_migration_master_b: "p9zpa-ansible-test2"
        nim_master_migration_alt_disk: "hdisk1"
        nim_master_migration_db_filename: "db_backupfile"
        nim_master_migration_lpp_source_v: "2317A_73D"
        nim_master_migration_spot_v: "2317A_73D_spot"
        nim_master_migration_nim_master_fileset_src: "~/bos.sysmgt"
```

---

</SwmSnippet>

## Viosupgrade operation using NIM

This playbook demonstrates how to upgrade VIOS (Virtual I/O Server) using NIM.

<SwmSnippet path="/playbooks/demo_nim_viosupgrade.yml" line="1">

---

The playbook <SwmPath>[playbooks/demo_nim_viosupgrade.yml](playbooks/demo_nim_viosupgrade.yml)</SwmPath> demonstrates how to upgrade VIOS using NIM.

```yaml
---
- name: Viosupgrade operation using NIM
  hosts: nimserver
  gather_facts: false
  vars:
    target_v: "quimby-vios2"
    target_file_v: "/tmp/mytargetfile"
    params_altdisk_v:
      all:
        resources: master_net_conf:logdir
        manage_cluster: true
        preview: true
      quimby-vios2:
        ios_mksysb: vios3-1-1-0_sysb
        rootvg_clone_disk: hdisk3
        backup_file_resource: quimby-vios2_iosb
    params_bosinst_v:
      all:
        ios_mksysb: vios3-1-1-0_sysb
        spotname: vios3-1-1-0_spot
        resources: master_net_conf:logdir
```

---

</SwmSnippet>

## Backup operation using NIM for on <SwmToken path="playbooks/demo_nim.yml" pos="2:11:13" line-data="- name: NIM operation on AIX/VIOS">`AIX/VIOS`</SwmToken>

This playbook demonstrates how to perform backup operations on <SwmToken path="playbooks/demo_nim.yml" pos="2:11:13" line-data="- name: NIM operation on AIX/VIOS">`AIX/VIOS`</SwmToken> using NIM.

<SwmSnippet path="/playbooks/demo_nim_backup.yml" line="1">

---

The playbook <SwmPath>[playbooks/demo_nim_backup.yml](playbooks/demo_nim_backup.yml)</SwmPath> demonstrates how to perform backup operations on <SwmToken path="playbooks/demo_nim_backup.yml" pos="2:17:19" line-data="- name: Backup operation using NIM for on AIX/VIOS">`AIX/VIOS`</SwmToken> using NIM.

```yaml
---
- name: Backup operation using NIM for on AIX/VIOS
  hosts: nimserver
  gather_facts: false
  vars:
    preview_v: true
    flags_v: ""
    other_attributes_v: ""
    # for savevg/restvg operation
    svg_type_v: savevg
    svg_location_v: /export/nim/savevg
    svg_name_v: ""
    svg_name_prefix_v: ""
    svg_name_postfix_v: ""
    svg_volume_group_v: datavg
    svg_other_attributes_v: -a disk=hdisk1 -a verbose=yes
    svg_shrink_fs_v: false
    # for Standalone LPAR
    # lpar_nim_node_v:        {}
    lpar_targets_v: quimby06
    sysb_location_v: /export/nim/mksysb
```

---

</SwmSnippet>

## BOS install using mksysb image

This playbook demonstrates how to install the Base Operating System (BOS) using mksysb images.

<SwmSnippet path="/playbooks/demo_nim_install.yml" line="1">

---

The playbook <SwmPath>[playbooks/demo_nim_install.yml](playbooks/demo_nim_install.yml)</SwmPath> demonstrates how to install the Base Operating System (BOS) using mksysb images.

```yaml
---
- name: BOS install using mksysb image
  hosts: nimserver
  gather_facts: false
  vars:
    vm_targets: quimby01
    res_group: basic_res_grp

  tasks:
    - name: Install using group resource
      ibm.power_aix.nim:
        action: bos_inst
        targets: "{{ vm_targets }}"
        group: "{{ res_group }}"
      register: result
    - ansible.builtin.debug:
        var: result
```

---

</SwmSnippet>

## Update operation using NIM on VIOS

This playbook demonstrates how to update VIOS through the NIM master.

<SwmSnippet path="/playbooks/demo_nim_updateios.yml" line="1">

---

The playbook <SwmPath>[playbooks/demo_nim_updateios.yml](playbooks/demo_nim_updateios.yml)</SwmPath> demonstrates how to update VIOS through the NIM master.

```yaml
---
- name: Update operation using NIM on VIOS
  hosts: nimserver
  gather_facts: false
  vars:
    preview_v: true
    targets_v: quimby-vios2
    lpp_source_v: ""
    rm_filesets_v: ""
    rm_installp_bundle_v: ""
    accept_licenses_v: true
    manage_cluster_v: false
    time_limit_v: "09/30/2020 14:30"

  tasks:
    - name: Update VIOS through the NIM Master
      ibm.power_aix.nim_updateios:
        action: install
        targets: "{{ targets_v }}"
        manage_cluster: "{{ manage_cluster_v }}"
        accept_licenses: "{{ accept_licenses_v }}"
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
