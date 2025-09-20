# 🚀 RISC-V SoC Design Workflow 📝

This document outlines the end-to-end workflow for designing a System on a Chip (SoC), from initial specification to the final manufactured hardware. The process is broken down into several major verification stages, ensuring the design remains correct at every level of abstraction.

> The ultimate goal is to create a physical chip that perfectly matches the initial requirements, proven by the final verification: `O0 == O1 == O2 == O3 == O4`.

---

## 📋 Project Scope & Target

This design is targeted for the cost-effective and power-efficient embedded systems market.

| Feature                 | Specification                                                              |
| ----------------------- | -------------------------------------------------------------------------- |
| **Operating Frequency** | `100MHz` to `130MHz`                                                       |
| **Target Applications** | <ul><li>Arduino Boards</li><li>TV Panels</li><li>AC & Home Appliances</li><li>Wearables (e.g., iWatch)</li></ul> |

---

## ⛓️ The Design & Verification Flow

The core principle of this workflow is to continuously verify each new stage of the design against a "golden reference" from the previous stage. This ensures that no errors are introduced as the design moves from abstract concept to physical reality.

### Visualized Workflow
<img width="3116" height="444" alt="canvas_vlsi-250920_0727" src="https://github.com/user-attachments/assets/aca50d3d-8bca-453a-87ad-190f03f9294b" />


### 0️⃣ O0: Application Requirements & Specification

This is the foundational stage that defines the "why" and "what" of the project before any code is written.

* **Process**: Gather and define system requirements, target application behavior, desired features, and performance goals. This is the **conceptual blueprint** of the chip.
* **Output**: A detailed requirements document (`O0`), which serves as the ultimate **source of truth**.
* **Verification**: This stage sets the benchmark for all subsequent work.

### 1️⃣ O1: Chip Modelling & Specification (C-Model)

A high-level, functional model is created based on the requirements from `O0`.

* **Process**: The specifications are modeled using a high-level language, typically **C/C++**. This **C-model** defines *what* the chip should do, not *how* it does it in hardware. A C-based testbench is also developed.
* **Output**: A verified functional specification (`O1`).
* **Verification**: `O0 == O1`. This crucial step ensures the C-model accurately captures the initial requirements.

### 2️⃣ O2: RTL (Register-Transfer Level) Architecture

The functional model is translated into a "soft copy" of the hardware.

* **Process**: The C-model's functionality is described using a Hardware Description Language (**HDL**) like **Verilog** or **VHDL**. This **RTL code** defines the chip's microarchitecture.
* **Output**: The RTL design (`O2`).
* **Verification**: `O1 == O2`. The C testbench is run on the RTL design (in a simulator) to confirm identical behavior.

### 3️⃣ O3: SoC Integration

All the different hardware components are assembled into a complete System on a Chip.

* **Process**: The SoC is built by integrating key blocks:
    * **Processor Core**: The main CPU.
    * **Peripherals/IPs**: Digital **Macros** and **Analog IPs** (for real-world interface).
    * **GPIOs**: General Purpose Input/Output connections.
* **Output**: The fully integrated SoC design (`O3`).
* **Verification**: `O1 == O2 == O3`. The integrated design is again verified against the golden C-model.

### 4️⃣ O4: Post-Silicon Validation

The final stage where the physical, manufactured chip is tested.

* **Process**:
    1.  **RTL-to-GDSII**: The logical design (`O3`) is converted into a physical layout (`GDSII` file) through synthesis, placement, and routing.
    2.  **Checks**: The layout undergoes **DRC** (Design Rule Check) and **LVS** (Layout Versus Schematic) checks.
    3.  **Manufacturing**: The `GDSII` file is sent to a foundry (**Tapeout**), and physical chips are received back (**Tape-in**).
    4.  **Board Bring-up**: The chip is mounted on a PCB and tested with firmware.
* **Output**: The real-world behavior of the physical chip (`O4`).
* **Verification**: `O1 == O2 == O3 == O4`. The original C testbench is run on the actual silicon to provide the final sign-off.

---
