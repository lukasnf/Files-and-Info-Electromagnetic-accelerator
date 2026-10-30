# 4-Stage Circular Electromagnetic Accelerator (Ring Accelerator)

A closed-loop, multi-stage electromagnetic accelerator designed to continuously circulate and accelerate a ferromagnetic projectile around a 3D-printed circular track using four sequentially pulsed coils and optical synchronization.

# ⚠️ MANDATORY SAFETY PRECAUTIONS & WARNINGS

> **DANGER: HIGH CURRENT & HIGH SPEEDS**  
> Operating this device involves significant electrical, kinetic, and fire hazards. Read and implement all safety requirements prior to assembly or testing.
---

## 📌 Overview & Operating Principle

Unlike linear accelerators that fire a projectile once, this ring accelerator uses a closed circular track to repeatedly pass the projectile through the same set of coils, building velocity over multiple orbits.

1. **Continuous Position Tracking:** Through-beam optical sensors (photoelectric light gates) are positioned along the track immediately before each of the four coil stages.
2. **Synchronized Pulsing:** When the projectile breaks a sensor's beam, the microcontroller calculates the entry velocity and triggers the corresponding power stage.
3. **Reluctance Acceleration:** The energized coil produces a localized magnetic gradient, pulling the projectile into its core.
4. **Anti-Braking Shutoff:** The coil turns off immediately before the projectile passes its geometric midpoint to avoid deceleration ("suckback effect").
5. **Multi-Lap Circulation:** The projectile exits the coil, remains guided along the curved track, and proceeds to the subsequent stage for cumulative speed gains.

---

## 🛠 Hardware Specifications

### 1. Mechanical Components (3D Printing)
* **Circular Track / Guide Segments:** Modular, curved 3D-printed guide rails that form a continuous circular loop with integrated sensor slots and bobbin mounts.
  * *Recommended Material:* PETG, ABS, or ASA (PLA is prone to softening under sustained coil heat and high friction).
  * *Infill:* 40%+ with at least 4 perimeter walls to withstand radial outward forces from circulating projectiles.
* **Coil Bobbins (`spule_endgütlig.stl`):** Custom winding bobbins curved or seated tangentially to fit the circular track profile.

### 2. Electromagnetic Stages (4x)
* **Windings:** Enameled copper magnet wire (typically 0.6 mm to 1.0 mm diameter).
* **Switching Components:** Fast-switching power MOSFETs or IGBTs paired with dedicated low-side or high-side gate drivers.
* **Flyback / Inductive Protection:** High-speed freewheeling diodes (ultrafast recovery) or bidirectional TVS diodes in parallel with each coil stage to safely dissipate back-EMF spikes.

### 3. Sensor System
* **Type:** High-speed infrared optical interrupters (phototransistor and IR emitter pairs).
* **Interfacing:** Connected directly to hardware interrupt-capable pins on the microcontroller to guarantee microsecond-level response times.

---

## ⚙️ Trigger Sequence & Timing Dynamics

Because the projectile increases in speed with each completed lap, the pulse width must adapt dynamically:

```text
       ┌─────────── [Coil 1 / Sensor 1] ───────────┐
       │                                           │
       ▼                                           ▲
 [Coil 4 / Sensor 4]                         [Coil 2 / Sensor 2]
       │                                           │
       └─────────── [Coil 3 / Sensor 3] ───────────┘
