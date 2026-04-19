# Environment Setup

This document outlines how I configured the virtual environment used for all WebGoat exercises in this repository.

---

## Overview

| Component | Details |
|-----------|---------|
| Host OS | Windows 11 |
| Virtualisation Software | VMware Workstation Pro 17 |
| Guest OS | Kali Linux 2025.4 (64-bit) |
| Target Application | WebGoat 2023.8 |
| Network Mode | NAT (isolated from external network) |

---

## Step 1: Install VMware Workstation Pro

1. Download **VMware Workstation Pro 17** from the Broadcom portal:
   > https://knowledge.broadcom.com/external/article/368667/download-and-license-vmware-desktophype.html
   
   You will need to register a free Broadcom account. Once logged in, search for *VMware Workstation Pro* under My Downloads.
   
   > ⚠️ Note: Version 25H2 has reported bugs — stick with **Pro 17** (17.6.x).

2. Run the installer and follow the setup wizard. No license key is required for the free version — select **Finish** when prompted.

---

## Step 2: Set Up Kali Linux Virtual Machine

1. Download the **Kali Linux 2025.4 VMware image (64-bit)** from the official Kali site:
   > https://cdimage.kali.org/kali-2025.4/kali-linux-2025.4-vmware-amd64.7z

2. Extract the `.7z` archive using 7-Zip or a similar tool.

3. Open VMware Workstation and select **Open Virtual Machine**. Navigate to the extracted folder and select the `.vmx` file.

4. Click **Power on this virtual machine** to start Kali Linux.

5. Log in with the default credentials:
   - **Username:** `kali`
   - **Password:** `kali`

> ⚠️ If your mouse cursor disappears after booting, select **Update this virtual machine** from the VMware prompt to resolve the compatibility issue.

---

## Step 3: Install and Run WebGoat 2023.8

> All of the following steps are performed **inside the Kali Linux virtual machine**.

1. Open the Firefox browser inside Kali and download **WebGoat 2023.8**:
   > https://github.com/WebGoat/WebGoat/releases/tag/v2023.8
   
   Download the file: `webgoat-2023.8.jar`
   
   It will be saved to `/home/kali/Downloads/` by default.

   > ⚠️ Use version **2023.8** specifically — the latest 2025 release requires a different Java version that is incompatible with the Kali Linux environment used here.

2. Open a terminal and navigate to the Downloads directory:

   ```bash
   cd Downloads
   ```

3. Start WebGoat with the following command:

   ```bash
   java -Dfile.encoding=UTF-8 -Dwebgoat.port=8080 -Dwebwolf.port=9090 -jar webgoat-2023.8.jar
   ```

4. Once the server starts, open Firefox and go to:
   > http://127.0.0.1:8080/WebGoat
   
   *(Note: this URL is case-sensitive)*

5. On first launch, click **Register yourself as a new user** and create an account.

6. You should now see the WebGoat lesson dashboard — setup is complete.

---

## Security Notes

- WebGoat is intentionally vulnerable. Always run it inside an isolated VM with NAT networking — **never on your host machine or a shared network**.
- Disconnect from the internet or keep the VM in NAT/Host-only mode while WebGoat is running.
- All exercises in this repository were conducted in this isolated environment for academic purposes only.
