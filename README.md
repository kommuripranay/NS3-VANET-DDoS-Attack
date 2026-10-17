# Adaptive RSU Overload Mitigation Under DDoS Attacks in VANET Using NS-3

A simulation-based study of DDoS attacks against Roadside Units (RSUs) in a Vehicular Ad Hoc Network (VANET), implemented using NS-3 and C++.

The project models a highway VANET with normal vehicle communication and multiple attacker vehicles targeting RSU 2 with high-rate UDP traffic. An adaptive mitigation mechanism monitors RSU traffic and restricts identified attack traffic when sustained overload is detected.

## Project Overview

Vehicular Ad Hoc Networks (VANETs) rely on communication between vehicles and roadside infrastructure such as RSUs. A DDoS attack can generate excessive traffic towards an RSU and degrade its ability to serve legitimate vehicles.

This project studies the behaviour of a highway-based VANET under normal and DDoS attack conditions and introduces a lightweight RSU-side mitigation mechanism.

The simulation uses:

- 40 vehicles by default
- 3 fixed RSUs
- IEEE 802.11p wireless communication
- IPv4
- AODV routing
- UDP-based communication
- Highway mobility
- Multiple DDoS attacker vehicles
- Adaptive RSU overload detection and mitigation

## Simulation Scenario

The highway contains three RSUs positioned along the road.

Normal vehicles communicate with the nearest RSU according to their current position. Selected attacker vehicles generate high-rate UDP traffic directly towards RSU 2.

### Normal Traffic

- Source port: `9001`
- Destination port: `9002`
- Packet size: 100 bytes
- Transmission interval: approximately 0.5–1.5 seconds

### Attack Traffic

- Source port: `9003`
- Destination port: `9002`
- Default attackers: 3
- Attack rate: approximately 150 packets/s per attacker
- Aggregate attack rate: approximately 450 packets/s
- Attack duration: 20–40 seconds

## Adaptive Mitigation

RSU 2 monitors its incoming traffic using one-second observation windows.

The default overload parameters are:

```text
Overload threshold: 60 packets/s
Consecutive windows: 2

When the packet rate exceeds 60 packets/s for two consecutive monitoring windows, sustained overload is detected.
The mitigation mechanism then:
1. Activates RSU-side attack traffic control.
2. Identifies attack traffic using its configured source port.
3. Drops identified attack packets.
4. Continues accepting legitimate traffic.
5. Continues monitoring the RSU.
6. Disables mitigation when traffic returns to the configured normal range.
The mechanism is intentionally lightweight and does not use machine learning or a separate intrusion-detection system.
Simulation Phases
The simulation is divided into four phases:
- Phase A — Normal Traffic: Vehicles communicate normally.
- Phase B — DDoS Attack: Selected vehicles generate high-rate traffic towards RSU 2.
- Phase C — RSU Failure: The baseline experiment can take RSU 2 offline after the attack.
- Phase D — Recovery: RSU 2 is restored and communication resumes.
Running the Simulation
The project is designed to run with NS-3.
Example:
python3.12 ./ns3 run "vanet_highway --N=40 --runTime=60 --attackStart=20 --attackEnd=40"

To enable adaptive mitigation:
python3.12 ./ns3 run "vanet_highway --N=40 --runTime=60 --attackStart=20 --attackEnd=40 --enableMitigation=true"

The main parameters can also be modified from the command line:
--N
--runTime
--attackStart
--attackEnd
--enableMitigation
--overloadPps
--overloadConsec

Output
The simulation can generate:
- Packet reception statistics
- CSV traffic measurements
- PCAP packet traces
- NetAnim XML output
- Console output showing RSU traffic rates and mitigation events
Example mitigation output:
MITIGATION ENABLED at RSU ...
MITIGATION: dropped attack packet ...
MITIGATION DISABLED at RSU ...

Repository Structure
NS3-VANET-DDoS-Attack/
├── README.md
├── highway_topology.png
└── vanet_highway.cc

Objectives
1. Study the effect of DDoS traffic on an RSU in a VANET.
2. Monitor RSU traffic using packet-rate measurements.
3. Detect sustained RSU overload.
4. Implement lightweight adaptive attack-traffic mitigation.
5. Preserve legitimate communication during mitigation.
6. Compare network behaviour before and after mitigation.
Technologies
- C++
- NS-3
- IEEE 802.11p
- AODV
- IPv4
- UDP
- VANET simulation
