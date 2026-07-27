# enterprise-network-security-lab
Created by GNS3
# Enterprise Multi-VLAN Infrastructure & Security Simulation

โครงการจำลองสถาปัตยกรรมระบบเครือข่ายและระบบความปลอดภัยระดับองค์กรด้วย GNS3 โดยเน้นเรื่อง **High Availability (Redundancy)**, **Traffic Management (Load Balancing)** และ **Gateway Security**

---

## 📐 Network Architecture Overview
*(แปะรูปภาพ Topology Diagram จาก GNS3 ที่นี่)*

* **Core Layer:** Dual Open vSwitch (OVS) พร้อมการทำ **STP** (Spanning Tree Protocol) กำหนด Primary/Backup Root Bridge และ **LACP Bond** (Link Aggregation)
* **Firewall & Routing:** pfSense จัดการ Inter-VLAN Routing, Stateful Firewall, และ DNS-based Filtering (**pfBlockerNG**)
* **Load Balancer:** HAProxy กระจาย Traffic ให้กับ Backend Web Servers พร้อมกำหนด **Rate Limiting** และ **Custom Error Pages (403/503)**
* **Backend & Clients:** Lightweight Web Applications บน Linux (Ubuntu) และ Client Simulation บน Alpine Linux / Webterm

---

## 🛠️ Configuration Directory Structure

ไฟล์การตั้งค่าของอุปกรณ์ทั้งหมดใน GNS3 ถูกแยกไว้อย่างเป็นระเบียบในโฟลเดอร์ `/configs`:

1. **Core Switches (Open vSwitch):**
   * [`ovs1-core-primary.conf`](./configs/switches/ovs1-core-primary.conf) - OVS1 (STP Priority: 0x1000)
   * [`ovs2-core-backup.conf`](./configs/switches/ovs2-core-backup.conf) - OVS2 (STP Priority: 0x2000)
2. **Servers & Clients:**
   * [`backend-server.conf`](./configs/servers-clients/backend-server.conf) - Backend Web Server (Static IP + Auto-deploy Python Service)
   * [`dhcp-client.conf`](./configs/servers-clients/dhcp-client.conf) - Webterm / Alpine DHCP Configuration

---

## 🚀 How to Deployment / Reproduce Lab

1. นำไฟล์ Config ในโฟลเดอร์ `configs/` ไปแปะในช่อง **Edit config** (`/etc/network/interfaces`) ของ Node แต่ละตัวใน GNS3
2. กด **Start** อุปกรณ์ทุกตัวพร้อมกัน
3. ตัว Backend Server จะทำการติดตั้ง `python3` และเริ่มรันบริการ Web Server บน Port 80 โดยอัตโนมัติภายใน 5-10 วินาทีหลังจากระบบบูตเสร็จ
