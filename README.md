# Troubleshooting: Enabling Communication Between Two Computers on the Same Network

## Objective
Ensure that two computers on the same local network can communicate with each other by pinging each other's IP address.

---

## Steps Taken

### 1. **Check IP Configuration**
   - On both computers, run `ipconfig` to ensure the following:
     - Both computers are on the **same subnet** (e.g., `192.168.x.x`).
     - Both computers have the same **subnet mask** (e.g., `255.255.255.0`).
     - The **default gateway** matches the router’s IP (e.g., `192.168.x.x`).

### 2. **Disable Windows Firewall Temporarily**
   - On both computers, disable the firewall to ensure no blocking of ICMP (ping) traffic.
   - Run the following command in the command prompt on both computers:
     ```bash
     netsh advfirewall set allprofiles state off
     ```

### 3. **Verify Connectivity**
   - Run `ping <IP address>` to test if the computers can communicate. Initially, both computers were unable to ping each other, but they could successfully ping the router.

### 4. **Check ARP Table**
   - Run the following command on both computers to check if they see each other’s IP address and MAC address:
     ```bash
     arp -a
     ```
   - Both computers showed each other's IP and MAC, confirming they were aware of each other on the network.

### 5. **Enable ICMP (Ping) Requests**
   - On both computers, run the following command to ensure that ICMP requests are enabled:
     ```bash
     netsh advfirewall firewall add rule name="Allow ICMPv4" protocol=ICMPv4:8,any dir=in action=allow
     ```

### 6. **Check Network Profile**
   - Ensure the network profile on both computers is set to **Private**, as **Public** profiles block local device communication.
   - Go to **Settings > Network & Internet > Ethernet/Wi-Fi > Properties**, and ensure the connection is set to **Private**.

### 7. **Router Configuration**
   - Ensure **Client Isolation** or **AP Isolation** is disabled in the router settings. This setting can block communication between devices on the same network.
   - Confirm that **MAC filtering** is not blocking either device.

### 8. **Flush DNS and Reset Network Stack**
   - Run the following commands on both computers to reset the network stack:
     ```bash
     netsh winsock reset
     netsh int ip reset
     ipconfig /flushdns
     ipconfig /release
     ipconfig /renew
     ```

### 9. **Re-enable Windows Firewall**
   - Once the pinging issue was resolved, re-enable the Windows firewall on both computers:
     ```bash
     netsh advfirewall set allprofiles state on
     ```

### 10. **Test After Re-enabling Firewall**
   - After re-enabling the firewall, test the ping again to ensure the connection still works and that the firewall rules allow ICMP traffic.

---

## Conclusion
After performing these steps, both computers were able to successfully ping each other and communicate over the same local network. Firewalls were re-enabled with ICMP traffic explicitly allowed to ensure security while maintaining connectivity.

---
