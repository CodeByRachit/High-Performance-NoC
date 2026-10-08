# High-Performance 4x4 Mesh Network-on-Chip (NoC)

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Language](https://img.shields.io/badge/language-SystemVerilog-brightgreen.svg)
![Topology](https://img.shields.io/badge/topology-Mesh-orange.svg)

## 🚀 Overview

This repository features a complete, scalable RTL design of a **4x4 Network-on-Chip (NoC)** based on a mesh topology. The architecture has been written and verified purely in **SystemVerilog**, showcasing a parameterized design capable of efficient data routing across an embedded chip.

It includes an elaborate implementation of fundamental networking concepts on a hardware level, such as X-Y routing algorithms, priority-based switch allocation, and FIFO buffer management. 

## 📁 Repository Structure

```text
├── src/                # SystemVerilog RTL modules (Router, Buffer, Switch, etc.)
├── include/            # Common definitions, routing logic, and SystemVerilog interfaces
├── testbenches/        # Simulation environments for primitive units, single router, and full NoC
└── README.md           # Project documentation
```

## 🏗️ Hardware Architecture & Modules

The design is highly modular, ensuring ease of scalability and debugging. The core components of the network include:

### 1. Router (`Router.sv`)
The heart of the NoC. The router connects to five distinct ports: `Local`, `North`, `South`, `East`, and `West`. It acts as a wrapper around the buffer unit, routing unit, and switch allocator.

### 2. Buffer Unit (`Buffer_Unit.sv` & `FIFO.sv`)
Handles packet reception and storage. It utilizes a First-In-First-Out (FIFO) buffer mechanism and communicates with neighboring modules via a robust `ReqAckIO` (Request-Acknowledge) interface.

### 3. Routing Unit (`Routing_Unit.sv`)
Implements a combinational **X-Y Routing Algorithm** to compute the next-hop output port based on a packet's destination coordinates.

### 4. Switch Allocator (`Switch_Allocator.sv`)
Resolves port contention using a strict priority-based arbitration mechanism. The arbitration prioritizes the ports in the following order to resolve conflict efficiently: `Local` > `West` > `North` > `East` > `South`.

### 5. Switch (`Switch.sv`)
Responsible for physically connecting the requested input channels to the granted output channels based on the allocator's decision.

## 🔌 Communication Interfaces

The system heavily relies on structured SystemVerilog interfaces (`interfaces.sv`) for modularity:
- **`ReqAckIO`**: Facilitates synchronous Request/Acknowledge handshaking between routers.
- **`ReqGntIO`**: Manages Request/Grant signals between internal buffers and the Switch Allocator.
- **`FifoIO`**: Encapsulates read/write operations and status flags for buffer management.

## 🧪 Simulation and Verification

The repository adopts a bottom-up verification strategy:
- **Primitive Testing**: Individual tests isolate the behavior of `FIFO.sv`, `Buffer_Unit.sv`, and `Routing_Unit.sv`.
- **Router Testing**: `Router_TB.sv` injects packets into all five ports simultaneously to observe routing functionality and conflict resolution.
- **System-Level NoC Simulation**: `NOC_TB.sv` uses test `Node.sv` components connected to each of the 16 routers. These nodes randomly generate packets and use high-level SystemVerilog mailboxes as a side-channel reference to ensure no packets are lost or misrouted in transit.

## 📝 License

This hardware design is open-sourced under the MIT License.
