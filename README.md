# Cisco Syslog Gap Detector & Recovery Tool

A Python-based network automation tool designed to monitor Cisco IOS syslog streams in real-time, detect dropped log sequences, and automatically recover the missing logs via SSH using timestamp-based regex.

## Features
- **Real-time UDP Listener:** Captures syslog messages on a configurable UDP port.
- **Gap Detection:** Monitors incoming syslog streams to instantly detect when logs go missing.
- **Automated Recovery:** Automatically initiates an SSH connection (via Netmiko) to the offending device to fetch missing logs.
- **Dynamic Regex Generation:** Intelligently builds Cisco-compatible regular expressions based on the exact **timestamp** of the log gap to pull the missing data directly from the device's buffer.
- **Log Overflow Protection:** Implements intelligent rate-limiting to prevent console flooding. If a device sends a burst of logs (e.g., >10 logs in 5 seconds), the script temporarily suppresses further output from that IP to maintain readability and system stability during network storms.
- **Color-Coded Severity:** Visual console output based on syslog severity (Red: ≤3, Yellow: 4-5, Green: >5).
- **Persistent Logging:** Automatically saves critical logs (severity ≤3) to a dated text file for later review.

## Topology & Testing Environment

![Network Topology](topology-syslog-server.png)

- **Emulator:** **PNETLab**
- **Host Machine:** Windows PC running the Python script (acting as the Syslog Server).
- **Network Devices:** General Cisco IOS devices (Routers/Switches).
- **Log Loss Simulation:** An **ASA firewall** was utilized in the topology to intentionally simulate and trigger log drops, allowing for reliable testing of the gap-detection and recovery mechanisms.

## Prerequisites & Device Configuration
For the script to function correctly, the following must be configured:

1. **Bidirectional Reachability:** The Python server must be able to reach the network devices, and the devices must have a route back to the server (for both Syslog UDP and SSH TCP).
2. **SSH Access:** SSH must be fully configured on the target devices (domain name, crypto keys, VTY lines configured for login local).
3. **Syslog Configuration:** Devices must be configured to send logs to the Python host over UDP.
   ```cisco
   service timestamps log datetime msec
   logging host <IP_OF_PYTHON_HOST> transport udp port <DESIRED_PORT>
   logging trap debugging
   
1.ensure your PNETLab devices are configured to send syslog to the host machine and that SSH is reachable.
2.Run the script:   python final-syslog-git.py
3.The script will begin listening. Press Ctrl+C to stop the listener and initiate the gap-recovery process for any detected missing logs.
4.Follow the interactive prompts to confirm the generated timestamp regex and provide your SSH credentials.

**IMPORTANT**: 
For the best experience, verify connectivity and SSH access to your devices BEFORE running the program.
All required third-party libraries are listed in the import statements at the top of the script
Make sure the file path is valid ( where you save your important logs ) 



Built by SLIMANE ABDELHAK
