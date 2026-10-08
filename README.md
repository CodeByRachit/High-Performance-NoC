# Advanced Network-on-Chip (NoC) Design

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Language](https://img.shields.io/badge/language-SystemVerilog-brightgreen.svg)

## 🚀 Overview

This repository contains the RTL design and implementation of a highly efficient **4x4 Network-on-Chip (NoC)** using a mesh topology. The design is fully implemented and tested in **SystemVerilog**, showcasing a complete, parameterized approach to scalable on-chip communication.

## 📁 Project Structure

- `src/`: Core SystemVerilog source files (Buffers, Routers, Allocators).
- `include/`: Common definitions and interfaces.
- `testbenches/`: Comprehensive testbenches for all modules and the full NoC.

## 🔧 Core Components

1. **Buffer Unit**: Manages packet reception, storage, and forwarding using FIFOs.
2. **Routing Unit**: Implements the X-Y routing algorithm to navigate packets to their destination.
3. **Switch Allocator**: Resolves conflicts with a priority-based arbitration mechanism.
4. **Switch & Router**: Integrates all sub-modules to route packets across the 5 ports (Local, North, South, East, West).

## 🧪 Testing & Simulation

The project features a multi-tiered testing strategy:
- **Primitive Testing**: Individual tests for FIFOs, Buffers, and Routing Units.
- **Router Testing**: Verification of conflict resolution and proper routing.
- **Full NoC Simulation**: A high-level testbench using simulated Nodes to inject and receive packets across the 4x4 mesh.

## 📝 License

This project is open-source and available under the MIT License.
