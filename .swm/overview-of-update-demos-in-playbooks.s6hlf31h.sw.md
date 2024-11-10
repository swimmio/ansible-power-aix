---
title: Overview of Update Demos in Playbooks
---
# Overview of Update Demos in Playbooks

Update Demos in Playbooks demonstrate how to apply updates to AIX systems using Ansible modules. These demos include tasks for installing all updates, updating RPM images, and installing the latest level of install utilities.

# Usage Examples

The playbook <SwmPath>[playbooks/demo_install_all_updates.yml](playbooks/demo_install_all_updates.yml)</SwmPath> showcases how to install all installp updates from a specified device, update RPM images, and install the latest install utilities. The playbook <SwmPath>[playbooks/demo_installp.yml](playbooks/demo_installp.yml)</SwmPath> demonstrates various installp actions such as listing software products, listing fixes, installing filesets, and removing filesets. The playbook <SwmPath>[playbooks/demo_flrtvc.yml](playbooks/demo_flrtvc.yml)</SwmPath> is used to run the FLRTVC tool on AIX, which checks for recommended fixes and updates. The playbook <SwmPath>[playbooks/demo_shell_flrtvc_wget_ifix.yml](playbooks/demo_shell_flrtvc_wget_ifix.yml)</SwmPath> shows how to synchronize ifixes, APAR CSV files, and FLRTVC files from a web server.

# Main Functions

There are several main functions in this folder. Some of them are installing all installp updates, updating RPM images, and installing the latest level of install utilities. We will dive a little into installing all installp updates and updating RPM images.

## Install All Installp Updates

The task <SwmToken path="playbooks/demo_install_all_updates.yml" pos="10:6:16" line-data="    - name: Install all installp updates on device">`Install all installp updates on device`</SwmToken> uses the <SwmToken path="playbooks/demo_install_all_updates.yml" pos="11:1:5" line-data="      ibm.power_aix.install_all_updates:">`ibm.power_aix.install_all_updates`</SwmToken> module to install all updates from the specified device.

<SwmSnippet path="/playbooks/demo_install_all_updates.yml" line="10">

---

This snippet demonstrates how to install all installp updates on a device using the <SwmToken path="playbooks/demo_install_all_updates.yml" pos="11:1:5" line-data="      ibm.power_aix.install_all_updates:">`ibm.power_aix.install_all_updates`</SwmToken> module.

```yaml
    - name: Install all installp updates on device
      ibm.power_aix.install_all_updates:
        device: "{{ device_val }}"
```

---

</SwmSnippet>

## Update RPM Images

The task <SwmToken path="playbooks/demo_install_all_updates.yml" pos="14:6:18" line-data="    - name: Update any rpm images on your system, with newer technology levels from the /images directory">`Update any rpm images on your system`</SwmToken> uses the <SwmToken path="playbooks/demo_install_all_updates.yml" pos="11:1:5" line-data="      ibm.power_aix.install_all_updates:">`ibm.power_aix.install_all_updates`</SwmToken> module with the <SwmToken path="playbooks/demo_install_all_updates.yml" pos="17:1:1" line-data="        update_rpm: true">`update_rpm`</SwmToken> parameter set to true to update RPM images from the specified device.

<SwmSnippet path="/playbooks/demo_install_all_updates.yml" line="14">

---

This snippet demonstrates how to update RPM images on your system using the <SwmToken path="playbooks/demo_install_all_updates.yml" pos="15:1:5" line-data="      ibm.power_aix.install_all_updates:">`ibm.power_aix.install_all_updates`</SwmToken> module with the <SwmToken path="playbooks/demo_install_all_updates.yml" pos="17:1:1" line-data="        update_rpm: true">`update_rpm`</SwmToken> parameter.

```yaml
    - name: Update any rpm images on your system, with newer technology levels from the /images directory
      ibm.power_aix.install_all_updates:
        device: "{{ device_val }}"
        update_rpm: true
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
