---
title: Overview of User and Group Management Demos
---
# Overview of User and Group Management Demos

User and Group Demos in Playbooks provide examples of how to manage users and groups on AIX systems using Ansible modules. These playbooks serve as practical examples for administrators to understand and implement user and group management on AIX systems using the Ansible ecosystem.

## User Management

The <SwmPath>[playbooks/demo_user.yml](playbooks/demo_user.yml)</SwmPath> playbook demonstrates various user management tasks such as creating users in LDAP, creating local users, modifying user attributes, and deleting users.

<SwmSnippet path="/playbooks/demo_user.yml" line="1">

---

The <SwmPath>[playbooks/demo_user.yml](playbooks/demo_user.yml)</SwmPath> playbook includes tasks for creating users in LDAP, creating local users, modifying user attributes, and deleting users. Below is an example of creating a user in LDAP.

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
```

---

</SwmSnippet>

## Example: Creating a User in LDAP

The following snippet from <SwmPath>[playbooks/demo_user.yml](playbooks/demo_user.yml)</SwmPath> demonstrates how to create a user in LDAP:

```
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

## Group Management

The <SwmPath>[playbooks/demo_group.yml](playbooks/demo_group.yml)</SwmPath> playbook showcases group management tasks including creating groups locally and in LDAP, adding and removing group members, modifying group attributes, and deleting groups.

<SwmSnippet path="/playbooks/demo_group.yml" line="1">

---

The <SwmPath>[playbooks/demo_group.yml](playbooks/demo_group.yml)</SwmPath> playbook includes tasks for creating groups locally and in LDAP, adding and removing group members, modifying group attributes, and deleting groups. Below is an example of creating a group.

```yaml
---
- name: GROUP on AIX
  hosts: "{{host_name}}"
  gather_facts: false
  vars:
    host_name: all
    user_list_val: aixtestgroup
    group_name: aix_g7
    group_name1: aix_g8
    load_module: LDAP
  tasks:

    - name: Create a group #create a group in local
      ibm.power_aix.group:
        state: present
        load_module: files
        name: "{{ group_name }}"
    - name: Create a group #create a group in LDAP even its present in local
      ibm.power_aix.group:
        state: present
        load_module: LDAP
```

---

</SwmSnippet>

## Example: Creating a Group

The following snippet from <SwmPath>[playbooks/demo_group.yml](playbooks/demo_group.yml)</SwmPath> demonstrates how to create a group:

```
- name: Create a group
  ibm.power_aix.group:
    state: present
    load_module: files
    name: "{{ group_name }}"
- name: Create a group in LDAP
  ibm.power_aix.group:
    state: present
    load_module: LDAP
```

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
