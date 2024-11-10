---
title: Overview of Ansible Playbooks
---
# Playbooks Overview

Playbooks are YAML files that define a series of tasks to be executed on specified hosts. They are used to automate the configuration and management of AIX systems. Each playbook contains one or more plays, and each play specifies the hosts to target and the tasks to be performed on those hosts. Tasks are executed in the order they are defined.

# Variables in Playbooks

Playbooks can include variables, which allow for dynamic values to be used within tasks. These variables can be defined within the playbook itself or in separate variable files.

# Sample Playbooks

The IBM Power Systems AIX collection includes sample playbooks that demonstrate how to use the collection's modules to perform various operations on AIX systems. These sample playbooks cover a wide range of scenarios, such as updating software, managing system configurations, and performing maintenance tasks.

# Playbook Documentation

An Ansible playbook consists of organized instructions that define work for a managed node (host) to be managed with Ansible. A playbooks directory that contains a sample playbook is included in the IBM Power Systems AIX collection. The sample playbook can be run with the `ansible-playbook` command with some modification to the inventory and group_vars.

# Sample Playbook Example

This sample playbook demonstrates how to manage user accounts on AIX systems using the IBM Power Systems AIX collection. It includes tasks for creating, modifying, and deleting users both locally and in LDAP.

<SwmSnippet path="/playbooks/demo_user.yml" line="1">

---

This sample playbook demonstrates how to manage user accounts on AIX systems using the IBM Power Systems AIX collection. It includes tasks for creating, modifying, and deleting users both locally and in LDAP.

```yaml
---
- name: USER on AIX
  hosts: "{{host_name}}"
  gather_facts: false
  vars:
    host_name: all
    user_name: aixguest
    password_val: abc12345
    attribute_home: /home/test/aixguest

  tasks:
    - name: Create user in LDAP
      ibm.power_aix.user:
        state: present
        name: "{{ user_name }}"
        change_passwd_on_login: false
        password: "{{ password_val }}"
        load_module: LDAP
        attributes:
          home: "{{ attribute_home }}"
          data: 1272
```

---

</SwmSnippet>

# Endpoints of Playbooks

Endpoints of Playbooks provide specific functionalities and operations that can be performed using playbooks. Below are some examples of endpoints and their purposes.

## <SwmToken path="playbooks/demo_nim_alt_disk_migration.yml" pos="13:6:6" line-data="- name: &quot;Nim_alt_disk_migration demo&quot;">`Nim_alt_disk_migration`</SwmToken> demo

The <SwmToken path="playbooks/demo_nim_alt_disk_migration.yml" pos="13:6:8" line-data="- name: &quot;Nim_alt_disk_migration demo&quot;">`Nim_alt_disk_migration demo`</SwmToken> endpoint performs an alternate disk migration on a NIM client. It includes tasks such as importing the <SwmToken path="playbooks/demo_nim_alt_disk_migration.yml" pos="32:4:4" line-data="        name: nim_alt_disk_migration">`nim_alt_disk_migration`</SwmToken> role and setting various parameters like the NIM client, target disk, LPP source, and control phases.

<SwmSnippet path="/playbooks/demo_nim_alt_disk_migration.yml" line="13">

---

The <SwmToken path="playbooks/demo_nim_alt_disk_migration.yml" pos="13:6:8" line-data="- name: &quot;Nim_alt_disk_migration demo&quot;">`Nim_alt_disk_migration demo`</SwmToken> endpoint performs an alternate disk migration on a NIM client. It includes tasks such as importing the <SwmToken path="playbooks/demo_nim_alt_disk_migration.yml" pos="32:4:4" line-data="        name: nim_alt_disk_migration">`nim_alt_disk_migration`</SwmToken> role and setting various parameters like the NIM client, target disk, LPP source, and control phases.

```yaml
- name: "Nim_alt_disk_migration demo"
  hosts: "{{ hosts_val }}"
  gather_facts: false
  remote_user: root
  vars:
    hosts_val: nimserver
    nim_client_v: p9zpa-ansible-test2
    target_disk_v: ""
    target_disk_policy_v: minimize
    lpp_source_v: lpp_72V_2114
    spot_v: spot_72V_2114
    force_target_disk: true
    validate_nim_resources_v: true
    perform_migration_v: true

  tasks:

    # perform alt_disk_migration
    - ansible.builtin.import_role:
        name: nim_alt_disk_migration
      vars:
```

---

</SwmSnippet>

## AIX sync all ifixes on webserver

The <SwmToken path="playbooks/demo_shell_flrtvc_wget_ifix.yml" pos="8:6:16" line-data="- name: &quot;AIX sync all ifixes on webserver&quot;">`AIX sync all ifixes on webserver`</SwmToken> endpoint synchronizes ifixes, APAR CSV, and FLRTVC files from specified URLs to a local directory on the webserver. It includes tasks for creating the directory, downloading files using <SwmToken path="playbooks/demo_shell_flrtvc_wget_ifix.yml" pos="32:13:13" line-data="        cmd: &quot;{{ proxy }} wget -q -nc -r -np -nd --no-check-certificate  -l 1 -A .tar,.asc,.sig  {{ ifix_url }} &quot;">`wget`</SwmToken>, and fixing permissions.

<SwmSnippet path="/playbooks/demo_shell_flrtvc_wget_ifix.yml" line="8">

---

The <SwmToken path="playbooks/demo_shell_flrtvc_wget_ifix.yml" pos="8:6:16" line-data="- name: &quot;AIX sync all ifixes on webserver&quot;">`AIX sync all ifixes on webserver`</SwmToken> endpoint synchronizes ifixes, APAR CSV, and FLRTVC files from specified URLs to a local directory on the webserver. It includes tasks for creating the directory, downloading files using <SwmToken path="playbooks/demo_shell_flrtvc_wget_ifix.yml" pos="32:13:13" line-data="        cmd: &quot;{{ proxy }} wget -q -nc -r -np -nd --no-check-certificate  -l 1 -A .tar,.asc,.sig  {{ ifix_url }} &quot;">`wget`</SwmToken>, and fixing permissions.

```yaml
- name: "AIX sync all ifixes on webserver"
  hosts: aap-server
  gather_facts: false
  vars:
    ifix_path: "/var/lib/awx/ifix/"
    ifix_url: "https://aix.software.ibm.com/aix/ifixes/security/"
    apar_csv_url: "https://esupport.ibm.com/customercare/flrt/doc?page=aparCSV"
    apar_csv_filename: "apar.csv"
    flrtvc_url: "https://esupport.ibm.com/customercare/sas/f/flrt3/FLRTVC-latest.zip"
    flrtvc_filename: "FLRTVC-latest.zip"
    proxy: "http_proxy=YOURPROXYHERE:8080 https_proxy=$http_proxy"
    debug: false
    sync_ifix: true
    sync_apar: true
    sync_flrtvc: true
  tasks:
    - name: "Create ifix_path /var/lib/awx/ifix/ if not exists"
      ansible.builtin.file:
        path: "{{ ifix_path }}"
        state: directory
        mode: '0755'
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
