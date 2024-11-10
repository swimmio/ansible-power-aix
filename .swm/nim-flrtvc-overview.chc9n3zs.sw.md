---
title: NIM FLRTVC Overview
---
# NIM FLRTVC Overview

NIM FLRTVC is a module used to generate a Fix Level Recommendation Tool Vulnerability Checker (FLRTVC) report, download, and install security and HIPER (High Impact <SwmToken path="plugins/modules/nim_flrtvc.py" pos="21:27:27" line-data="- Use the NIM master to apply known security and HIPER (High Impact PERvasive) fixes on target">`PERvasive`</SwmToken>) fixes. It utilizes the NIM master to apply known security and HIPER fixes on target systems based on their inventory, ensuring the systems are at supported and secure levels.

## How NIM FLRTVC Works

The module downloads and uses the FLRTVC script to generate a report, parses this report, downloads the required fixes, extracts the files, and checks their versions against installed software levels. It also checks for file locking that could prevent fix installation, rejects fixes that do not match these requirements, and installs the remaining ones. In case of <SwmToken path="plugins/modules/nim_flrtvc.py" pos="27:8:10" line-data="- In case of inter-locking file(s) you might want run against the task.">`inter-locking`</SwmToken> files, the task might need to be run again. The list of installed and rejected fixes is provided in the results metadata.

## Main Functions

There are several main functions in this module: <SwmToken path="plugins/modules/nim_flrtvc.py" pos="1008:2:2" line-data="def run_flrtvc(module, output, machine, flrtvc_path, params, force):">`run_flrtvc`</SwmToken>, <SwmToken path="plugins/modules/nim_flrtvc.py" pos="1122:2:2" line-data="def run_downloader(module, machine, output, urls, resize_fs=True):">`run_downloader`</SwmToken>, and <SwmToken path="plugins/modules/nim_flrtvc.py" pos="1231:2:2" line-data="def run_installer(module, machine, output, epkgs, resize_fs=True):">`run_installer`</SwmToken>. Each of these functions plays a crucial role in the overall process of generating reports, downloading fixes, and installing them.

### <SwmToken path="plugins/modules/nim_flrtvc.py" pos="1008:2:2" line-data="def run_flrtvc(module, output, machine, flrtvc_path, params, force):">`run_flrtvc`</SwmToken>

The <SwmToken path="plugins/modules/nim_flrtvc.py" pos="1008:2:2" line-data="def run_flrtvc(module, output, machine, flrtvc_path, params, force):">`run_flrtvc`</SwmToken> function runs the FLRTVC command on a target system. It prepares the environment by running <SwmToken path="plugins/modules/nim_flrtvc.py" pos="1028:6:6" line-data="    # Run &#39;lslpp -Lcq&#39; on the remote machine and save to file">`lslpp`</SwmToken> and <SwmToken path="plugins/modules/nim_flrtvc.py" pos="527:15:15" line-data="            r&#39; for i in `/usr/sbin/emgr -P |/usr/bin/tail -n +4 |/usr/bin/awk \&#39;{print \$NF}\&#39;`;&#39;">`emgr`</SwmToken> commands to gather necessary information, waits for these commands to complete, and then executes the FLRTVC script. The results are parsed and stored in the output.

<SwmSnippet path="/plugins/modules/nim_flrtvc.py" line="1008">

---

The <SwmToken path="plugins/modules/nim_flrtvc.py" pos="1008:2:2" line-data="def run_flrtvc(module, output, machine, flrtvc_path, params, force):">`run_flrtvc`</SwmToken> function demonstrates how the FLRTVC script is executed on a target system, including the preparation of the command, execution, and handling of the output.

```python
def run_flrtvc(module, output, machine, flrtvc_path, params, force):
    """
    Run command flrtvc on a target system
    args:
        module     (dict): The Ansible module
        output     (dict): The result of the execution for the target host
        machine     (str): The remote machine name
        flrtvc_path (str): The path to the flrtvc script to run
        params     (dict): The parameters to pass to flrtvc command
        force      (bool): The flag to automatically remove efixes
    note:
        Create and build output['0.report']
    return:
        True if flrtvc succeeded
        False otherwise
    """

    if force:
        remove_efix(module, output, machine)

    # Run 'lslpp -Lcq' on the remote machine and save to file
```

---

</SwmSnippet>

### <SwmToken path="plugins/modules/nim_flrtvc.py" pos="1122:2:2" line-data="def run_downloader(module, machine, output, urls, resize_fs=True):">`run_downloader`</SwmToken>

The <SwmToken path="plugins/modules/nim_flrtvc.py" pos="1122:2:2" line-data="def run_downloader(module, machine, output, urls, resize_fs=True):">`run_downloader`</SwmToken> function downloads <SwmToken path="plugins/modules/nim_flrtvc.py" pos="1124:3:3" line-data="    Download URLs and check efixes">`URLs`</SwmToken> and checks efixes. It processes each URL, determines if it is an efix or tar file, downloads the file, and extracts efixes if necessary. The results are stored in the output.

<SwmSnippet path="/plugins/modules/nim_flrtvc.py" line="1122">

---

The <SwmToken path="plugins/modules/nim_flrtvc.py" pos="1122:2:2" line-data="def run_downloader(module, machine, output, urls, resize_fs=True):">`run_downloader`</SwmToken> function processes <SwmToken path="plugins/modules/nim_flrtvc.py" pos="1124:3:3" line-data="    Download URLs and check efixes">`URLs`</SwmToken>, downloads files, and extracts efixes if necessary.

```python
def run_downloader(module, machine, output, urls, resize_fs=True):
    """
    Download URLs and check efixes
    args:
        module (dict): The Ansible module
        machine    (str): The remote machine name
        output    (dict): The result of the command
        urls      (list): The list of URLs to download
        resize_fs (bool): Increase the filesystem size if needed
    note:
        Create and build
            output['2.discover']
            output['3.download']
            output['4.1.reject']
            output['4.2.check']
        for the provided machine.
    """

    out = {'messages': output['messages'],
           '2.discover': [],
           '3.download': [],
```

---

</SwmSnippet>

### <SwmToken path="plugins/modules/nim_flrtvc.py" pos="1231:2:2" line-data="def run_installer(module, machine, output, epkgs, resize_fs=True):">`run_installer`</SwmToken>

The <SwmToken path="plugins/modules/nim_flrtvc.py" pos="1231:2:2" line-data="def run_installer(module, machine, output, epkgs, resize_fs=True):">`run_installer`</SwmToken> function installs efixes on the target system. It copies the efix files to the appropriate directory, checks for any issues during the copy process, and logs the results. If the installation is successful, it updates the status in the output.

<SwmSnippet path="/plugins/modules/nim_flrtvc.py" line="1231">

---

The <SwmToken path="plugins/modules/nim_flrtvc.py" pos="1231:2:2" line-data="def run_installer(module, machine, output, epkgs, resize_fs=True):">`run_installer`</SwmToken> function installs efixes on the target system and updates the status in the output.

```python
def run_installer(module, machine, output, epkgs, resize_fs=True):
    """
    Install epkgs efixes
    args:
        module (dict): The Ansible module
        machine (str): The remote machine name
        output (dict): The result of the command
        epkgs  (list): The list of efixes to install
        resize_fs (bool): Increase the filesystem size if needed
    return:
        True if install succeeded
        False otherwise
    note:
        epkgs should be results['meta']['4.2.check'] which is
        sorted against packaging date. Do not change the order.
        Create and build results['meta']['5.install'].
    """

    if not epkgs:
        msg = 'Nothing to install'
        results['status'][machine] = 'SUCCESS'
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
