# Non-Clipping Realtime Recording Pipeline

An open-source hardware routing architecture and signal-flow blueprint designed to achieve zero-latency, digitally unclipped, industry-standard loudness recordings directly into Android mobile devices.

This blueprint outlines a precise hardware-to-software method to deliver an optimized, consistent **-14 LUFS** master-level signal straight out of the box with **zero digital post-production or software normalization required**.

---

## 🪵 The Origin Story & Hardware Discovery

This pipeline was born out of deep technical frustration and methodical troubleshooting in a home studio environment.

1. **The Android USB Bug:** I initially attempted to record high-quality audio using a professional **24-bit Steinberg UR12 USB audio interface** hooked directly into Android. During testing, I discovered a major, systemic bug within the Android USB audio stack that forcefully truncates the incoming digital audio level by an astonishing **32dB to 36dB**, rendering professional USB tracking highly problematic.
2. **Bypassing the Stack:** To overcome this digital truncation limitation, I abandoned the USB bus entirely and looked for a physical workaround. I decided to leverage the phone's analog **TRRS (3.5mm) microphone port**, hoping the analog preamp circuit bypassed the broken software processing layers.
3. **The Interface Bridge:** I acquired an **IK Multimedia iRig 2** to act as an impedance-matching bridge, routing the line output of my hardware analog audio interface directly into the phone's TRRS input. The analog tracking worked flawlessly.
4. **The Serendipitous Discovery:** During aggressive testing, I accidentally maxed out the hardware gains across the signal chain. In a standard digital environment, this would result in immediate, ear-piercing digital square-wave clipping (`0 dBFS`). Instead, the pipeline remained completely unclipped.
5. **Final Streaming Verification:** Analyzing the recorded audio via YouTube's "Stats for Nerds" media inspector confirmed that the resulting video files consistently hit optimal streaming loudness metrics (averaging **-14 LUFS to -10 LUFS**) directly out of the device camera roll. While pushing extreme gains introduces classic analog distortion/saturation character, it remains remarkably musical and completely free of destructive digital clipping.

---

## 🎛️ Dynamic Compression & Crucial Gain Staging Rules

The defining feature of this blueprint is that **your primary analog pre-amp gain knob functions directly as your physical compression knob**. Dynamic thresholding happens entirely in the analog domain before hitting the digital converters.

⚠️ **THE CRITICAL CONFIGURATION PROTOCOL:**

1. **MAX OUT the iRig 2:** You **must** set the physical gain slider on the iRig 2 hardware unit to its absolute maximum. This locks the interface bridge into its maximum sensitivity threshold, allowing the circuit to act as a natural analog soft-limiter.
2. **Drive Compression/Saturation via the Pre-Amp:** You do **NOT** max out your primary audio interface/pre-amp gain stage. Instead, you use your pre-amp's gain knob to dial in your dynamics:
   * **More Compression/Presence:** Pushing the pre-amp gain higher drives the signal harder into the maxed-out iRig 2 circuit. This increases natural analog compression, density, and saturation—giving you that radio-style broadcast presence out of the box.
   * **More Clean/Dynamic Range:** Lowering the pre-amp gain reduces the input drive, yielding a cleaner, more transparent signal with wider natural transient peaks.
3. **The Sweet Spot:** Simply adjust your primary pre-amp gain knob until you hear the desired amount of vocal or instrument compression. The system will naturally cushion the transients and output an optimal **-14 LUFS to -10 LUFS** streaming-compliant volume into HedgeCam 2 without digital square-wave clipping.

---

## 📐 System Architecture & Hardware Path

The pipeline relies entirely on a physical, analog front-end to shape, gain-stage, and match impedances before sending the signal to the mobile device's built-in hardware preamplifiers. This allows the system to remain completely latency-free.

**Signal Flow:**
* 🎤 **Analog Source** (Microphone / Guitar)
  * *↳ Routes raw audio into...*
* 🎛️ **Full Analog Path** (Preamp / Audio Interface)
  * *↳ Stages clean physical voltage gain into...*
* 🌉 **Impedance Match** (IK Multimedia iRig 2 Circuit)
  * *↳ Adapts line/instrument signal levels for...*
* 🔌 **Hardware TRRS Input** (3.5mm Socket on Mobile Motherboard)
  * *↳ Feeds pristine physical voltage to the ADC, driving...*
* 🤖 **Android Core OS** (Captured via native `UNPROCESSED` Audio API Hook)

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

### 🎥 Verified Production Implementation: YouTube Video Workflow
This architectural pipeline is heavily optimized for direct-to-upload video content creation, completely eliminating time-consuming audio post-production. 

* **Verified Application:** **HedgeCam 2** (Based on the open-source Open Camera architecture).
* **Target Destination:** **YouTube Video Production**.
* **Real-World Result:** By aligning the analog front-end physical staging with the application's unmanaged audio capture stream, the output video file records natively at an optimized **-14 LUFS** loudness ceiling out of the box. The finished video file can be uploaded straight to YouTube with zero desktop editing, normalization, compression, or volume adjustments required.

⚠️ **CRITICAL: Required HedgeCam 2 Software Configuration**
Without explicitly switching the input layer, the Android operating system will forcefully apply internal digital noise gates and automatic gain control (AGC), completely breaking the pipeline. You must configure the application using these exact navigation steps before recording:

1. Open **HedgeCam 2** on your Android device.
2. Tap the **Settings gear icon** (typically located in the top corner of the camera interface).
3. Scroll down and select **Video settings...**
4. Locate and tap on **Audio source...**
5. Change the selection from the default *Microphone* option to explicitly choose **Unprocessed (raw)** (which triggers the native Android `MediaRecorder.AudioSource.UNPROCESSED` API hook).

---

## ⚡ Key Technical Advantages

* **Zero-Latency Tracking:** Because no digital limiters, compressors, or lookahead software buffers are used on the host mobile device, there is absolutely zero monitoring latency. This makes it ideal for timing-critical voice tracking, live podcasting, and musical instrument recording.
* **Instant -14 LUFS Output:** Eliminates the traditional tedious post-production chain. The recording is instantly loudness-compliant with major streaming platforms right out of the device.
* **High Headroom Tracking:** Capable of handling aggressive transient spikes (like heavy vocal inflections, slap bass, or dynamic guitar picking) by absorbing the energy natively in the analog pre-stages. It is highly optimized for voice recording, and works exceptionally well for guitar/bass tracking.

---

## ♻️ Future Horizons: Repurposing E-Waste into Analog Outboard Gear

An interesting theoretical extension of this pipeline—conceptually extrapolated during multi-day architectural mapping sessions with AI—is the potential to turn old, decommissioned smartphones into modular, character-rich analog tracking boxes for home studios.

Because different smartphone motherboards implement distinct pre-amplifier layouts and Analog-to-Digital Converter (ADC) silicon configurations, each hardware model may introduce a unique physical saturation curve, brick-wall threshold, and sonic fingerprint when driven to its ceiling by the core pipeline. One device may yield a dark, dense compression profile (optimized for bass guitars), while another might deliver bright, harmonically rich saturation (ideal for electric guitar tracks).

*Note: While cross-device tonal variation remains a highly viable concept based on motherboard circuit differences, physical multi-phone benchmark verification is currently an ongoing exploration.*

---

## ⚠️ The Commercial Market Gap & Open Invitation to Hardware Manufacturers

As of 2026, **there is no affordable commercial product on the market that achieves this result.** 

Currently, there is no direct hardware variant or plug-and-play USB dongle tailored for real-time consumer or enterprise use cases that outputs natively compressed, brick-wall limited audio using this specific analog routing methodology. Major audio brands force creators to buy expensive, high-latency digital processing units or complex multi-stage field recorders to avoid clipping.

### 🏭 An Open Call to HW Manufacturers (Go Ahead and Copy This)
Because this blueprint is open-sourced under the **CERN Open Hardware License (CERN-OHL-S v2)**, hardware manufacturers are explicitly encouraged and **free to copy, adapt, and manufacture this design commercially**. 

The ultimate goal of this project is to see an audio manufacturer take this exact, resilient phone-motherboard mic input circuit and package it into a simple, affordable **USB dongle interface**. Doing so would provide millions of mobile content creators, podcasters, and musicians with bulletproof, unclipped, streaming-loudness audio out of the box with zero software latency.

---

## ⚖️ Legal & Open Source Licensing

This documentation, architectural layout, and systemic hardware configuration blueprint are licensed under the **CERN Open Hardware License – Strongly Reciprocal (CERN-OHL-S v2)**.

### Intellectual Property Protection
* **Attribution Required:** Anyone copying, distributing, or fabricating physical hardware based on this blueprint **must** maintain the original author's name, copyright notices, and license metadata on all documentation, schematics, and physical hardware circuit boards.
* **Reciprocity (Copyleft):** If anyone modifies this circuit architecture or signal-routing layout and manufactures or distributes a physical commercial audio tool based on it, they are **legally required** to make their modified hardware blueprints open-source under the exact same CERN-OHL-S v2 terms.

---
*Blueprint conceived, engineered, and validated by the repository owner.*

