## RISC-V SoC Tapeout Program: From Design to Fabrication

This document provides a detailed overview of the two fundamental phases of creating a System-on-a-Chip (SoC):
1.  **Standard Cell Design & Characterization:** The digital design phase where logic gates (like inverters) are laid out and their performance is simulated.
2.  **CMOS Fabrication:** The physical manufacturing process that builds the transistors and wiring on a silicon wafer.

---

## 🎨 Part 1: Design Library Cell using Magic Layout & NGSpice

A **SPICE deck** is a structured text file used in circuit simulation tools such as NGSpice or HSpice.  
It contains:
- **Component declarations** (MOSFETs, resistors, capacitors)
- **Node connectivity**
- **Model definitions**
- **Analysis commands** (DC sweep, AC analysis, transient simulation)

### Why SPICE Decks Matter
They allow verification of:
- Voltage transfer characteristics (VTC)
- Noise margins
- Rise/fall times
- Propagation delays
- Power dissipation  
This helps validate the electrical behavior before layout and fabrication.

A typical CMOS inverter deck includes:
- PMOS and NMOS transistor definitions
- Input voltage source
- Supply sources
- Load capacitor
- `.tran` and `.dc` analysis commands

---

### <ins>SPICE Deck Creation for a CMOS Inverter</ins>

To perform a simulation, you must create a SPICE deck. A SPICE deck is a text-based file that describes the circuit. It serves as the input for the NGSpice simulator.

A typical deck includes:
* **Circuit Elements:** Lists all components (e.g., `M1` for PMOS, `M0` for NMOS), their values (like transistor width $W$ and length $L$), and the nodes they connect to (e.g., `vdd`, `gnd`, `in`, `out`).
* **Model Definitions:** Includes `.model` statements or links to technology files (like the Sky130 PDK models) that define the complex electrical behavior of the transistors.
* **Voltage/Current Sources:** Defines the power supplies (e.g., `Vdd`) and the input signals (e.g., a `VPULSE` source to simulate a 0-to-1 transition).
* **Analysis Commands:** Tells SPICE what to simulate (e.g., `.tran` for a transient analysis over time, or `.dc` for a DC sweep).
* **Measurement Commands:** Instructs SPICE to measure specific parameters, such as the 50% delay from input to output.

---

## 🏭 Part 2: The 16-Mask CMOS Fabrication Process

This is the industrial process of building the circuit, layer by layer, on a silicon wafer. A "16-Mask" process means it uses 16 unique photolithography steps (masks) to pattern the different materials.

#### 1. <ins>Selecting a Substrate</ins>
* **Foundation:** The process begins with a substrate, which is a thin, pure silicon wafer. This acts as the base material for all components.
* **Doping:** For an **N-well** process (common for CMOS), a P-type substrate is used. The P-substrate's doping concentration must be lower than the N-well's doping to ensure the well is a distinct, isolated region.

#### 2. <ins>Well Formation (N-well and P-well)</ins>
* **Purpose:** To create isolated "tubs" of different doping types to house the NMOS and PMOS transistors. PMOS transistors are built in the N-well, and NMOS transistors are built in the P-substrate (or a dedicated P-well in a "twin-tub" process).
* **Process:** This involves multiple steps:
    * **Oxidation:** A layer of Silicon Dioxide (SiO₂) is grown on the wafer.
    * **Photolithography (Mask 1 - N-well):** The wafer is coated with a light-sensitive 'photoresist'. A mask is used to expose only the areas where the N-well should be. The exposed (or unexposed) resist is washed away.
    * **Ion Implantation:** The wafer is bombarded with N-type dopant ions (like Phosphorus). These ions penetrate the silicon only in the unmasked areas.
    * **Annealing:** The wafer is heated. This "activates" the dopants, allowing them to settle into the silicon crystal lattice, and drives them deeper to form the well.
    * This process is repeated with a different mask (Mask 2) and P-type dopants (like Boron) to create P-wells.

#### 3. <ins>Creating an Active Region (Device Isolation)</ins>
* **Purpose:** To electrically isolate individual transistors from each other, preventing current from leaking between them. The area where the transistor (source, drain, channel) will be built is called the "active region".
* **Process:** This is achieved by creating thick oxide regions to separate the active areas.
* **LOCOS (Local Oxidation of Silicon):** A common method is LOCOS.
    * A silicon nitride (Si₃N₄) layer is patterned to cover the active regions.
    * The wafer is heated in an oxygen-rich environment. A thick "field oxide" (SiO₂) grows in the unmasked areas, creating insulating walls.
    * **Bird's Beak:** A known issue with LOCOS is the "bird's beak," where the oxide tapers and encroaches laterally under the nitride mask. This wastes space and limits device density.
* **Modern Alternative (STI):** Most modern processes use **Shallow Trench Isolation (STI)**. This involves etching narrow trenches into the silicon, filling them with oxide, and polishing the surface flat. STI avoids the bird's beak and allows for much denser transistor packing.

#### 4. <ins>Formation of Gate Terminal</ins>
* **Purpose:** To create the transistor's "gate," which controls the flow of current.
* **Process:**
    * **Gate Oxide:** A very thin, extremely high-quality layer of Silicon Dioxide (SiO₂) is grown on the active region using thermal oxidation. This layer's thickness and quality are critical, as it insulates the gate from the channel.
    * **Polysilicon Deposition:** A layer of polycrystalline silicon (polysilicon) is deposited over the entire wafer. Polysilicon is used because it's a good conductor (when doped) and can withstand the high-temperature steps that follow.
    * **Photolithography & Etching (Mask 3 - Poly):** The polysilicon is patterned using a mask and etched to form the gate electrodes. This etch must be precise.

#### 5. <ins>Lightly Doped Drain (LDD) Formation</ins>
* **Purpose:** To mitigate **Hot Carrier Effects** (see below). High electric fields near the drain can accelerate electrons, giving them enough energy to damage the gate oxide. LDDs add a lightly doped region to "grade" the electric field, making it less sharp.
* **Process:**
    * **Mask 4 (N-LDD):** A light dose of N-type impurities is implanted into the areas for NMOS source/drains. The polysilicon gate itself acts as a mask, "self-aligning" this implant.
    * **Mask 5 (P-LDD):** A light dose of P-type impurities is implanted for the PMOS devices.

#### 6. <ins>Sidewall Spacer Formation</ins>
* **Purpose:** These spacers are critical for the LDD structure. They serve as a mask for the main source/drain implant, offsetting it from the gate edge.
* **Process:**
    * A layer of insulating material (like Silicon Nitride) is deposited over the entire wafer, conforming to the topography.
    * **Anisotropic Etching:** The wafer undergoes a *directional* etch (see below). This etch removes the material from all horizontal surfaces but leaves it on the vertical "sidewalls" of the polysilicon gate.

#### 7. <ins>Source and Drain Formation</ins>
* **Purpose:** To create the heavily doped n+ (NMOS) and p+ (PMOS) regions that form the transistor's source and drain terminals.
* **Process:**
    * **Mask 6 (N+ S/D):** A heavy dose of N-type dopants is implanted via ion implantation. The polysilicon gate *and* the newly formed sidewall spacers act as a mask. This creates the heavily doped n+ region, while the LDD region remains protected under the spacer.
    * **Mask 7 (P+ S/D):** The process is repeated with P-type dopants for the PMOS transistors.
    * **Annealing:** A high-temperature step (often Rapid Thermal Anneal, or RTA) is performed to activate these S/D dopants.

#### 8. <ins>Contact Formation (Silicidation & Vias)</ins>
* **Purpose:** To create a low-resistance electrical connection between the silicon (source, drain, and gate) and the first metal layer.
* **Process:**
    * **Silicidation:**
        * The wafer is cleaned (e.g., with an HF solution to remove thin native oxides).
        * A thin layer of refractory metal (like Titanium or Cobalt) is deposited (sputtered) over the wafer.
        * The wafer is heated (e.g., $650 - 700^\circ \text{C}$). The metal reacts *only* with the exposed silicon (the S/D and poly gate tops) to form a silicide (e.g., TiSi₂). This material has much lower resistance than the doped silicon.
        * The unreacted metal (which sits on the oxide) is stripped away.
    * **Inter-Layer Dielectric (ILD):** A thick layer of insulating oxide (SiO₂) is deposited over the entire chip.
    * **Planarization:** The surface is planarized using **Chemical Mechanical Polishing (CMP)** to make it perfectly flat for the next layers.
    * **Contact Etch (Mask 8):** A mask is used to etch holes (contacts) through the ILD, stopping on the silicide.
    * **Plug Formation:** The holes are filled with a metal, typically Tungsten (W), and CMP is used again to remove excess Tungsten, leaving "plugs."

#### 9. <ins>Higher Level Metal Formation (Interconnects)</ins>
* **Purpose:** To "wire" the millions of transistors together to build the final circuit. Modern chips have many (6-10+) layers of metal.
* **Process:**
    * **Metal 1 (Mask 9):** A layer of metal (e.g., Aluminum or Copper) is deposited. It is then masked and etched to form the first level of wiring, which connects to the Tungsten plugs.
    * **Inter-Metal Dielectric (IMD):** An insulating layer (e.g., SiO₂ doped with phosphorus or boron, known as BPSG) is deposited to isolate Metal 1. BPSG is often used because it can be "reflowed" with heat to help planarize the surface.
    * **Planarization:** CMP is used again to create a perfectly flat surface.
    * **Via 1 (Mask 10):** A mask is used to etch "vias" (like contacts, but between metal layers) down to Metal 1.
    * **Metal 2 (Mask 11):** The vias are filled, and the Metal 2 layer is deposited and patterned.
    * This process (IMD deposition, CMP, Via etch, Metal deposition) is repeated for all subsequent metal layers (M3, M4, etc.) up to the final layer.

---

### 📕 Key Fabrication Concepts & Challenges

* **Plasma Anisotropic Etching:** A dry etching technique essential for modern fabrication. It uses a plasma to generate ions that are accelerated vertically toward the wafer. This allows for etching with highly vertical sidewalls and minimal undercutting (lateral etching), which is crucial for creating tiny, precise features.
* **Hot Electron Effect:** This is a reliability problem in short-channel MOSFETs. When the electric field near the drain is very high (due to short channels and high $V_{DS}$), electrons gain high kinetic energy. These "hot" electrons can be injected into the gate oxide, causing permanent damage. This damage manifests as a shift in the transistor's threshold voltage ($V_t$) over time, degrading performance and eventually causing the circuit to fail.
* **Short Channel Effect (SCE):** As the channel length ($L$) of a transistor shrinks, the drain and source depletion regions become a larger fraction of the channel length. This reduces the gate's control over the channel. Consequences include:
    * **Threshold Voltage ($V_t$) Roll-off:** The threshold voltage decreases as $L$ decreases.
    * **Drain-Induced Barrier Lowering (DIBL):** The drain voltage starts to affect the channel barrier, further lowering $V_t$ and increasing off-state leakage current.
    * SCEs are a primary obstacle in transistor scaling.
* **Photolithography:** The most critical and repeated step in fabrication. It's the process of using UV light and "masks" (stencils) to transfer a circuit pattern onto the light-sensitive photoresist coated on the wafer. The patterned resist then acts as a mask for subsequent etching or implantation steps.
* **Chemical Mechanical Polishing (CMP):** A process that uses a chemical slurry and mechanical polishing to planarize (flatten) the wafer surface. This is absolutely essential because photolithography requires a perfectly flat surface to maintain focus. CMP is used after STI, after ILD deposition, and after metal plug/damascene formation.
