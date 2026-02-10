# OpenEPC

OpenEPC is an open-source implementation of key **3GPP LTE Evolved Packet Core (EPC)** network functions. 
This project focuses on **core-network signalling, policy control, and data-plane gateways**, 
while remaining interoperable with external LTE access networks and IMS systems.

OpenEPC is designed for **research, experimentation, and learning**, with an emphasis on standards-based interfaces and modular design.

---

## Implemented EPC Network Functions

OpenEPC includes the following EPC components:

- **HSS** – Home Subscriber Server  
- **MME** – Mobility Management Entity  
- **SGW** – Serving Gateway  
- **PGW** – Packet Data Network Gateway  
- **PCRF** – Policy and Charging Rules Function  

Each network function is implemented as a standalone C++ module and communicates via standardized 3GPP interfaces.

---

## Supported Protocols

- **Diameter**
  - Used for S6a, Gx, Rx, and Sy
- **GTP-C / GTP-U**
  - Used for S5/S8 control and user plane

---

## EPC Interfaces Coverage

### ✅ Implemented Interfaces

| Interface | Between | Description | Protocol |
|---------|--------|-------------|-------------|
| **S6a** | MME ↔ HSS | Subscriber authentication, authorization, mobility data | Diameter |
| **S11** | MME ↔ SGW | Control plane (session management) | GTP-C |
| **S5/S8** | SGW ↔ PGW | Bearer management and user-plane tunneling | GTP-C (Control), GTP-U (User) |
| **Gx** | PGW ↔ PCRF | Policy and charging control | Diameter |

---

### 🧩 eNB-Facing Interfaces Supported (No eNodeB Implementation)

The MME and SGW implement **eNB-facing interfaces** to enable interaction with external systems such as **OAI**.

| Interface | Between | Description | Protocol |
|---------|--------|-------------|-------------|
| **S1-MME** | eNodeB ↔ MME | Control plane (signaling) | SCTP Carrying S1AP |
| **S1-U** | eNodeB ↔ SGW | User plane (data transfer) | GTP-U |

### 🧩 IMS-Facing Interfaces (Supported, No IMS Implementation)

The PCRF implements **IMS-facing interfaces** to enable interaction with external IMS systems such as **Kamailio**.

| Interface | Between | Description |
|---------|--------|-------------|
| **Rx** | P-CSCF ↔ PCRF | IMS session-based policy control |
| **Sy** | OCS ↔ PCRF | Online charging control |

📌 **Note:**  
OpenEPC does **not** implement IMS components (P-CSCF, S-CSCF, I-CSCF).  
Rx and Sy are provided so that **external IMS/charging systems can interact with the PCRF**, making OpenEPC *IMS-compatible* rather than an IMS implementation.

---

### 🔓 Open / External Interfaces (Not Implemented)

These interfaces are intentionally left open for integration with external systems:

| Interface | Expected Peer | Status |
|---------|--------------|-------|
| **S1-MME** | eNB | External (no eNB implementation) |
| **S1-U** | eNB | External (no eNB implementation) |
| **SGi** | Internet / Data Network | External |
| **Gy** | OCS | Optional / External |
| **Cx / Sh** | IMS HSS | Not implemented |

---

## Architecture Diagram

                                +--------------------+       +-------+-------+         +--------------------+          
                                |        HSS         |       |  Charging/OCS | <-----> |        PCRF        |<-- Rx   
                                |  (Subscriber Data) |       |   (External)  |     Sy  | Policy & Charging  |    |      
                                +----------+---------+       +-------+-------+         +----------+---------+    |      
                                           |                                                      |              |
                                          S6a                                                     Gx             |   +------------+------------+
                                           |                                                      |              --->|    IMS (External)       |
        +--------------+    S1-MME    +----+----+      S11      +----+----+      S5/S8       +----+----+             |    (e.g. Kamailio)      |
        |     eNB      |<------------>|   MME   |<------------->|   SGW   |<---------------->|   PGW   | <---------> +------------+------------+
        |  (External)  |              | Mobility|               | Serving |                  |   PDN   |     SGi     +------------+------------+           
        |              |              | Control |               | Gateway |                  | Gateway | <---------> |   External Data Network |             
        +------+-------+              +----+----+               +----+----+                  +----+----+             |  (Internet / Services)  | 
               |                                                     |                                                 +------------+------------+
               -------------------------- S1-U -----------------------

---

## Design Philosophy

- **Standards-oriented** (3GPP-aligned interfaces and protocols)
- **Modular & extensible**
- **Interoperable by design**
- Clear separation between **EPC core** and **access / IMS domains**

OpenEPC supports IMS interaction through PCRF interfaces without embedding IMS logic, keeping the EPC clean and focused.

---

## Technology Stack

- **Language:** C++
- **Protocols:** Diameter, GTP-C, GTP-U, SCTP, S1AP
- **Architecture:** Component-based, message-driven

---

## Scope & Non-Goals

### In Scope
- LTE EPC core functionality
- Policy and charging control
- IMS-compatible PCRF interfaces (Rx)
- Integration with external OAI/IMS systems

### Out of Scope
- eNB / access network implementation
- IMS core network (P-CSCF, S-CSCF, I-CSCF)
- End-user services

---

## Intended Use

- Research & academic projects
- EPC protocol exploration
- Interoperability testing
- Proof-of-concept OAI/IMS integration

---

## License

- GNU GENERAL PUBLIC LICENSE Version 3

---
# Target

- Primary implementation S6a, S11, S5/S8, Gx
- Secondary implementation Rx, Sy, S1-MME, S1-U

---

## Disclaimer

This software is provided for **research and educational purposes only**  
and is **not production-grade** without further validation, testing, and security hardening.

