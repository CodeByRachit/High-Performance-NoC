# High-Performance NoC

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Language](https://img.shields.io/badge/language-SystemVerilog-brightgreen.svg)
![Topology](https://img.shields.io/badge/topology-Mesh-orange.svg)

## 📖 What is a Network-on-Chip (NoC)?

Imagine a bustling city where different neighborhoods (processors, memory, peripherals) need to talk to each other. Instead of building a direct road between every single neighborhood (which would be chaotic and take up too much space), we build a **grid of intersections** (routers). 

A **Network-on-Chip (NoC)** does exactly this for microchips. It is a highly efficient, scalable network built directly onto a silicon chip that routes data packets between different components, much like cars on a grid of streets. 

## 🚀 Overview of This Project

This project is a complete RTL (Register Transfer Level) design of a **4x4 Mesh NoC** written purely in **SystemVerilog**. It features a 16-node grid (4x4) where each node can seamlessly communicate with any other node using standardized algorithms and hardware buffers.

### The 4x4 Mesh Topology

```mermaid
graph TD
    %% Define Nodes
    N00((Node 0,0)) <--- ---> N01((Node 0,1)) <--- ---> N02((Node 0,2)) <--- ---> N03((Node 0,3))
    N10((Node 1,0)) <--- ---> N11((Node 1,1)) <--- ---> N12((Node 1,2)) <--- ---> N13((Node 1,3))
    N20((Node 2,0)) <--- ---> N21((Node 2,1)) <--- ---> N22((Node 2,2)) <--- ---> N23((Node 2,3))
    N30((Node 3,0)) <--- ---> N31((Node 3,1)) <--- ---> N32((Node 3,2)) <--- ---> N33((Node 3,3))

    %% Vertical connections
    N00 <--- ---> N10 <--- ---> N20 <--- ---> N30
    N01 <--- ---> N11 <--- ---> N21 <--- ---> N31
    N02 <--- ---> N12 <--- ---> N22 <--- ---> N32
    N03 <--- ---> N13 <--- ---> N23 <--- ---> N33
```
*Each node contains a Router that directs traffic North, South, East, West, or Locally.*

## 🏗️ Hardware Architecture

The system is highly modular. If you look inside a single Node, you will find several sub-components working together:

1. **The Router**: The main "intersection" controller. It connects to 5 ports (Local, North, South, East, West).
2. **Buffer Unit (FIFO)**: When data arrives, it is temporarily stored in these First-In-First-Out queues so packets don't crash into each other.
3. **Routing Unit**: The "GPS" of the node. It uses the **X-Y Routing Algorithm** to decide which direction a packet should go next based on its final destination.
4. **Switch Allocator**: The "Traffic Cop". If two packets want to go out the same port at the same time, this resolves the conflict using a strict priority system (`Local` > `West` > `North` > `East` > `South`).
5. **Switch**: The physical crossbar that connects the input buffers to the chosen output ports.

## 🔌 Communication Interfaces

We use strict interfaces (`interfaces.sv`) to keep the code clean and prevent errors:
- **`ReqAckIO`**: A handshake protocol (Request/Acknowledge) used between routers to ensure data is safely passed along.
- **`ReqGntIO`**: A Request/Grant system used internally to talk to the Switch Allocator.

## 🧪 How It Was Tested

Hardware needs to be perfect before it's manufactured. This project uses a **bottom-up testing strategy**:
1. **Unit Tests**: We first test the small pieces (FIFOs, Buffers) individually.
2. **Router Test**: We test a single intersection by blasting data into all 5 ports to see how it handles congestion.
3. **Full System Simulation**: We simulate the entire 4x4 grid. Test Nodes generate random packets, and we use high-level SystemVerilog "mailboxes" to magically track the packets and ensure 100% of them arrive at their correct destinations safely.

## 📁 Repository Structure

```text
├── src/                # SystemVerilog RTL modules (Router, Buffer, Switch, etc.)
├── include/            # Common definitions, routing logic, and interfaces
├── testbenches/        # Simulation environments for unit tests and the full grid
└── README.md           # You are here!
```

## 📝 License

This hardware design is open-sourced under the MIT License.
