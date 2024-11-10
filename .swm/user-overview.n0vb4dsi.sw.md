---
title: User Overview
---
# User Overview

A user in the context of an AIX system refers to an entity that can be created, modified, or deleted using the provided module. This module allows for the creation of a new user with specified attributes, modification of existing user attributes, or deletion of a user. The module requires root user privileges or a privileged user with specific authorizations to perform these actions. It supports operations on users present in the local machine or in an LDAP server.

## User Attributes and Options

The module provides options to specify user attributes, delete the home directory upon user removal, and enforce password changes on first login. This flexibility ensures that user management can be tailored to specific requirements.

## Example: Creating a User

This example demonstrates how to create a user named <SwmToken path="plugins/modules/user.py" pos="100:9:9" line-data="- name: Create user aixguest1010">`aixguest1010`</SwmToken> with specific attributes and password settings.

<SwmSnippet path="/plugins/modules/user.py" line="99">

---

The code snippet shows how to create a user named <SwmToken path="plugins/modules/user.py" pos="100:9:9" line-data="- name: Create user aixguest1010">`aixguest1010`</SwmToken> with specific attributes and password settings.

```python
EXAMPLES = r'''
- name: Create user aixguest1010
  ibm.power_aix.user:
    state: present
    name: aixguest1010
    change_passwd_on_login: false
    password: as$12ndhkfjk$1c
    attributes:
      home: /home/test/aixguest1010
      data: 1272
'''
```

---

</SwmSnippet>

## Main Functions

There are several main functions in this module. Some of them are <SwmToken path="plugins/modules/user.py" pos="285:5:5" line-data="    current_attrs = get_user_attrs(module)">`get_user_attrs`</SwmToken>, <SwmToken path="plugins/modules/user.py" pos="287:5:5" line-data="    attrs = changed_attrs(module, current_attrs)">`changed_attrs`</SwmToken>, <SwmToken path="plugins/modules/user.py" pos="268:2:2" line-data="def modify_user(module):">`modify_user`</SwmToken>, <SwmToken path="plugins/modules/user.py" pos="316:2:2" line-data="def create_user(module):">`create_user`</SwmToken>, <SwmToken path="plugins/modules/user.py" pos="358:2:2" line-data="def remove_user(module):">`remove_user`</SwmToken>, <SwmToken path="plugins/modules/user.py" pos="389:2:2" line-data="def user_exists(module):">`user_exists`</SwmToken>, <SwmToken path="plugins/modules/user.py" pos="303:5:5" line-data="        msg_pass = change_password(module)">`change_password`</SwmToken>, and <SwmToken path="plugins/modules/user.py" pos="448:2:2" line-data="def main():">`main`</SwmToken>. We will dive a little into <SwmToken path="plugins/modules/user.py" pos="268:2:2" line-data="def modify_user(module):">`modify_user`</SwmToken>, <SwmToken path="plugins/modules/user.py" pos="316:2:2" line-data="def create_user(module):">`create_user`</SwmToken>, and <SwmToken path="plugins/modules/user.py" pos="358:2:2" line-data="def remove_user(module):">`remove_user`</SwmToken>.

### <SwmToken path="plugins/modules/user.py" pos="268:2:2" line-data="def modify_user(module):">`modify_user`</SwmToken>

The <SwmToken path="plugins/modules/user.py" pos="268:2:2" line-data="def modify_user(module):">`modify_user`</SwmToken> function modifies the attributes of an existing user. It retrieves the current attributes of the user, determines the changes needed, and applies those changes using the <SwmToken path="plugins/modules/user.py" pos="271:14:14" line-data="    output, return code and error of chuser command, if any.">`chuser`</SwmToken> command. If a password change is required, it also updates the user's password.

<SwmSnippet path="/plugins/modules/user.py" line="268">

---

The <SwmToken path="plugins/modules/user.py" pos="268:2:2" line-data="def modify_user(module):">`modify_user`</SwmToken> function modifies the attributes of the user and returns the output, return code, and error of the <SwmToken path="plugins/modules/user.py" pos="271:14:14" line-data="    output, return code and error of chuser command, if any.">`chuser`</SwmToken> command, if any.

```python
def modify_user(module):
    '''
    Modify_user function modifies the attributes of the user and returns
    output, return code and error of chuser command, if any.

    arguments:
        module  (dict): The Ansible module
    note:
        Exits with fail_json in case of error
    return:
        (message for command, changed status)
    '''

    name = module.params['name']
    msg = None
    changed = False
    # Get current user attributes
    current_attrs = get_user_attrs(module)
    # Get user attributes to change
    attrs = changed_attrs(module, current_attrs)
    # Redefine attributes
```

---

</SwmSnippet>

### <SwmToken path="plugins/modules/user.py" pos="316:2:2" line-data="def create_user(module):">`create_user`</SwmToken>

The <SwmToken path="plugins/modules/user.py" pos="316:2:2" line-data="def create_user(module):">`create_user`</SwmToken> function creates a new user with the specified attributes. It constructs the <SwmToken path="plugins/modules/user.py" pos="320:3:3" line-data="    for mkuser command, if any.">`mkuser`</SwmToken> command with the provided attributes and executes it. If a password is provided, it sets the password for the new user.

<SwmSnippet path="/plugins/modules/user.py" line="316">

---

The <SwmToken path="plugins/modules/user.py" pos="316:2:2" line-data="def create_user(module):">`create_user`</SwmToken> function creates the user with the attributes provided in the attributes field. It returns the standard output, return code, and error for the <SwmToken path="plugins/modules/user.py" pos="320:3:3" line-data="    for mkuser command, if any.">`mkuser`</SwmToken> command, if any.

```python
def create_user(module):
    '''
    Create_user function creates the user with the attributes provided in the
    attribiutes field. It returns the standard output, return code and error
    for mkuser command, if any.

    arguments:
        module  (dict): The Ansible module
    note:
        Exits with fail_json in case of error
        If this successfully returns, that means changes were made.
    return:
        Message for successful command.
    '''
    attributes = module.params['attributes']
    name = module.params['name']
    opts = ""
    load_module_opts = None
    msg = ""

    # Adding the load module to the command so that the user is created at the right location.
```

---

</SwmSnippet>

### <SwmToken path="plugins/modules/user.py" pos="358:2:2" line-data="def remove_user(module):">`remove_user`</SwmToken>

The <SwmToken path="plugins/modules/user.py" pos="358:2:2" line-data="def remove_user(module):">`remove_user`</SwmToken> function removes an existing user from the system. It constructs the <SwmToken path="plugins/modules/user.py" pos="371:7:7" line-data="    cmd = [&#39;userdel&#39;]">`userdel`</SwmToken> command and executes it. If the <SwmToken path="plugins/modules/user.py" pos="373:8:8" line-data="    if module.params[&#39;remove_homedir&#39;]:">`remove_homedir`</SwmToken> option is set, it also deletes the user's home directory.

<SwmSnippet path="/plugins/modules/user.py" line="358">

---

The <SwmToken path="plugins/modules/user.py" pos="358:2:2" line-data="def remove_user(module):">`remove_user`</SwmToken> function removes the user from the system. It returns the standard output, return code, and error for the <SwmToken path="plugins/modules/user.py" pos="371:7:7" line-data="    cmd = [&#39;userdel&#39;]">`userdel`</SwmToken> command, if any.

```python
def remove_user(module):
    '''
    Remove_user function removes the user from the system.It returns the standard output,
    return code and error for mkuser command, if any.

    arguments:
        module  (dict): The Ansible module
    note:
        Exits with fail_json in case of error
    return:
        Message for successfull command
    '''
    name = module.params['name']
    cmd = ['userdel']

    if module.params['remove_homedir']:
        cmd.append('-r')

    cmd.append(name)

    rc, stdout, stderr = module.run_command(cmd)
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
