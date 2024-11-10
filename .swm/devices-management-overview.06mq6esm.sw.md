---
title: Devices Management Overview
---
# Devices Overview

Devices refer to the hardware components managed within a logical partition (LPAR) on AIX systems. The module allows configuring, modifying, and unconfiguring devices, ensuring they are in the desired state. Devices can be set to different states such as 'available', 'defined', or 'removed', depending on the required operation.

## Devices Management

The module supports operations on individual devices or all devices within the LPAR, providing flexibility in device management. It also includes options for forcing changes on locked devices and recursively unconfiguring devices along with their children.

## Device States

Attributes of devices can be changed when they are in the 'available' state, and devices can be unconfigured or stopped when in the 'defined' state.

## Example of Configuring a Device

This example shows how to configure a device named <SwmToken path="plugins/modules/devices.py" pos="104:4:4" line-data="    device: proc0">`proc0`</SwmToken> to be in the 'available' state.

<SwmSnippet path="/plugins/modules/devices.py" line="102">

---

Example of configuring a device named <SwmToken path="plugins/modules/devices.py" pos="104:4:4" line-data="    device: proc0">`proc0`</SwmToken> to be in the 'available' state.

```python
- name: Configure a device
  devices:
    device: proc0
    state: available
```

---

</SwmSnippet>

## Example of Unconfiguring a Device

This example demonstrates how to unconfigure a device named <SwmToken path="plugins/modules/devices.py" pos="104:4:4" line-data="    device: proc0">`proc0`</SwmToken> to be in the 'defined' state.

<SwmSnippet path="/plugins/modules/devices.py" line="107">

---

Example of unconfiguring a device named <SwmToken path="plugins/modules/devices.py" pos="109:4:4" line-data="    device: proc0">`proc0`</SwmToken> to be in the 'defined' state.

```python
- name: Unconfigure a device
  devices:
    device: proc0
    state: defined
```

---

</SwmSnippet>

## Example of Removing a Device

This example illustrates how to remove a device named <SwmToken path="plugins/modules/devices.py" pos="112:11:11" line-data="- name: Remove (delete) fcs0 device and children">`fcs0`</SwmToken> and its children, setting the state to 'removed'.

<SwmSnippet path="/plugins/modules/devices.py" line="112">

---

Example of removing a device named <SwmToken path="plugins/modules/devices.py" pos="112:11:11" line-data="- name: Remove (delete) fcs0 device and children">`fcs0`</SwmToken> and its children, setting the state to 'removed'.

```python
- name: Remove (delete) fcs0 device and children
  devices:
    device: fcs0
    state: removed
    recursive: 'true'
```

---

</SwmSnippet>

# Main Functions

There are several main functions in this folder. Some of them are <SwmToken path="plugins/modules/devices.py" pos="303:2:2" line-data="def chdev(module, device):">`chdev`</SwmToken>, <SwmToken path="plugins/modules/devices.py" pos="379:2:2" line-data="def cfgdev(module, device):">`cfgdev`</SwmToken>, and <SwmToken path="plugins/modules/devices.py" pos="411:2:2" line-data="def rmdev(module, device, state):">`rmdev`</SwmToken>. We will dive a little into <SwmToken path="plugins/modules/devices.py" pos="303:2:2" line-data="def chdev(module, device):">`chdev`</SwmToken> and <SwmToken path="plugins/modules/devices.py" pos="379:2:2" line-data="def cfgdev(module, device):">`cfgdev`</SwmToken>.

## chdev

The <SwmToken path="plugins/modules/devices.py" pos="303:2:2" line-data="def chdev(module, device):">`chdev`</SwmToken> function changes the attributes of a device. It first fetches the initial properties of the device using <SwmToken path="plugins/modules/devices.py" pos="319:5:5" line-data="    init_props = get_device_attributes(module, device)">`get_device_attributes`</SwmToken>, then checks for idempotency with <SwmToken path="plugins/modules/devices.py" pos="321:8:8" line-data="    attributes, msg = check_idempotency(module, init_props, attributes, msg)">`check_idempotency`</SwmToken>. If changes are needed, it constructs the appropriate command options and executes the change.

<SwmSnippet path="/plugins/modules/devices.py" line="303">

---

The <SwmToken path="plugins/modules/devices.py" pos="303:2:2" line-data="def chdev(module, device):">`chdev`</SwmToken> function changes the attributes of a device.

```python
def chdev(module, device):
    """
    Changes the attributes of the device.
    param module: Ansible module argument spec.
    param device: Volume Group name.
    return: changed - True/False(device state modified or not),
            msg - message
    """
    attributes = module.params["attributes"]
    force = module.params["force"]
    chtype = module.params["chtype"]
    parent_device = module.params["parent_device"]
    msg = ''

    ''' get initial properties of the device before
    attempting to modfiy it. '''
    init_props = get_device_attributes(module, device)

    attributes, msg = check_idempotency(module, init_props, attributes, msg)

    opts = ""
```

---

</SwmSnippet>

## cfgdev

The <SwmToken path="plugins/modules/devices.py" pos="379:2:2" line-data="def cfgdev(module, device):">`cfgdev`</SwmToken> function configures a device or discovers all devices. It determines the current state of the device using <SwmToken path="plugins/modules/devices.py" pos="390:5:5" line-data="        current_state = get_device_state(module, device)">`get_device_state`</SwmToken> and then executes the configuration command if the device is not already in the desired state.

<SwmSnippet path="/plugins/modules/devices.py" line="379">

---

The <SwmToken path="plugins/modules/devices.py" pos="379:2:2" line-data="def cfgdev(module, device):">`cfgdev`</SwmToken> function configures a device or discovers all devices.

```python
def cfgdev(module, device):
    """
    Configure the device or discover all devices (device=all)
    param module: Ansible module argument spec.
    param device: device name.
    return: changed - True/False(device state modified or not),
            msg - message
    """
    current_state = 'None'
    cmd = "cfgmgr "
    if device != 'all':
        current_state = get_device_state(module, device)
        if current_state is True:
            msg = f"Device { device } is already in Available state."
            return False, msg

        if current_state is None:
            msg = f"Device { device } does not exist."
            module.fail_json(msg=msg)

        cmd += f"-l { device } "
```

---

</SwmSnippet>

## rmdev

The <SwmToken path="plugins/modules/devices.py" pos="411:2:2" line-data="def rmdev(module, device, state):">`rmdev`</SwmToken> function unconfigures or stops a device when its state is 'defined' and removes the device definition when its state is 'removed'. It uses <SwmToken path="plugins/modules/devices.py" pos="390:5:5" line-data="        current_state = get_device_state(module, device)">`get_device_state`</SwmToken> to determine the current state of the device and constructs the appropriate command options for the removal operation.

<SwmSnippet path="/plugins/modules/devices.py" line="411">

---

The <SwmToken path="plugins/modules/devices.py" pos="411:2:2" line-data="def rmdev(module, device, state):">`rmdev`</SwmToken> function unconfigures or stops a device when its state is 'defined' and removes the device definition when its state is 'removed'.

```python
def rmdev(module, device, state):
    """
    Unconfigure/stop the device when state is 'defined'
    Removes the device definition in Customized Devices object class when state is 'removed'
    param module: Ansible module argument spec.
    param device: device name.
    param state: state of the device
    return: changed - True/False(device state modified or not or device definition
                      is removed or not),
    msg - message
    """
    parent_device = module.params["parent_device"]
    force = module.params["force"]
    recursive = module.params["recursive"]
    rmtype = module.params["rmtype"]
    current_state = None
    opts = ""

    if device != 'all':
        current_state = get_device_state(module, device)
        if current_state is None:
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
