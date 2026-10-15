# Non-Clipping Realtime Recording Pipeline

An open-source hardware routing architecture and signal-flow blueprint designed to achieve zero-latency, digitally unclipped, industry-standard loudness recordings directly into Android mobile devices.

This blueprint outlines a precise hardware-to-software method to deliver an optimized, consistent **-14 LUFS** master-level signal straight out of the box with **zero digital post-production or software normalization required**.

---

## 📐 System Architecture & Hardware Path

The pipeline relies entirely on a physical, analog front-end to shape, gain-stage, and match impedances before sending the signal to the mobile device's built-in hardware preamplifiers. This allows the system to remain completely latency-free.

**Signal Flow:**
1. **Analog Source** (Microphone / Guitar)
   └── *Routes audio into...*
2. **Full Analog Path** (Preamp / Audio Interface)
   └── *Stages clean voltage gain into...*
3. **Impedance Match** (IK Multimedia iRig 2 Circuit)
   └── *Adapts signal levels for...*
4. **Hardware TRRS Input** (3.5mm Socket on Mobile Motherboard)
   └── *Feeds pristine raw voltage to...*
5. **Android Core OS** (Captured via UNPROCESSED Audio API Hook)

---

### 1. The Analog Source (Input Layer)
The pipeline accommodates any analog acoustic or high-impedance (Hi-Z) electrical source, proving its structural versatility across all gear price brackets:
* **Microphones:** Broadcast-grade dynamic microphones (e.g., Røde Procaster) down to budget consumer equipment (e.g., Lidl Karaoke Microphone).
* **Instruments:** Balanced line signals, electric guitars, or bass guitars.

### 2. Full Analog Signal Path (Gain & Conditioning Layer)
The source enters a dedicated analog preamplifier or audio interface utilizing a 100% analog signal path (e.g., M-Audio Fast Track USB or integrated analog mixers). 
* **The Blueprint Trick:** The gain staging is optimized entirely within the analog domain, utilizing the physical voltage headroom of the analog circuits to cushion explosive transients *before* encountering digital brickwall clipping.

### 3. Impedance & Level Matching (The Bridge)
The high-level analog output is routed through an **IK Multimedia iRig 2** circuit. This step acts as the critical hardware interface layer, matching the voltage levels and electrical impedance of line/instrument gear to the highly sensitive microphone input expectations of mobile device chipsets.

### 4. Hardware Receiver (Mobile Integration Layer)
The signal enters the Android phone physically via the analog **TRRS (3.5mm) microphone input socket**. 
* **Hardware Validation:** This framework has been physically verified and field-tested using **Xiaomi REDMI** hardware architectures. 
* *Note:* Dynamic results and analog coloration may vary depending on the specific microphone preamp circuit layout embedded on the host device's motherboard.

### 5. Capture & API Hook (Software Intake Layer)
The recording application running on the Android device **must** explicitly call the native Android audio API using the **`UNPROCESSED`** audio source flag (`MediaRecorder.AudioSource.UNPROCESSED`). 
* **Why this matters:** This bypasses all of Android's internal digital background noise cancellation, hardware ducking, and aggressive digital automatic gain control (AGC). It allows the raw, perfectly staged analog signal to hit the Analog-to-Digital Converter (ADC) completely untouched.

---

## ⚡ Key Technical Advantages

* **Zero-Latency Tracking:** Because no digital limiters, compressors, or lookahead software buffers are used on the host mobile device, there is absolutely zero monitoring latency. This makes it ideal for timing-critical voice tracking, live podcasting, and musical instrument recording.
* **Instant -14 LUFS Output:** Eliminates the traditional tedious post-production chain. The recording is instantly loudness-compliant with major streaming platforms right out of the device.
* **High Headroom Tracking:** Capable of handling aggressive transient spikes (like heavy vocal inflections, slap bass, or dynamic guitar picking) by absorbing the energy natively in the analog pre-stages. It is highly optimized for voice recording, and works exceptionally well for guitar/bass tracking.

---

## ⚖️ Legal & Open Source Licensing

This documentation, architectural layout, and systemic hardware configuration blueprint are licensed under the **CERN Open Hardware License – Strongly Reciprocal (CERN-OHL-S v2)**.

### Intellectual Property Protection
* **Attribution Required:** Anyone copying, distributing, or fabricating physical hardware based on this blueprint **must** maintain the original author's name, copyright notices, and license metadata on all documentation, schematics, and physical hardware circuit boards.
* **Reciprocity (Copyleft):** If anyone modifies this circuit architecture or signal-routing layout and manufactures or distributes a physical commercial audio tool based on it, they are **legally required** to make their modified hardware blueprints open-source under the exact same CERN-OHL-S v2 terms.

---
*Blueprint conceived, engineered, and validated by the repository owner.*

