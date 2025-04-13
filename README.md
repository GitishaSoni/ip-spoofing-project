# 🛡️ IP Spoofing Project using Kali Linux, Scapy & Wireshark

This project demonstrates **IP Spoofing** using a **forged source IP address** sent from a Kali Linux virtual machine, and analyzed in Wireshark on a Windows host. It shows how a packet appears to originate from a fake IP address, but the original **MAC address reveals the true source**, proving IP spoofing.

---

## 📦 Tools & Technologies

- 🐧 Kali Linux (Virtual Machine)
- 🪟 Windows (Host)
- 🐍 Scapy (Python Library)
- 🔍 Wireshark (Packet Sniffer)
- 🌐 VMware Workstation Player

---

## 🧩 Step-by-Step Setup & Execution

---

## 1️⃣ Installing Required Tools

### 🪟 On Windows (Host)

1. **Install VMware**:  
   Download and install from [VMware Workstation Player](https://www.vmware.com/products/workstation-player.html)

2. **Download Kali Linux ISO**:  
   Get it from [https://www.kali.org/get-kali/#kali-platforms](https://www.kali.org/get-kali/#kali-platforms)

3. **Install Wireshark**:  
   From [https://www.wireshark.org/download.html](https://www.wireshark.org/download.html)

---

### 🐧 In Kali Linux (VM)

1. Open the terminal and run:

   ```bash
   sudo apt update
   sudo apt install python3-pip net-tools
   pip3 install scapy
2. Check Kali's IP & MAC:

bash
Copy code

### Network Configuration (in VMware)
1. Go to VMware Player → Right-click your Kali VM → Settings

2. Go to Network Adapter

3. Select Host mode to share the network with your host (Windows)

4. Start the Kali VM

### Verify Network Connection
 Check Windows IP:
> ipconfig
Look for the IPv4 Address (e.g., 192.168.1.2)

Check Kali IP: (in kali's terminal)
> ip a

***Ping Test:***
> ping <Windows-IP>

### Sending Spoofed Packet Using Scapy
1. In Kali's terminal enter,
> sudo python3

Paste the following code (line-by-line):

> from scapy.all import *
> packet = IP(src="1.2.3.4", dst="192.168.75.100") / TCP(dport=80, flags="S")
> send(packet)

**Sending Multiple Spoofed Packets**
for i in range(10):
    packet = IP(src=f"1.2.3.{i}", dst="192.168.75.1") / TCP(dport=80, flags="S")
    send(packet)

###  Capture in Wireshark (on your device)

1. Open Wireshark

2. Select the correct interface (usually named VMware Network Adapter, Ethernet, or Wi-Fi)

3. In filter bar, type:
   > ip.src == 192.168.1.100(your window's ip)

4. Press Enter
5. If you see a packet from spoofe IP:
   > Right-click → Follow TCP Stream OR
   > Expand Ethernet II and Internet Protocol
6. Note the MAC Address of the sender — it will be Kali's real MAC address, even though the IP is spoofed.

### Verifying the Spoof
On Kali:
> ip link show

###  On Wireshark:
Compare the Ethernet Source MAC address from the captured packet to Kali's MAC.
If they match but IP is different, IP spoofing is successfully demonstrated.

### Conclusion
In this project, we sent a packet from Kali Linux with a spoofed IP address (192.168.1.100). The Windows machine received this packet, believing it came from that IP. However, analyzing it in Wireshark revealed that the MAC address belonged to Kali, proving that:

IP spoofing works at the network layer (IP).

The data link layer (MAC) still exposes the actual origin.
