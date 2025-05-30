# 🛠️ PROJECT TITLE: **Internal Network Discovery Using Nmap**


## 🎯 OBJECTIVES

Use Nmap to discover live hosts on my network and identify their open ports for basic enumeration.



## 🧰 TOOLS USED

* **Nmap**: For scanning and discovering hosts
* **VMware**: Testing environment
* **Kali Linux**: Scanning OS



## 🌐 NETWORK SETUP

* **Mode**: `Bridged`

> *Reason*: NAT mode prevented the VM from accessing other devices on my home Wi-Fi. Bridged mode allowed the VM to act like a separate device on the same network.

* **VM IP**: `192.168.0.192`
* **Host IP**: `192.168.0.156`
* **Target Devices**:

  * iPhone: `192.168.0.169`
  * Mystery Device: `192.168.0.150`



## 🪜 STEP-BY-STEP PROCESS

1. **Identify VM IP Address**

   ```bash
   ip a
   ```

   > Used this instead of `ifconfig` to avoid unnecessary noise.

2. **Confirm Network Range**

   ```bash
   ip route
   ```

   **Output:**

   ```
   default via 192.168.0.1 dev eth0 proto dhcp src 192.168.0.192 metric 100
   192.168.0.0/24 dev eth0 proto kernel scope link src 192.168.0.192 metric 100
   ```

3. **Scan for Live Hosts in the Subnet**

   ```bash
   nmap -sn 192.168.0.0/24
   ```

   > Initially used `192.168.0.1/24` which missed a device. Updating to `192.168.0.0/24` caught all active devices.



## ❗ CHALLENGES FACED

### 🧩 Challenge 1: Device Not Showing in Scan

> My iPhone wasn't detected in the initial scan.

* ✅ Checked Wi-Fi and connection status
* ✅ Confirmed the IP manually
* ✅ Scanned directly with:

  ```bash
  nmap 192.168.0.169
  ```
* ✅ Realized I was using the wrong subnet start (`192.168.0.1` instead of `192.168.0.0`)

### 🧱 Challenge 2: Ports Were “Ignored” or “Filtered”

* Used individual scans and `nmap -p-` to check port response
* 🧪 Later planned to check firewall rules or test with `--reason` flag



## 🧠 WHY I DID IT THIS WAY

| Decision                 | Reason                                                             |
| ------------------------ | ------------------------------------------------------------------ |
| Used `-sn` ping scan     | To detect live hosts without scanning ports. Faster and stealthier |
| Didn’t use `-sV` or `-A` | Too noisy and unnecessary at this phase                            |
| Scanned `/24` range      | Most home networks use this subnet size; covers 254 hosts          |



## 📊 RESULTS

* **Scan Command**: `nmap -sn 192.168.0.0/24`
* **Scan Date**: 2025-05-30
* **Total Devices Found**: 3 (excluding host + VM)

| IP Address      | MAC Vendor / Hostname     | Notes                                |
| --------------- | ------------------------- | ------------------------------------ |
| `192.168.0.1`   | Guangzhou Tozed Kangwei   | MTN Router, Ports: 53, 80, 443, 5555 |
| `192.168.0.169` | Apple Inc. (iPhone)       | My Personal iPhone                   |
| `192.168.0.150` | Intel Corp. (No Hostname) | Mystery Device (Brother’s PC)        |



## 📚 WHAT I LEARNED

* Difference between `-sn`, `-sS`, and `-sV`
* How to accurately identify devices on a subnet
* Importance of choosing the right subnet range
* First-hand experience documenting real-world problems



## 🔍 OBSERVATIONS

### 📡 Router - `192.168.0.1`

* **Port 80 & 443** → Likely used for Web Admin Interface
* **Port 53** → Internal DNS Resolution
* **Port 5555/7777** → Possibly remote management (could be worth investigating)

### 📱 iPhone - `192.168.0.169`

* No open ports (expected)
* Identified via MAC

### 🖥️ Mystery Device - `192.168.0.150`

* No hostname
* Detected MAC suggests it's a PC



## 💬 REFLECTIVE SUMMARY

> **How did it feel?**
> Felt good overall—I've used Nmap before, but this time I did it with a purpose and a plan.

> **What frustrated me?**
> Not seeing all devices in the scan threw me off at first. Turns out it was a subnet start issue.

> **What am I proud of?**
> That I finally completed and documented this properly. Not just screenshots—actual thinking and reflection.

