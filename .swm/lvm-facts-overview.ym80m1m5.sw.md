---
title: LVM Facts Overview
---
# LVM Facts Overview

LVM Facts is a module that reports Logical Volume Manager (LVM) information as facts. It lists and reports details about defined AIX LVM components such as Physical Volumes (<SwmToken path="plugins/modules/lvm_facts.py" pos="83:14:14" line-data="      - Contains a list of VGs, PVs and LVs.">`PVs`</SwmToken>), Logical Volumes (<SwmToken path="plugins/modules/lvm_facts.py" pos="83:18:18" line-data="      - Contains a list of VGs, PVs and LVs.">`LVs`</SwmToken>), and Volume Groups (<SwmToken path="plugins/modules/lvm_facts.py" pos="83:11:11" line-data="      - Contains a list of VGs, PVs and LVs.">`VGs`</SwmToken>). The module can specify the name of an LVM component and the type of LVM component to report information on, including <SwmToken path="plugins/modules/lvm_facts.py" pos="83:14:14" line-data="      - Contains a list of VGs, PVs and LVs.">`PVs`</SwmToken>, <SwmToken path="plugins/modules/lvm_facts.py" pos="83:18:18" line-data="      - Contains a list of VGs, PVs and LVs.">`LVs`</SwmToken>, <SwmToken path="plugins/modules/lvm_facts.py" pos="83:11:11" line-data="      - Contains a list of VGs, PVs and LVs.">`VGs`</SwmToken>, or all components. Users can provide existing LVM facts to which the queried facts should be updated, or the LVM facts in the <SwmToken path="plugins/modules/lvm_facts.py" pos="67:8:8" line-data="    lvm: &quot;{{ ansible_facts.LVM }}&quot;">`ansible_facts`</SwmToken> will be replaced. The module gathers information using commands like <SwmToken path="plugins/modules/lvm_facts.py" pos="393:6:6" line-data="    cmd = &quot;lsvg&quot;">`lsvg`</SwmToken> and <SwmToken path="plugins/modules/lvm_facts.py" pos="276:6:6" line-data="    cmd = &quot;lspv&quot;">`lspv`</SwmToken> to retrieve details about <SwmToken path="plugins/modules/lvm_facts.py" pos="83:11:11" line-data="      - Contains a list of VGs, PVs and LVs.">`VGs`</SwmToken>, <SwmToken path="plugins/modules/lvm_facts.py" pos="83:14:14" line-data="      - Contains a list of VGs, PVs and LVs.">`PVs`</SwmToken>, and <SwmToken path="plugins/modules/lvm_facts.py" pos="83:18:18" line-data="      - Contains a list of VGs, PVs and LVs.">`LVs`</SwmToken>. The gathered facts are then added to <SwmToken path="plugins/modules/lvm_facts.py" pos="67:8:8" line-data="    lvm: &quot;{{ ansible_facts.LVM }}&quot;">`ansible_facts`</SwmToken>, providing a comprehensive overview of the LVM components on the system.

## How to Use LVM Facts

This example shows how to gather all LVM facts, VG facts, update PV facts to existing LVM facts, and gather LV facts using the <SwmToken path="plugins/modules/lvm_facts.py" pos="58:1:1" line-data="  lvm_facts:">`lvm_facts`</SwmToken> module.

<SwmSnippet path="/plugins/modules/lvm_facts.py" line="57">

---

The following code demonstrates how to gather all LVM facts, VG facts, update PV facts to existing LVM facts, and gather LV facts using the <SwmToken path="plugins/modules/lvm_facts.py" pos="58:1:1" line-data="  lvm_facts:">`lvm_facts`</SwmToken> module.

```python
- name: Gather all lvm facts
  lvm_facts:
- name: Gather VG facts
  lvm_facts:
    name: all
    component: vg
- name: Update PV facts to existing LVM facts
  lvm_facts:
    name: all
    component: pv
    lvm: "{{ ansible_facts.LVM }}"
- name: Gather LV facts
  lvm_facts:
    name: all
    component: lv
'''
```

---

</SwmSnippet>

## Main Functions

There are several main functions in this module. Some of them are <SwmToken path="plugins/modules/lvm_facts.py" pos="264:2:2" line-data="def load_pvs(module, name, LVM):">`load_pvs`</SwmToken>, <SwmToken path="plugins/modules/lvm_facts.py" pos="308:13:13" line-data="                        LVM[&#39;PVs&#39;][pv] = parse_pvs(module, stdout, pv)">`parse_pvs`</SwmToken>, <SwmToken path="plugins/modules/lvm_facts.py" pos="381:2:2" line-data="def load_vgs(module, name, LVM):">`load_vgs`</SwmToken>, <SwmToken path="plugins/modules/lvm_facts.py" pos="418:13:13" line-data="                    LVM[&#39;VGs&#39;][vg] = parse_vgs(module, stdout, vg)">`parse_vgs`</SwmToken>, <SwmToken path="plugins/modules/lvm_facts.py" pos="470:2:2" line-data="def load_lvs(module, name, LVM):">`load_lvs`</SwmToken>, and <SwmToken path="plugins/modules/lvm_facts.py" pos="495:5:5" line-data="                    lv_data = parse_lvs(module, stdout, vg, name)">`parse_lvs`</SwmToken>. We will dive a little into each of these functions.

### <SwmToken path="plugins/modules/lvm_facts.py" pos="264:2:2" line-data="def load_pvs(module, name, LVM):">`load_pvs`</SwmToken>

The <SwmToken path="plugins/modules/lvm_facts.py" pos="264:2:2" line-data="def load_pvs(module, name, LVM):">`load_pvs`</SwmToken> function retrieves details for the specified Physical Volume (PV) or all <SwmToken path="plugins/modules/lvm_facts.py" pos="83:14:14" line-data="      - Contains a list of VGs, PVs and LVs.">`PVs`</SwmToken>. It runs the <SwmToken path="plugins/modules/lvm_facts.py" pos="276:6:6" line-data="    cmd = &quot;lspv&quot;">`lspv`</SwmToken> command to gather information and updates the LVM facts with the retrieved data.

<SwmSnippet path="/plugins/modules/lvm_facts.py" line="264">

---

The <SwmToken path="plugins/modules/lvm_facts.py" pos="264:2:2" line-data="def load_pvs(module, name, LVM):">`load_pvs`</SwmToken> function retrieves details for the specified Physical Volume (PV) or all <SwmToken path="plugins/modules/lvm_facts.py" pos="83:14:14" line-data="      - Contains a list of VGs, PVs and LVs.">`PVs`</SwmToken>. It runs the <SwmToken path="plugins/modules/lvm_facts.py" pos="276:6:6" line-data="    cmd = &quot;lspv&quot;">`lspv`</SwmToken> command to gather information and updates the LVM facts with the retrieved data.

```python
def load_pvs(module, name, LVM):
    """
    Get the details for the specified PV or all
    arguments:
        module  (dict): Ansible module argument spec.
        name     (str): physical volume name.
        LVM     (dict): LVM facts.
    return:
        warnings (list): List of warning messages
        LVM      (dict): LVM facts
    """
    warnings = []
    cmd = "lspv"
    rc, stdout, stderr = module.run_command(cmd)
    if rc != 0:
        warnings.append(
            f"Command failed. cmd={cmd} rc={rc} stdout={stdout} stderr={stderr}")
    else:
        for ln in stdout.splitlines():
            fields = ln.split()
            pv = fields[0]
```

---

</SwmSnippet>

### <SwmToken path="plugins/modules/lvm_facts.py" pos="308:13:13" line-data="                        LVM[&#39;PVs&#39;][pv] = parse_pvs(module, stdout, pv)">`parse_pvs`</SwmToken>

The <SwmToken path="plugins/modules/lvm_facts.py" pos="308:13:13" line-data="                        LVM[&#39;PVs&#39;][pv] = parse_pvs(module, stdout, pv)">`parse_pvs`</SwmToken> function parses the output of the <SwmToken path="plugins/modules/lvm_facts.py" pos="276:6:6" line-data="    cmd = &quot;lspv&quot;">`lspv`</SwmToken> command for a specific PV. It extracts relevant data and formats it into a dictionary that is used to update the LVM facts.

### <SwmToken path="plugins/modules/lvm_facts.py" pos="381:2:2" line-data="def load_vgs(module, name, LVM):">`load_vgs`</SwmToken>

The <SwmToken path="plugins/modules/lvm_facts.py" pos="381:2:2" line-data="def load_vgs(module, name, LVM):">`load_vgs`</SwmToken> function retrieves details for the specified Volume Group (VG) or all <SwmToken path="plugins/modules/lvm_facts.py" pos="83:11:11" line-data="      - Contains a list of VGs, PVs and LVs.">`VGs`</SwmToken>. It runs the <SwmToken path="plugins/modules/lvm_facts.py" pos="393:6:6" line-data="    cmd = &quot;lsvg&quot;">`lsvg`</SwmToken> command to gather information and updates the LVM facts with the retrieved data.

<SwmSnippet path="/plugins/modules/lvm_facts.py" line="381">

---

The <SwmToken path="plugins/modules/lvm_facts.py" pos="381:2:2" line-data="def load_vgs(module, name, LVM):">`load_vgs`</SwmToken> function retrieves details for the specified Volume Group (VG) or all <SwmToken path="plugins/modules/lvm_facts.py" pos="83:11:11" line-data="      - Contains a list of VGs, PVs and LVs.">`VGs`</SwmToken>. It runs the <SwmToken path="plugins/modules/lvm_facts.py" pos="393:6:6" line-data="    cmd = &quot;lsvg&quot;">`lsvg`</SwmToken> command to gather information and updates the LVM facts with the retrieved data.

```python
def load_vgs(module, name, LVM):
    """
    Get the details for the specified VG or all
    arguments:
        module  (dict): Ansible module argument spec.
        name     (str): volume group name.
        LVM     (dict): LVM facts.
    return:
        warnings (list): List of warning messages
        LVM      (dict): LVM facts
    """
    warnings = []
    cmd = "lsvg"
    rc, stdout, stderr = module.run_command(cmd)
    if rc != 0:
        warnings.append(f"Command failed. cmd={cmd} rc={rc} stdout={stdout} stderr={stderr}")
    else:
        for ln in stdout.splitlines():
            vg = ln.split()[0].strip()
            if (name != 'all' and name != vg):
                continue
```

---

</SwmSnippet>

### <SwmToken path="plugins/modules/lvm_facts.py" pos="470:2:2" line-data="def load_lvs(module, name, LVM):">`load_lvs`</SwmToken>

The <SwmToken path="plugins/modules/lvm_facts.py" pos="470:2:2" line-data="def load_lvs(module, name, LVM):">`load_lvs`</SwmToken> function retrieves details for the specified Logical Volume (LV) or all <SwmToken path="plugins/modules/lvm_facts.py" pos="83:18:18" line-data="      - Contains a list of VGs, PVs and LVs.">`LVs`</SwmToken>. It runs the <SwmToken path="plugins/modules/lvm_facts.py" pos="489:7:10" line-data="            cmd = f&quot;lsvg -l { vg }&quot;">`lsvg -l`</SwmToken> command to gather information and updates the LVM facts with the retrieved data.

<SwmSnippet path="/plugins/modules/lvm_facts.py" line="470">

---

The <SwmToken path="plugins/modules/lvm_facts.py" pos="470:2:2" line-data="def load_lvs(module, name, LVM):">`load_lvs`</SwmToken> function retrieves details for the specified Logical Volume (LV) or all <SwmToken path="plugins/modules/lvm_facts.py" pos="83:18:18" line-data="      - Contains a list of VGs, PVs and LVs.">`LVs`</SwmToken>. It runs the <SwmToken path="plugins/modules/lvm_facts.py" pos="489:7:10" line-data="            cmd = f&quot;lsvg -l { vg }&quot;">`lsvg -l`</SwmToken> command to gather information and updates the LVM facts with the retrieved data.

```python
def load_lvs(module, name, LVM):
    """
    Get the details for the specified LV or all
    arguments:
        module  (dict): Ansible module argument spec.
        name     (str): logical volume name.
        LVM     (dict): LVM facts.
    return:
        warnings (list): List of warning messages
        LVM      (dict): LVM facts
    """
    warnings = []
    cmd = "lsvg"
    rc, stdout, stderr = module.run_command(cmd)
    if rc != 0:
        warnings.append(f"Command failed. cmd={cmd} rc={rc} stdout={stdout} stderr={stderr}")
    else:
        for line in stdout.splitlines():
            vg = line.split()[0].strip()
            cmd = f"lsvg -l { vg }"
            rc, stdout, stderr = module.run_command(cmd)
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
