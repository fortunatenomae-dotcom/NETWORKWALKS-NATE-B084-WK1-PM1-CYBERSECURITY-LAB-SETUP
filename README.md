# Cybersecurity Lab Environment Setup (Week 1 - Project Module 1)

Building an isolated virtual lab for penetration testing and ethical hacking practice.

## 📌 Project Overview
This project establishes a foundational cybersecurity testing lab environment hosted on Oracle VirtualBox. The lab uses Kali Linux as the primary attacker machine and is placed inside an isolated private NAT Network (`10.0.0.0/24`) designed for safe security assessments, vulnerability research, and penetration testing simulations.

## ⚙️ Specifications & Architecture
* **Hypervisor:** Oracle VirtualBox 7.x
* **Attacker Machine:** Kali Linux (Debian 64-bit)
* **Network Mode:** VirtualBox NAT Network
* **Network Subnet:** `10.0.0.0/24`
* **Default Gateway:** `10.0.0.1`
* **Kali Static IP:** `10.0.0.2/24`
* **DNS Resolver:** `8.8.8.8` / `10.0.0.1`
* **Host-VM Integration:** Bidirectional Shared Clipboard, Drag-and-Drop, and Auto-mounted Host Downloads Folder (`/media/sf_Downloads`)

---

## 🛠️ Step-by-Step Implementation

### Phase 1: Environment & Hypervisor Setup
1. Installed **7-Zip** for unpacking large virtual machine archives.
2. Installed **Oracle VirtualBox 7.x** on the Windows host.
3. Created a dedicated **NAT Network** named `NatNetwork` with IPv4 prefix `10.0.0.0/24` and DHCP enabled.

### Phase 2: Kali Linux VM Deployment & VM Settings
1. Downloaded and extracted the pre-built Kali Linux VirtualBox appliance.
2. Imported the VM into VirtualBox Manager.
3. Configured VM Features:
   * **Shared Clipboard & Drag'n'Drop:** Enabled (*Bidirectional*).
   * **Network Adapter 1:** Attached to `NAT Network` (`NatNetwork`) with Promiscuous Mode set to `Allow All`.
   * **Shared Folders:** Auto-mounted the host Windows `Downloads` folder to `/media/sf_Downloads`.

### Phase 3: Static Network Configuration & Connectivity Testing
1. Booted Kali Linux (`kali` / `kali`).
2. Configured static IPv4 address using NetworkManager:
   * **IP Address:** `10.0.0.2`
   * **Netmask:** `24` (`255.255.255.0`)
   * **Gateway:** `10.0.0.1`
   * **DNS:** `8.8.8.8`
3. Verified internet routing and DNS resolution using terminal diagnostics (`ip a`, `ping 10.0.0.1`, and `curl -I https://www.google.com`).

### Phase 4: Disaster Recovery & Snapshots
* Created a clean baseline snapshot (`Clean Lab Setup - Wk1`) to allow instant state recovery during future exploit testing.

---

## 🔧 Troubleshooting Encountered & Solutions
* **Issue 1 - NetworkManager IP persistence:** After saving the static IP in the GUI, the interface remained on DHCP (`10.0.0.3`). Resolved by deactivating and reactivating the connection using:
  ```bash
  sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
  sudo nmcli connection down "Wired connection 1"
  sudo nmcli connection up "Wired connection 1"
  Issue 2 - Subnet Mask Typo: Accidentally entered /2 netmask which modified the broadcast to 63.255.255.255. Corrected via nmcli to /24 (10.0.0.255 broadcast).
   ## 📸 Lab Screenshots
   <img width="1280" height="800" alt="VirtualBox_kali-linux-2026 3-virtualbox-amd64_26_10_2026_21_27_15" src="https://github.com/user-attachments/assets/a72f6698-31eb-4486-b03a-cca6e7e2b676" />
<img width="1920" height="1080" alt="Screenshot (8)" src="https://github.com/user-attachments/assets/46634838-a7c6-41c2-ab9c-b36b49b4d800" />
<img width="1920" height="1080" alt="Screenshot (7)" src="https://github.com/user-attachments/assets/ce762819-df1a-4492-b363-7794810de194" />
<img width="1920" height="1080" alt="Screenshot (6)" src="https://github.com/user-attachments/assets/c009688a-c840-4cdd-b741-6d386b183255" />
<img width="1920" height="1080" alt="Screenshot (2)" src="https://github.com/user-attachments/assets/7a43bdc1-fe1d-4e9c-b371-f780bbda07f5" />
<img width="1920" height="1080" alt="Screenshot (1)" src="https://github.com/user-attachments/assets/261e4fd8-06a2-40a9-917e-11ba19b879a9" />
<img width="1280" height="800" alt="VirtualBox_kali-linux-2026 3-virtualbox-amd64_26_10_2026_21_27_15" src="https://github.com/user-attachments/assets/6e3ba655-46b2-4249-8864-06395b65d1ed" />
