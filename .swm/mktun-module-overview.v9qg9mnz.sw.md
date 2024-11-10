---
title: MKTUN Module Overview
---
# MKTUN Overview

MKTUN is a module used to manage tunnel configurations on AIX systems. It allows for the creation, activation, deactivation, and removal of tunnels. The module can also export and import tunnel definitions, supporting both <SwmToken path="plugins/modules/mktun.py" pos="190:15:15" line-data="- name: Create and activate a manual IPv4 tunnel">`IPv4`</SwmToken> and <SwmToken path="plugins/modules/mktun.py" pos="47:9:9" line-data="        - Base64 encoding of IPv6 tunnels to be imported.">`IPv6`</SwmToken> tunnels. It requires AIX version <SwmToken path="plugins/modules/mktun.py" pos="32:6:8" line-data="- AIX &gt;= 7.1 TL3">`7.1`</SwmToken> <SwmToken path="plugins/modules/mktun.py" pos="32:10:10" line-data="- AIX &gt;= 7.1 TL3">`TL3`</SwmToken> or higher and Python <SwmToken path="plugins/modules/mktun.py" pos="33:6:8" line-data="- Python &gt;= 3.6">`3.6`</SwmToken> or higher. Additionally, it requires a privileged user with specific authorizations.

# Commands Used in MKTUN

The module uses various commands to perform its operations:

- <SwmToken path="plugins/modules/mktun.py" pos="307:2:2" line-data="def gentun(module, vopt, tun):">`gentun`</SwmToken>: Creates tunnel definitions.
- <SwmToken path="plugins/modules/mktun.py" pos="382:2:2" line-data="def lstun(module):">`lstun`</SwmToken>: Lists tunnel definitions from the tunnel database.
- <SwmToken path="plugins/modules/mktun.py" pos="191:1:1" line-data="  mktun:">`mktun`</SwmToken>: Manages tunnel configurations.
- <SwmToken path="plugins/modules/mktun.py" pos="556:10:10" line-data="    rmtun_path = module.get_bin_path(&#39;rmtun&#39;, required=True)">`rmtun`</SwmToken>: Removes tunnel configurations.
- <SwmToken path="plugins/modules/mktun.py" pos="557:10:10" line-data="    exptun_path = module.get_bin_path(&#39;exptun&#39;, required=True)">`exptun`</SwmToken>: Exports tunnel definitions.
- <SwmToken path="plugins/modules/mktun.py" pos="558:10:10" line-data="    imptun_path = module.get_bin_path(&#39;imptun&#39;, required=True)">`imptun`</SwmToken>: Imports tunnel definitions.

# Main Functions

There are several main functions in this module, including <SwmToken path="plugins/modules/mktun.py" pos="307:2:2" line-data="def gentun(module, vopt, tun):">`gentun`</SwmToken>, <SwmToken path="plugins/modules/mktun.py" pos="382:2:2" line-data="def lstun(module):">`lstun`</SwmToken>, <SwmToken path="plugins/modules/mktun.py" pos="485:2:2" line-data="def make_devices(module):">`make_devices`</SwmToken>, and <SwmToken path="plugins/modules/mktun.py" pos="500:2:2" line-data="def main():">`main`</SwmToken>. We will dive into <SwmToken path="plugins/modules/mktun.py" pos="307:2:2" line-data="def gentun(module, vopt, tun):">`gentun`</SwmToken> and <SwmToken path="plugins/modules/mktun.py" pos="382:2:2" line-data="def lstun(module):">`lstun`</SwmToken>.

<SwmSnippet path="/plugins/modules/mktun.py" line="307">

---

## gentun

The <SwmToken path="plugins/modules/mktun.py" pos="307:2:2" line-data="def gentun(module, vopt, tun):">`gentun`</SwmToken> function creates a manual tunnel definition in the tunnel database and returns the tunnel ID. It constructs a command using various tunnel options and executes it. If the command fails, it logs the error and fails the module execution.

```python
def gentun(module, vopt, tun):
    """
    Create the manual tunnel definition in the tunnel database
    with gentun and return the tunnel id.
    """
    cmd = [gentun_path, vopt, '-t', 'manual', '-s',
           tun['src']['address'], '-d', tun['dst']['address']]

    # gentun options that use lowercase letters for source and uppercase for destination
    gentun_opts = {
        'ah_algo': '-a',
        'enc_mac_algo': '-b',
        'enc_mac_key': '-c',
        'esp_algo': '-e',
        'ah_key': '-h',
        'esp_key': '-k',
        'esp_spi': '-n',
        'ah_spi': '-u'
    }
    for key, opt in gentun_opts.items():
        if tun['src'][key]:
```

---

</SwmSnippet>

<SwmSnippet path="/plugins/modules/mktun.py" line="382">

---

## lstun

The <SwmToken path="plugins/modules/mktun.py" pos="382:2:2" line-data="def lstun(module):">`lstun`</SwmToken> function lists manual tunnel definitions from the tunnel database. It constructs a command to retrieve tunnel definitions and parses the output to build a dictionary of tunnel information. If the command fails, it logs the error and fails the module execution.

```python
def lstun(module):
    """
    List manual tunnel definitions from tunnel database.

    Fields returned by lstun -O for manual tunnels:
        tunnel|source|dest|policy|dpolicy|mask|fw|emode|tunlife|
        sspia|dspia|aalgo|daalgo|sakey|dakey|
        sspie|dspie|ealgo|dealgo|sekey|dekey|
        eaalgo|deaalgo|seakey|deakey|
        replay|header
    """
    tunnels = {}
    for version in ['ipv4', 'ipv6']:
        tunnels[version] = {}

        vopt = '-v4' if version != 'ipv6' else '-v6'

        # List tunnel definitions in tunnel database
        cmd = [lstun_path, vopt, '-p', 'manual', '-O']
        rc, stdout, stderr = module.run_command(cmd)
```

---

</SwmSnippet>

# Example Usage of MKTUN

This example demonstrates how to create and activate a manual <SwmToken path="plugins/modules/mktun.py" pos="190:15:15" line-data="- name: Create and activate a manual IPv4 tunnel">`IPv4`</SwmToken> tunnel using the MKTUN module. The source and destination addresses, authentication and encryption algorithms, and other tunnel parameters are specified.

<SwmSnippet path="/plugins/modules/mktun.py" line="190">

---

Example of creating and activating a manual <SwmToken path="plugins/modules/mktun.py" pos="190:15:15" line-data="- name: Create and activate a manual IPv4 tunnel">`IPv4`</SwmToken> tunnel using the MKTUN module.

```python
- name: Create and activate a manual IPv4 tunnel
  mktun:
    manual:
      ipv4:
        - src:
            address: 10.10.11.72
            ah_algo: HMAC_MD5
            esp_algo: DES_CBC_8
          dst:
            address: 10.10.11.98
            esp_spi: 12345
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
