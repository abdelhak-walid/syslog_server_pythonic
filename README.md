# syslog_server_pythonic
A Python-based syslog monitoring tool for Cisco IOS. Listens on UDP 1514, detects dropped log sequences in real-time, prevents console overflow via rate-limiting, and uses Netmiko to SSH into devices and fetch missing logs via dynamic regex.
