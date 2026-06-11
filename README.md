# Multi-Vendor IP Video Surveillance & Network Integration Lab

## 📌 Project Overview
This repository documents a practical network engineering project focused on integrating and troubleshooting a multi-vendor IP Closed-Circuit Television (CCTV) infrastructure. The main objective of this lab was to successfully bridge compatibility, addressing, and protocol gaps between different hardware and software vendors (**Hikvision/HiLook** cameras and a **Uniarch** Network Video Recorder) within a simulated and physical testing environment.

---

## 🛠️ Infrastructure & Technologies Used
* **Network Video Recorder (NVR):** Uniarch (by Uniview)
* **IP Cameras:** HiLook / Hikvision (ONVIF Profile S compliant)
* **Discovery & Configuration Tools:** Hikvision SADP (Search Active Devices Protocol) Tool
* **Network Protocols:** ONVIF, TCP/IP, NTP (Network Time Protocol), DHCP, H.264 / H.265+ Codecs

---

## 📐 Topology & Architecture
The architecture is segmented into a local management network where cross-brand authentication, video stream encapsulation, and network addressing parameters were carefully orchestrated.

## 🚀 Key Technical Challenges & Solutions

### 1. Cross-Brand Authentication Failures (The "Incorrect Password" Bug)
* **Challenge:** When attempting to link the HiLook/Hikvision cameras to the Uniarch NVR, the system continuously reported authentication errors ("Incorrect Username or Password"), even when using the correct master credentials.
* **Solution:** Hikvision-based devices disable ONVIF integration by default for security. Using the camera's web GUI, ONVIF protocol support was explicitly enabled, and a dedicated, independent integration user was provisioned within the camera's internal user management settings specifically for the NVR connection.

### 2. Clock Synchronization & NTP Alignment
* **Challenge:** Random connection drops occurred over the ONVIF protocol due to a lack of time alignment between nodes. Security handshake algorithms (WS-Security) reject communication if time metadata differs significantly.
* **Solution:** Configured localized Network Time Protocol (NTP) syncing across all cameras and the NVR, ensuring absolute timestamp alignment down to the millisecond.

### 3. Video Stream & Codec Compatibility (H.265+)
* **Challenge:** Live feeds were experiencing intermittent freezing or failing to render on the NVR interface.
* **Solution:** Tuned the compression settings. High-efficiency codecs like **H.265+** use vendor-specific optimizations that sometimes break raw ONVIF streams. The cameras were reconfigured to standard H.264/H.265 main profiles to match the Uniarch NVR decoding capabilities perfectly.

### 4. IP Segmentation & Network Discovery
* **Challenge:** Multi-vendor components initial default IPs collided or sat in mismatched subnets out of the box.
* **Solution:** Utilized the **SADP Tool** to search active devices across Layer 2, batch-assigned cohesive static IPv4 addressing within the local subnet, and resolved IP segmentation conflicts.

---

## 📊 Verification & Results

Once all configuration barriers were resolved, the Uniarch NVR successfully mapped and decoded the video streams from the HiLook cameras via the ONVIF protocol matrix.

> 📸 *screenshots of the NVR dashboard displaying the active multi-vendor live feed here:*
> Look at the images in the other folder under the following name and format; topology.png  &  nvr_live_feed.png

---

## 🔍 Useful Verification Commands & Troubleshooting Steps
1. **Device Discovery:** Use the SADP Tool to ensure all cameras are visible over the broadcast domain.
2. **Ping Connectivity:** Verify end-to-end ICMP reachability from the management console to each IP node.
3. **ONVIF Port Auditing:** Ensure the ONVIF port (typically `80` or `8000` depending on configuration) matches between the camera setting and the NVR addition wizard.

---
