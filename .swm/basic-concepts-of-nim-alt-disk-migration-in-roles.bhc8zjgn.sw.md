---
title: Basic Concepts of NIM Alt Disk Migration in Roles
---
# Overview

NIM Alt Disk Migration is a role that assists in automating the migration of AIX 7.1/7.2 to AIX 7.3. It is particularly useful when migrating multiple nodes concurrently, as it can perform the migration without validating resources each time.

# Validations

The role includes specific validations for the NIM client, such as connectivity, OS level, and valid hardware platform for the OS. It allows for the execution of all validations and exits before performing the migration, which is useful for debugging purposes.

## Example Playbook

This playbook demonstrates how to use the NIM Alt Disk Migration role to perform an alternate disk migration. It includes variables for the NIM client, target disk, lpp source, and spot, and controls the phases of the migration.

<SwmSnippet path="/playbooks/demo_nim_alt_disk_migration.yml" line="13">

---

The playbook <SwmPath>[playbooks/demo_nim_alt_disk_migration.yml](playbooks/demo_nim_alt_disk_migration.yml)</SwmPath> shows how to set up and execute the NIM Alt Disk Migration role. It includes variables such as <SwmToken path="playbooks/demo_nim_alt_disk_migration.yml" pos="19:1:1" line-data="    nim_client_v: p9zpa-ansible-test2">`nim_client_v`</SwmToken>, <SwmToken path="playbooks/demo_nim_alt_disk_migration.yml" pos="20:1:1" line-data="    target_disk_v: &quot;&quot;">`target_disk_v`</SwmToken>, <SwmToken path="playbooks/demo_nim_alt_disk_migration.yml" pos="22:1:1" line-data="    lpp_source_v: lpp_72V_2114">`lpp_source_v`</SwmToken>, and <SwmToken path="playbooks/demo_nim_alt_disk_migration.yml" pos="23:1:1" line-data="    spot_v: spot_72V_2114">`spot_v`</SwmToken>.

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

# Role Variables

This section explains the variables that can be configured for the NIM Alt Disk Migration role, such as the NIM client, target disk, lpp source, and spot.

# Main Functions

There are several main functions in this role. Some of them include creating a database backup file, unconfiguring the NIM master machine, setting up the NIM master machine as a client, performing the NIM Alt Disk Migration operation, rebooting the machine, copying and installing the NIM master fileset, and transferring and restoring the database backup.

## Creating Database Backup File

This function is responsible for creating a backup of the database before starting the migration process. It ensures that there is a restore point in case anything goes wrong during the migration.

## Perform NIM Alt Disk Migration Operation

This function handles the actual migration of the NIM master machine to an alternate disk. It involves copying the necessary files and configurations to the new disk and preparing it for use.

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
