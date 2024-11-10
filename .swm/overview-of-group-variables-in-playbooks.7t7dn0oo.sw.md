---
title: Overview of Group Variables in Playbooks
---
# Overview of Group Variables

Group Variables are used to define variables that are shared across multiple hosts within a playbook. These variables are stored in YAML files located in the `group_vars` directory. Group Variables help in managing configurations centrally, making it easier to maintain and update settings that are common across multiple hosts or playbooks.

# Usage in Playbooks

In the <SwmPath>[playbooks/group_vars/all.yml](playbooks/group_vars/all.yml)</SwmPath> file, variables are defined for various demo playbooks such as <SwmToken path="playbooks/group_vars/all.yml" pos="8:6:6" line-data="# vars in demo_flrtvc">`demo_flrtvc`</SwmToken>, <SwmToken path="playbooks/group_vars/all.yml" pos="15:6:6" line-data="# vars in demo_suma">`demo_suma`</SwmToken>, <SwmToken path="playbooks/group_vars/all.yml" pos="19:6:6" line-data="# vars in demo_yum">`demo_yum`</SwmToken>, and <SwmToken path="playbooks/group_vars/all.yml" pos="24:6:6" line-data="# vars in demo_yum_install_DB">`demo_yum_install_DB`</SwmToken>. These variables include settings like paths, states, and package names.

<SwmSnippet path="/playbooks/group_vars/all.yml" line="1">

---

The <SwmPath>[playbooks/group_vars/all.yml](playbooks/group_vars/all.yml)</SwmPath> file contains variables for various demo playbooks, including settings like paths, states, and package names. Here is an example of how these variables are defined:

```
1 # Contains jinja variables found throughout the sample playbooks
2 #
3 # Uncomment and modify when using the ansible-playbooks command
4 #
5 # NOTE: modify the behavior of individual hosts by providing
6 # entries in the host_vars directory
7 
8 # vars in demo_flrtvc
9 # apar: sec
10 # path: /usr/sys/inst.images
11 # check: no
12 # download_only: yes
13 # increase: yes
14 
15 # vars in demo_suma
16 # download_dir: /usr/sys/inst.images
17 # download_only: False
18 
19 # vars in demo_yum
20 # name: '*'
21 # state: latest
```

```yaml
# Contains jinja variables found throughout the sample playbooks
#
# Uncomment and modify when using the ansible-playbooks command
#
# NOTE: modify the behavior of individual hosts by providing
#       entries in the host_vars directory

# vars in demo_flrtvc
# apar:          sec
# path:          /usr/sys/inst.images
# check:         no
# download_only: yes
# increase:      yes

# vars in demo_suma
# download_dir:  /usr/sys/inst.images
# download_only: False

# vars in demo_yum
# name:    '*'
# state:   latest
```

---

</SwmSnippet>

# Specific Group Variables

Similarly, the <SwmPath>[playbooks/group_vars/nimserver.yml](playbooks/group_vars/nimserver.yml)</SwmPath> file contains variables specific to the <SwmToken path="playbooks/group_vars/nimserver.yml" pos="1:14:15" line-data="# Contains jinja variables found throughout the nim_* playbooks">`nim_*`</SwmToken> playbooks. These variables include configurations for tasks like NIM installation and FLRTVC operations.

<SwmSnippet path="/playbooks/group_vars/nimserver.yml" line="1">

---

The <SwmPath>[playbooks/group_vars/nimserver.yml](playbooks/group_vars/nimserver.yml)</SwmPath> file contains variables specific to the <SwmToken path="playbooks/group_vars/nimserver.yml" pos="1:14:15" line-data="# Contains jinja variables found throughout the nim_* playbooks">`nim_*`</SwmToken> playbooks, including configurations for tasks like NIM installation and FLRTVC operations. Here is an example of how these variables are defined:

```
1 # Contains jinja variables found throughout the nim_* playbooks
2 
3 # vars in demo_nim_install
4 # vm_targets: quimby01
5 # res_group: basic_res_grp
6 
7 # vars in demo_nim_flrtvc
8 # apar: all
9 # vm_targets: quimby01
10 # path: /export/nim/ansible
11 # download: no
```

```yaml
# Contains jinja variables found throughout the nim_* playbooks

# vars in demo_nim_install
# vm_targets: quimby01
# res_group: basic_res_grp

# vars in demo_nim_flrtvc
# apar: all
# vm_targets: quimby01
# path: /export/nim/ansible
# download: no
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
