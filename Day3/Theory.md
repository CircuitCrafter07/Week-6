
# Enhanced Theory: RISC-V SoC Tapeout Program — Detailed Notes

## 🧩 1. SPICE Deck Creation for CMOS Inverter (Expanded)
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

## 🧩 2. 16-Mask CMOS Fabrication Process (Deep Dive)

### 2.1 **Selecting a Substrate**
- The starting wafer is typically **p-type silicon** for NMOS or **n-type** for PMOS.
- Resistivity and doping concentration determine:
  - Threshold voltage (Vth)
  - Leakage current
  - Latch-up immunity  
- Substrate doping is kept **lower than well doping** to maintain correct junction formation.

---

### 2.2 **Active Region Formation**
This step prepares the regions where MOS transistors will reside.

**Steps involved:**
- Grow a thin pad oxide
- Deposit Si₃N₄ as a mask
- Photolithography to define active regions
- Etch nitride to expose areas for oxidation
- Oxidize exposed silicon to form **field oxide**

Importance:
- Defines MOS channel locations  
- Controls leakage and isolation  
- Prevents unintended parasitic transistor formation

---

### 2.3 **Local Oxidation of Silicon (LOCOS)**
LOCOS creates isolation between devices.

Key characteristics:
- Forms **thick field oxide**
- Nitride mask prevents oxidation in active areas
- Causes a “bird’s beak” effect due to lateral oxidation

**Bird’s Beak Issues:**
- Reduces active region width  
- Limits scaling  
- Non-uniform field oxide thickness

LOCOS was later replaced by **Shallow Trench Isolation (STI)** in deep submicron nodes.

---

### 2.4 **N-Well and P-Well Formation**
Wells form the bases for PMOS and NMOS devices.

Process:
1. Wafer oxidation  
2. Apply photoresist  
3. Expose and develop well regions  
4. Ion implantation  
5. Annealing at 900–1100°C for dopant activation  

Considerations:
- Well depth affects body effect  
- Implant energy determines junction depth  
- Retrograde wells help reduce punch-through

---

### 2.5 **Gate Terminal Formation**
The MOS gate stack is formed with extreme precision.

**Steps:**
- Grow ultra-thin **gate oxide (1–3 nm)**  
- Deposit **polysilicon gate** (now replaced by metal gate in advanced nodes)  
- Pattern using photolithography  
- Etch polysilicon using anisotropic plasma etching  

**Gate Control Importance:**
- Determines channel formation  
- Impacts threshold voltage  
- Influences switching speed

---

### 2.6 **Lightly Doped Drain (LDD) Formation**
LDD reduces the **electric field spike** near the drain, preventing:
- Hot carrier injection  
- Oxide damage  
- Long-term transistor degradation  

Steps:
1. Light doping near drain  
2. Deposit dielectric  
3. Anisotropically etch to form spacers  
4. Perform heavy doping for final S/D regions  

Sidewall spacers ensure precise alignment.

---

### 2.7 **Source-Drain Formation**
High-dose implantation forms conductive n+ / p+ regions.

Key considerations:
- Sheet resistance  
- Junction depth  
- Leakage reduction  
- Minimizing parasitic resistance  

Activation is done by **rapid thermal annealing (RTA)**.

---

### 2.8 **Contacts & Interconnects Formation**
To electrically connect devices:
1. Etch oxide using **HF dip**  
2. Deposit titanium  
3. Heat wafer (650–700°C) to form **TiSi₂ silicide**  
4. Deposit tungsten plugs for contacts  

This reduces contact resistance drastically.

---

### 2.9 **Higher-Level Metal Formation**
To route signals:
- Deposit SiO₂ (interlayer dielectric)  
- Use CMP to ensure **planar surfaces**
- Deposit aluminium or copper metal  
- Pattern using dual-damascene for copper  

Interconnect design affects:
- Timing  
- Crosstalk  
- IR drop  
- Electromigration reliability

---

## 🧠 Important Concepts

### 🔹 Plasma Anisotropic Etching
- Directional ion-based etching  
- Essential for deep trenches and fine patterns  
- Enables vertical sidewalls  

### 🔹 Hot Electron Effect
Occurs due to:
- High drain–source fields  
- Electrons gaining kinetic energy  
- Injecting into oxide  
This degrades device lifespan.

### 🔹 Short Channel Effects (SCE)
In scaled MOSFETs:
- Vth roll-off  
- Drain-Induced Barrier Lowering (DIBL)  
- Velocity saturation  
- Punch-through  
Requires LDD, halo implants, and high-k dielectrics.

---

## 📌 Summary
This expanded document provides deeper insight into:
- SPICE simulation fundamentals  
- CMOS fabrication steps  
- Transistor engineering challenges  
- Impact of process on electrical characteristics  

It forms a comprehensive reference for VLSI learners, RISC-V SoC designers, and anyone preparing for silicon tapeout workflows.
