# Enhancing-Network-Security-Through-VLAN-Segmentation-A-Practical-Implementation-and-Analysis

This project demonstrates how VLAN segmentation enhances network security by isolating devices into separate VLANs, preventing unauthorized communication between nodes.

## Project Overview:
- Nmap host scans were performed from different nodes (Client, Server, Attacker 1, Attacker 2) to test network visibility.
- An HTTP server was deployed on the Server node, and connectivity was tested using a client script.
- VLAN configuration was used to isolate traffic, restricting communication between unauthorized devices.

## Technologies Used:
- Core Network Emulator for network simulation.
- Nmap for network scanning.
- Wireshark for packet analysis.
- VLAN configuration.
- Bash scripting for automation.

## How to Use:
1. Clone this repository.
2. Load the `Scenario.xml` file in the CORE Network Emulator for network simulation. 
3. Follow the steps in `DOCUMENTATION.md` to reproduce the experiment.

## Project Structure:
- `README.md`: Overview of the project.
- `DOCUMENTATION.md`: Detailed documentation of the project, including steps and configurations.
- `start.sh`: Script to start the HTTP server.
- `run_curl.sh`: Script to test server-client connectivity.
- `Scenario.xml`: Script to setup the scenario in the CORE Network Emulator.
