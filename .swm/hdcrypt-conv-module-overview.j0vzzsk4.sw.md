---
title: HDCrypt Conv Module Overview
---
# Overview

HDCrypt Conv is a module used to convert logical or physical volumes into encrypted ones and vice versa. It supports operations such as encrypting and decrypting logical volumes, physical volumes, and volume groups. The module requires specifying the action to perform (encrypt or decrypt) and the devices to be targeted. A password is required for both encryption and decryption processes, which must also be encrypted.

# Usage of HDCrypt Conv

The <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="81:5:5" line-data="  ibm.power_aix.hdcrypt_conv:">`hdcrypt_conv`</SwmToken> module is used to encrypt or decrypt logical and physical volumes. It requires specifying the action (encrypt or decrypt) and the devices to be targeted. A password is also required for the <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="68:11:13" line-data="    - Specifies the password for encryption/decryption.">`encryption/decryption`</SwmToken> process.

<SwmSnippet path="/plugins/modules/hdcrypt_conv.py" line="80">

---

This example demonstrates how to convert a logical volume (LV) named <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="80:11:11" line-data="- name: &quot;convert LV (testlv) to encrypted LV&quot;">`testlv`</SwmToken> to an encrypted LV using the <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="82:4:4" line-data="    action: encrypt">`encrypt`</SwmToken> action and providing the necessary password.

```python
- name: "convert LV (testlv) to encrypted LV"
  ibm.power_aix.hdcrypt_conv:
    action: encrypt
    device:
      lv: testlv
    password: abc
```

---

</SwmSnippet>

# Main Functions

There are several main functions in this module. Some of them are <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="271:2:2" line-data="def encrypt_lv(module, name):">`encrypt_lv`</SwmToken>, <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="316:2:2" line-data="def decrypt_lv(module, name):">`decrypt_lv`</SwmToken>, <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="360:2:2" line-data="def encrypt_pv(module, name):">`encrypt_pv`</SwmToken>, and <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="392:2:2" line-data="def decrypt_pv(module, name):">`decrypt_pv`</SwmToken>. We will dive a little into <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="271:2:2" line-data="def encrypt_lv(module, name):">`encrypt_lv`</SwmToken> and <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="316:2:2" line-data="def decrypt_lv(module, name):">`decrypt_lv`</SwmToken>.

## <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="271:2:2" line-data="def encrypt_lv(module, name):">`encrypt_lv`</SwmToken>

The <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="271:2:2" line-data="def encrypt_lv(module, name):">`encrypt_lv`</SwmToken> function is used to encrypt a logical volume. It first checks if encryption is enabled on the volume group that the logical volume belongs to and enables it if not. Then, it uses the appropriate command to encrypt the logical volume based on the strength of the provided password.

<SwmSnippet path="/plugins/modules/hdcrypt_conv.py" line="271">

---

The <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="271:2:2" line-data="def encrypt_lv(module, name):">`encrypt_lv`</SwmToken> function checks if encryption is enabled on the volume group and enables it if not. It then uses the appropriate command to encrypt the logical volume based on the strength of the provided password.

```python
def encrypt_lv(module, name):
    """
    Encrypts the Logical Volume it is passed
    arguments:
        module: Ansible module argument spec.
        name: Name of the logical volume to encrypt
    note:
        If the volume group that the logical volume belongs to is not encryption enabled, it is first encryption enabled.
    return:
        None
    """
    password = module.params['password']
    vg_name = get_vg_name(module, name)

    # Enable Encryption if not already enabled on the VG
    vg_encrypt_enabled(module, vg_name)

    if crypto_status == "uninitialized":
        if not check_password_strength(password):
            cmd = expectPrompts['authinit_weak_pwd'] % (name, password, password)
        else:
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="316:2:2" line-data="def decrypt_lv(module, name):">`decrypt_lv`</SwmToken>

The <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="316:2:2" line-data="def decrypt_lv(module, name):">`decrypt_lv`</SwmToken> function is used to decrypt a logical volume. It first unlocks the logical volume using the provided password and then uses the appropriate command to decrypt the logical volume.

<SwmSnippet path="/plugins/modules/hdcrypt_conv.py" line="316">

---

The <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="316:2:2" line-data="def decrypt_lv(module, name):">`decrypt_lv`</SwmToken> function unlocks the logical volume using the provided password and then uses the appropriate command to decrypt the logical volume.

```python
def decrypt_lv(module, name):
    """
    Decrypts the Logical Volume it is passed
    arguments:
        module: Ansible module argument spec.
        name: Name of the logical volume to decrypt.
    return:
        None
    """
    global convert_failed

    password = module.params['password']
    cmd = expectPrompts['unlock'] % (name, password)
    rc, stdout, stderr = module.run_command(cmd)
    result['stdout'] = stdout
    result['stderr'] = stderr
    result['cmd'] = cmd
    if "3020-0125" in stdout:
        result['msg'] += f"Password to decrypt {name} was incorrect.\n"
        convert_failed = True
        return
```

---

</SwmSnippet>

## <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="360:2:2" line-data="def encrypt_pv(module, name):">`encrypt_pv`</SwmToken>

The <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="360:2:2" line-data="def encrypt_pv(module, name):">`encrypt_pv`</SwmToken> function is used to encrypt a physical volume. It uses the appropriate command to encrypt the physical volume based on the strength of the provided password.

## <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="392:2:2" line-data="def decrypt_pv(module, name):">`decrypt_pv`</SwmToken>

The <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="392:2:2" line-data="def decrypt_pv(module, name):">`decrypt_pv`</SwmToken> function is used to decrypt a physical volume. It first unlocks the physical volume using the provided password and then uses the appropriate command to decrypt the physical volume.

<SwmSnippet path="/plugins/modules/hdcrypt_conv.py" line="392">

---

The <SwmToken path="plugins/modules/hdcrypt_conv.py" pos="392:2:2" line-data="def decrypt_pv(module, name):">`decrypt_pv`</SwmToken> function unlocks the physical volume using the provided password and then uses the appropriate command to decrypt the physical volume.

```python
def decrypt_pv(module, name):
    '''
    Decrypts the physical volume that is passed.

    arguments:
        module (dict): Ansible module argument spec.
        name (str) : Name of the PV that needs to be decrypted.

    returns:
        None
    '''

    password = module.params['password']

    cmd = expectPrompts['unlock'] % (name, password)

    rc, stdout, stderr = module.run_command(cmd)
    result['stdout'] = stdout
    result['stderr'] = stderr
    result['cmd'] = cmd
    result['rc'] = rc
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
