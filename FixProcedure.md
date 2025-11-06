# ⚙️ FixProcedure: Restore Nested Virtualization on AMD (Windows 11 24H2+)

> **Important:** This process disables Windows security features like Credential Guard, VBS, and Core Isolation.  
> Use **only** on isolated lab or testing systems — **not on production or work devices.**

---

## 🧩 Step 1: Disable Credential Guard via Registry

**Registry Keys to Modify:**

| Key Path | Key Name | Type | Value |
|-----------|-----------|------|--------|
| `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa` | `LsaCfgFlags` | `REG_DWORD` | `0` |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\DeviceGuard` | `LsaCfgFlags` | `REG_DWORD` | `0` |

---

## 🔐 Step 2: Disable Credential Guard with UEFI Lock

Run **Command Prompt as Administrator** and execute the following:

```cmd
mountvol X: /s
copy %WINDIR%\System32\SecConfig.efi X:\EFI\Microsoft\Boot\SecConfig.efi /Y
bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader
bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"
bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}
bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO

```
## 🧱 Step 3: Disable Virtualization-Based Security (VBS)

Delete the following registry keys:

| Key Path                                                          | Key Name                            |
| ----------------------------------------------------------------- | ----------------------------------- |
| `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\DeviceGuard` | `EnableVirtualizationBasedSecurity` |
| `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\DeviceGuard` | `RequirePlatformSecurityFeatures`   |

## Step 4: Update Boot Configuration

Run Command Prompt as Administrator and execute:

```cmd
bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS
bcdedit /set vsmlaunchtype off

```
## 🧩 Step 5: Modify Group Policy

**Open Group Policy Editor (gpedit.msc):**
Computer Configuration → Administrative Templates → System → Device Guard →
→ Double-click “Turn On Virtualization-Based Security”
→ Set it to Disabled

## 🧠 Step 6: Turn Off Core Isolation

**Open Windows Security → Device Security → Core Isolation**
**→ Toggle Memory Integrity and all options Off**

## 🧱 Step 7: Disable Hyper-V Features

**Open Windows Features (OptionalFeatures.exe) and uncheck:**

 Hyper-V

 Virtual Machine Platform

 Windows Subsystem for Linux

## 🔁 Step 8: Restart Your PC

During boot, you may see a UEFI modification prompt.
Press F3, then Enter to confirm and continue.

## ✅ After reboot:
**Run the following command in Command Prompt (Admin):**

```cmd
systeminfo | find "Hyper-V"

```
**You should now no longer see A hypervisor has been detected.**
**VMware nested virtualization should be fully functional again 🎉**


