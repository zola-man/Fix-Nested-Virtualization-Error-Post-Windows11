# 🌀 Fixing Nested Virtualization Error on AMD Systems (Post Windows 11 Update)

> “VMware Workstation does not support nested virtualization on this host.  
> Module 'HV' power on failed. Failed to start the virtual machine.”  
> The line that ruined your lab day 😩  

---

## ⚡ Overview

If you’ve landed here, chances are your **VMware Workstation** suddenly refuses to boot VMs after a Windows 11 update, even though you’ve already disabled **Hyper-V**, tweaked **Group Policy**, edited the **Registry**, and even sacrificed a few hours of sleep.  

Well, you’re not alone.  

This issue started surfacing after Microsoft silently re-enabled **Device Guard**, **Core Isolation**, and **Virtualization-Based Security (VBS)** features during boot, even when you thought they were *off*. These features protect you from malicious code that abuses virtualization… but they also break your virtual labs.  

This repo is your friendly escape hatch 🧰 — built especially for **AMD users** (Intel folks, your standard fixes still apply).

---

## 🧠 What’s Happening Behind the Scenes

Hyper-V Requirements: A hypervisor has been detected
and refuse to start nested VMs.  

This behavior is tied to **Device Guard**, **Memory Integrity**, and **VBS**, which reserve hypervisor resources during boot time. For researchers, lab testers, and ethical hackers. Well, that’s a nightmare.

---

## 🧩 What This Repository Provides

- Step-by-step instructions to **fully disable VBS, Device Guard, and Core Isolation**
- PowerShell & Registry scripts tailored for **AMD** processors
- A reversible process (so you can re-enable security features later)
- Explanations of *why* each step matters (not just copy–paste commands)
- Safety disclaimers and optional checks before applying changes

---

## 🚨 Important Warning

This method **reduces system security** by disabling certain virtualization protections.  
It’s meant **only for isolated testing environments** — *not production machines or work devices!*  

> ⚠️ Proceed at your own risk.  
> This repo is designed for developers, cybersecurity learners, and lab testers who need their nested virtualization environments functional again.

---

## 💡 Who This Helps

- VMware / EVE-NG / GNS3 / Android Emulator users  
- Cybersecurity learners setting up **virtual labs**  
- Researchers working with **nested ESXi** or **Kali-in-VM** setups  
- Anyone getting that dreadful “Module ‘HV’ power on failed” error on AMD CPUs

---

## 🧭 Next Steps

1. **Follow the execution procedure** (coming next in the guide)
2. **Reboot**
3. **Run your lab** and smile again 😎  

---

## 💬 Final Thoughts

This project exists because some of us just want to get back to *building labs* without fighting the OS.  
If you’re one of those who said, “I’ve already disabled everything and it still doesn’t work,” — this repo is for you.  

---

### 🧑‍💻 Author
**Zelalem Adugnaw**  
Cyber Security Engineer | Researcher  
Have Fun :)
---

### ⭐ Contribute & Support
If this saved your sanity, give it a ⭐ and share with your fellow lab warriors!  
Let’s make virtualization great again 🖥️✨  

---


Windows now boots with hidden virtualization layers active (even when Hyper-V is "off"), making VMware detect:
