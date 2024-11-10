---
title: FLRTVC Overview
---
# FLRTVC Overview

FLRTVC is a module used to generate a report, download, and install security and HIPER (High Impact <SwmToken path="plugins/modules/flrtvc.py" pos="35:17:17" line-data="- Applies known security and HIPER (High Impact PERvasive) fixes on your system based on its">`PERvasive`</SwmToken>) fixes. It applies known security and HIPER fixes based on the system's inventory, ensuring the systems are at supported and secure levels.

## How FLRTVC Works

The module downloads and uses the Fix Level Recommendation Tool Vulnerability Checker script to generate a report. It parses the report, downloads the required fixes, extracts the files, and checks their versions against installed software levels. The module also checks for file locking that might prevent fix installation, rejecting fixes that do not match the requirements and installing the remaining ones. Users receive a list of installed and rejected fixes in the results metadata.

## Example Usage

This example demonstrates how to use FLRTVC to download patches for security vulnerabilities.

<SwmSnippet path="/plugins/modules/flrtvc.py" line="157">

---

This code snippet shows how to use FLRTVC to download patches for security vulnerabilities.

```python
- name: Download patches for security vulnerabilities
  flrtvc:
    apar: sec
    path: /usr/sys/inst.images
    download_only: true
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/flrtvc.py" line="172">

---

This code snippet shows how to use FLRTVC to install patches from a local patch server.

```python
- name: Install patches from local patch server
  flrtvc:
    apar: sec
    protocol: https
    localpatchserver: 192.168.1.1
    localpatchpath: ifix
    flrtvczip: https://192.168.1.1/ifix/flrtvc.zip
    csv: https://192.168.1.1/ifix/apar.csv
```

---

</SwmSnippet>

# Main Functions

There are several main functions in this module. Some of them are <SwmToken path="plugins/modules/flrtvc.py" pos="956:2:2" line-data="def run_flrtvc(flrtvc_path, params, force):">`run_flrtvc`</SwmToken>, <SwmToken path="plugins/modules/flrtvc.py" pos="1083:2:2" line-data="def run_downloader(urls, dst_path, resize_fs=True):">`run_downloader`</SwmToken>, and <SwmToken path="plugins/modules/flrtvc.py" pos="1189:2:2" line-data="def run_installer(epkgs, dst_path, resize_fs=True):">`run_installer`</SwmToken>. We will dive a little into these functions.

## <SwmToken path="plugins/modules/flrtvc.py" pos="956:2:2" line-data="def run_flrtvc(flrtvc_path, params, force):">`run_flrtvc`</SwmToken>

The <SwmToken path="plugins/modules/flrtvc.py" pos="956:2:2" line-data="def run_flrtvc(flrtvc_path, params, force):">`run_flrtvc`</SwmToken> function uses the FLRTVC script on the target system to generate a report. It first removes existing efixes if the force flag is set, then runs the <SwmToken path="plugins/modules/flrtvc.py" pos="973:6:6" line-data="    # Run &#39;lslpp -Lcq&#39; on the system and save to file">`lslpp`</SwmToken> and <SwmToken path="plugins/modules/flrtvc.py" pos="149:19:19" line-data="  - When the FLRTVC ksh script cannot execute the emgr command, it tries with B(sudo), so you can">`emgr`</SwmToken> commands to list filesets and efixes, respectively. It waits for these commands to complete and then runs the FLRTVC script in compact mode. If the script fails, it logs the error and returns False.

<SwmSnippet path="/plugins/modules/flrtvc.py" line="956">

---

This code snippet shows the implementation of the <SwmToken path="plugins/modules/flrtvc.py" pos="956:2:2" line-data="def run_flrtvc(flrtvc_path, params, force):">`run_flrtvc`</SwmToken> function.

```python
def run_flrtvc(flrtvc_path, params, force):
    """
    Use the flrtvc script on target system to get the
    args:
        flrtvc_path (str): The path to the flrtvc script to run
        params     (dict): The parameters to pass to flrtvc command
        force      (bool): The flag to automatically remove efixes
    note:
        Create and build results['meta']['0.report']
    return:
        True if flrtvc succeeded
        False otherwise
    """

    if force:
        remove_efix()

    # Run 'lslpp -Lcq' on the system and save to file
    lslpp_file = os.path.join(workdir, 'lslpp.txt')
    if os.path.exists(lslpp_file):
        os.remove(lslpp_file)
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/flrtvc.py" pos="1083:2:2" line-data="def run_downloader(urls, dst_path, resize_fs=True):">`run_downloader`</SwmToken>

The <SwmToken path="plugins/modules/flrtvc.py" pos="1083:2:2" line-data="def run_downloader(urls, dst_path, resize_fs=True):">`run_downloader`</SwmToken> function downloads <SwmToken path="plugins/modules/flrtvc.py" pos="1085:3:3" line-data="    Download URLs and check efixes">`URLs`</SwmToken> and checks efixes. It iterates over the list of <SwmToken path="plugins/modules/flrtvc.py" pos="1085:3:3" line-data="    Download URLs and check efixes">`URLs`</SwmToken>, downloading each one and checking if it is an efix or tar file. If it is a tar file, it extracts the efixes and adds them to the list of discovered efixes. It also handles filesystem resizing if needed.

<SwmSnippet path="/plugins/modules/flrtvc.py" line="1083">

---

This code snippet shows the implementation of the <SwmToken path="plugins/modules/flrtvc.py" pos="1083:2:2" line-data="def run_downloader(urls, dst_path, resize_fs=True):">`run_downloader`</SwmToken> function.

```python
def run_downloader(urls, dst_path, resize_fs=True):
    """
    Download URLs and check efixes
    args:
        urls      (list): The list of URLs to download
        dst_path   (str): Path directory where to download
        resize_fs (bool): Increase the filesystem size if needed
    note:
        Create and build
            results['meta']['2.discover']
            results['meta']['3.download']
            results['meta']['4.1.reject']
            results['meta']['4.2.check']
    """
    out = {'messages': results['meta']['messages'],
           '2.discover': [],
           '3.download': [],
           '4.1.reject': [],
           '4.2.check': []}

    for url in urls:
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/flrtvc.py" pos="1189:2:2" line-data="def run_installer(epkgs, dst_path, resize_fs=True):">`run_installer`</SwmToken>

The <SwmToken path="plugins/modules/flrtvc.py" pos="1189:2:2" line-data="def run_installer(epkgs, dst_path, resize_fs=True):">`run_installer`</SwmToken> function installs efixes. It first checks if there are any efixes to install and creates the necessary directories. It then copies the efixes to the destination directory and performs the installation. If any errors occur during the copy process, it logs the error and continues with the next efix.

<SwmSnippet path="/plugins/modules/flrtvc.py" line="1189">

---

This code snippet shows the implementation of the <SwmToken path="plugins/modules/flrtvc.py" pos="1189:2:2" line-data="def run_installer(epkgs, dst_path, resize_fs=True):">`run_installer`</SwmToken> function.

```python
def run_installer(epkgs, dst_path, resize_fs=True):
    """
    Install epkgs efixes
    args:
        epkgs     (list): The list of efixes to install
        dst_path   (str): Path directory where to install
        resize_fs (bool): Increase the filesystem size if needed
    return:
        True if geninstall succeeded
        False otherwise
    note:
        epkgs should be results['meta']['4.2.check'] which is
        sorted against packaging date. Do not change the order.
        Create and build results['meta']['5.install']
    """
    if not epkgs:
        # There were fixes downloaded but not interim fixes, which are the ones
        # the flrtvc module could install.
        msg = 'There are no interim fixes in epkg format to be installed.'
        results['meta']['messages'].append(msg)
        return True
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
