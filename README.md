# Configure-and-Verify-a-Site-to-Site-IPsec-VPN-Using-CLI-in-Packet-Tracer

Project Overview
This project focused on configuring a site-to-site IPsec VPN between two Cisco routers (R1 and R3) with R2 acting as a pass-through. The objective was to establish secure communication between the LANs of R1 and R3, ensuring the confidentiality and integrity of transmitted data.
Business Understanding
Implementing a site-to-site IPsec VPN allows organizations to securely connect different sites over the internet or other untrusted networks. This enhances data security, protects sensitive information, and ensures reliable communication between remote locations.
Tools and Frameworks Used
•	Packet Tracer: A network simulation tool for configuring Cisco devices.
•	Cisco IOS Commands: Used for router configuration.
Project Description
The project was executed in several steps, from verifying network connectivity to configuring and verifying the IPsec VPN tunnel. Below is a detailed breakdown of the tasks performed:
Topology and Addressing Table:
•	Configured routers (R1, R2, R3) and PCs, ensuring proper IP addressing and connectivity.
Objectives and Configuration Steps:
Part 1: Configure IPsec Parameters on R1
Step 1: Test Connectivity
•	Verified network connectivity by pinging from PC-A (192.168.1.3) to PC-C (192.168.3.3).
Step 2: Enable the Security Technology Package
•	Verified the security package and enabled it if necessary:
R1(config)# license boot module c1900 technology-package securityk9
•	Accepted the end-user license agreement and reloaded the router.
Step 3: Identify Interesting Traffic
•	Configured ACL 110 to permit traffic between the LANs on R1 and R3, triggering the VPN:
R1(config)# access-list 110 permit ip 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255
Step 4: Configure the IKE Phase 1 ISAKMP Policy
•	Configured ISAKMP policy parameters on R1:
R1(config)# crypto isakmp policy 10
R1(config-isakmp)# encryption aes 256
R1(config-isakmp)# authentication pre-share
R1(config-isakmp)# group 5
R1(config-isakmp)# exit
R1(config)# crypto isakmp key vpnpa55 address 10.2.2.2
Step 5: Configure the IKE Phase 2 IPsec Policy
•	Created a transform set and crypto map:
R1(config)# crypto ipsec transform-set VPN-SET esp-aes esp-sha-hmac
R1(config)# crypto map VPN-MAP 10 ipsec-isakmp
R1(config-crypto-map)# description VPN connection to R3
R1(config-crypto-map)# set peer 10.2.2.2
R1(config-crypto-map)# set transform-set VPN-SET
R1(config-crypto-map)# match address 110
Step 6: Configure the Crypto Map on the Outgoing Interface
•	Bound the VPN-MAP crypto map to the outgoing Serial 0/0/0 interface:
R1(config)# interface s0/0/0
R1(config-if)# crypto map VPN-MAP
________________________________________
Part 2: Configure IPsec Parameters on R3
Step 1: Enable the Security Technology Package
•	Verified and enabled the security package on R3, similar to R1.
Step 2: Configure Interesting Traffic
•	Configured ACL 110 on R3 for traffic from its LAN to R1:
R3(config)# access-list 110 permit ip 192.168.3.0 0.0.0.255 192.168.1.0 0.0.0.255
Step 3: Configure IKE Phase 1 ISAKMP Properties
•	Set up ISAKMP policy parameters on R3:
R3(config)# crypto isakmp policy 10
R3(config-isakmp)# encryption aes 256
R3(config-isakmp)# authentication pre-share
R3(config-isakmp)# group 5
R3(config-isakmp)# exit
R3(config)# crypto isakmp key vpnpa55 address 10.1.1.2
Step 4: Configure IKE Phase 2 IPsec Policy
•	Created the transform set and crypto map on R3:
R3(config)# crypto ipsec transform-set VPN-SET esp-aes esp-sha-hmac
R3(config)# crypto map VPN-MAP 10 ipsec-isakmp
R3(config-crypto-map)# description VPN connection to R1
R3(config-crypto-map)# set peer 10.1.1.2
R3(config-crypto-map)# set transform-set VPN-SET
R3(config-crypto-map)# match address 110
Step 5: Configure the Crypto Map on the Outgoing Interface
•	Bound the VPN-MAP crypto map to the outgoing Serial 0/0/1 interface:
R3(config)# interface s0/0/1
R3(config-if)# crypto map VPN-MAP
________________________________________
Part 3: Verify the IPsec VPN
Step 1: Verify the Tunnel Prior to Interesting Traffic
•	Checked the status of the IPsec tunnel:
R1# show crypto ipsec sa
•	Confirmed that packets encapsulated, encrypted, decapsulated, and decrypted were all 0.
Step 2: Create Interesting Traffic
•	Initiated a ping from PC-A to PC-C.
Step 3: Verify the Tunnel After Interesting Traffic
•	Re-checked the IPsec status on R1, noting that the packet counts had increased, indicating the tunnel was operational.
Step 4: Create Uninteresting Traffic
•	Pinged PC-B from PC-A and noted that the IPsec statistics remained unchanged, confirming that uninteresting traffic was not encrypted.
Step 5: Verify Results
•	Completed all tasks and verified a 100% completion percentage through the Packet Tracer interface.
________________________________________
Conclusion
The successful configuration of a site-to-site IPsec VPN between R1 and R3 illustrated the ability to securely transmit data across untrusted networks. This project demonstrated the necessary steps to establish and verify a VPN connection, ensuring data integrity and confidentiality between remote locations.


