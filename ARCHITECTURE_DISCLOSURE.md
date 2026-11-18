# TECHNICAL DISCLOSURE & PRIOR ART SPECIFICATION
## Title: Heterogenous neuromorphic claustral multiplexing, hardware-pacemaker phase-locking, and autonomous slow-wave consolidation architecture for embodied systems

**Document Type:** Defensive publication / Enabling prior art disclosure (35 U.S.C. § 102 / CPC G06N 3/04, G06N 3/063, B25J 9/16, G10L 13/02)
**Date of public disclosure:** November 16, 2026
**Author / Inventory:** Shawn Allen Guenther
**License:** Code licensed under GNU GPLv3; Hardware designs licensed under CERN-OHL-S v2.0. Dedicated to the public domain as prior art to bar third-party patent monopolies.

---

### 1. ABSTRACT

Disclosed herein is a distributed hardware and software architecture for an embodied, continuous-time synthetic cognitive agent ("Tetrapus") modeled on mammalian thalamocortical-claustral circuitry, avian / mammalian vocal-mirror loops, and cephalopod somatic ganglia distribution. Unlike conventional feed-forward artificial intelligence architectures (i.e., stateless transformers executing directed acyclic graphs), the disclosed system instantiates a persistent, capacity-limited central routing bottleneck ("Synthetic Claustrum") driven by a deterministic hardware crystal pacemaker ("Thalamic clock"). The Synthetic Claustrum executes a continuous time-recurrent network (specifically a Liquid Time-Constant / Closed-form Continuous-time network) that ingests its own prior hidden state vector ($H_{t-1}$) and prior routing mask ($M_{t-1}$) alongside real-time physical compute telemetry ("Introceptive Insula") and peripheral actuator torque-error interrupts ("Somatic Ganglia"). Using time-division multiplexing via a Dynamic Top-K sparse gating mechanism ($K \in [2, 3]$), the router dynamically phase-locks a small subset of peripheral sensory, motor, and vocal-associative  nodes with a heirarchical Predictive Coding / Active Inference cortex during a high-frequency waking oscillation (i.e., 40 Hz). The architecture further incorporates a non-pre-trained Cochlear-Syrinx Auditory-Motor Mirror Loop that self-organizes acoustic experiences into discrete proto-phoneme clusters and maps them to continuous formant vocal synthesis parameters and behavioral inhibition priors. When internal homeostatic telemetry (battery voltage depletion, thermal load, synaptic weight saturation or prediction-error stagnation) crosses a critical threshold, the system autonomously transitions into an offline "Slow-Wave Sleep" state (i.e., 1 Hz clock step-down), physically severing the recurrent self-monitoring loop ($H_{t-1}$) and sensory-motor gates while executing local Forward-Forward positive /negative synaptic consolidation from a non-volatile episodic vector archive ("Hippocampal Storage").

---

### 2. BACKGROUND & TECNICAL PROBLEM SOLVED

Conventional machine learning and robotic control architectures suffer from four structural limitations that prevent continuous, self-referential adaptive agency and authentic developmental symbol grounding on low-power edge hardware:
1. **Feed-Forward Statelessness:** Standard neural networks execute unidirectional forward passes without persistent state mutation or recurrent self-observation within a single clock cycle or invocation, lacking a unified locus of executive attention.
2. **Absence of Physical & Social Interoception:** Existing cognitive architectures treat compute hardware metrics (voltage, current draw, thermal dissipation, bus latency, memory pressure) as external engineering constraints rather than intrinsic homeostatic valence signals driving the neural control policy itself.
3. **Un-Grounded Pre-Trained Language Tokens:** Conventional conversational agents rely on static, pre-trained text tokenizers detached from physical sensorimotor consequences, preventing the agent from autonomously deciding whether and how to develop vocal mimicry, instrumental communication, or context-dependent motor inhibition ("behavioral etiquette").
4. **Catastrophic Forgetting in Continuous Online Learning:** Continuous-time networks that adapt weights online via Hebbian or local gradient rules rapidly saturate unless paired with an autonomous, rhythmically-gated offline consolildation phase that recalibrates network weights without global backpropagation.

---

### 3. SYSTEM ARCHITECTURE & HARDWARE TOPOLOGY (REFERNCE EMBODIMENT)

The reference embodient comprises an untethered, radially symmetric four-appendage mobile robotic platform ("Tetrapus") housing a two-tier deterogenous compute cluster operating on an isolated, air-gapped local network:

#### 3.1 Tier 1: Subcortical & Somatic Ganglia Ring (bare-metal / RTOS microcontrollers)
Seven (7) determinstic microcontroller units (MCUs, i.e., Raspberry Pi Pico 2 RP2350 or ESP32-S3) operating at 500 Hz to 1,000 Hz without operating system kernel scheduling jitter, aggregated via a powered USB 3.0 / UART bus:
* **Node G1 - Thalamic Hardware Pacemaker:** Generates a deterministic hardware timer interrupt pulse distributed via physical GPIO interrupt lines to all system nodes. Operates bimodally: **Mode A (Waking Gamma/Beta):** 10 Hz to 40 Hz (25 ms period); **Mode B (Slow-Wave Sleep Delta):** 0.5 Hz to 4 Hz (1,000 ms period).
* **Node G2 - Interoceptive Insula & Brainstem Node:** Polls system-wide physical telemetry via I2C / ADC / UART (i.e., INA3221 multi-channel current / voltage monitors across a 12.8V LifeP04 rail and 5.1V / 15A buck rail, CPU core thermals, and RAM pressure) alongside external social reward / correction triggers. Computes a normalized 4-dimensional **Homeostatic Valence Vector** ($V_t \in [0, 1]^4$) representing `[Battery_Hunger, Thermal_Pain, Current_Strain, Social_Discomfort]`.
* **Nodes G3, G4, G5, G6 - Articulated Appendage (Tentacle #1-#4) Ganglila:** Each MCU manages a dedicated 1 Mbps TTL UART serial bus of smart actuators (i.e., 3x Feetech STS3215 12V 30 kg-cm serial bus servos per appendage via a dedicated bus driver board, totaling 12 appendage servos) at 500 Hz. Each ganglion continuously reads real-time 12-bit magnetic angular position, angular velocity, winding temperature, and electrical current draw (torque load), executes a local <2 ms hardware refelx loop (backing off actuator torque upon current-spike collision detection), and computes a local **Prprioceptive Surprise Scalar** ($S_i = \vert{}P_{\text{expected}} - P_{\text{actual}}\vert{} + \alpha \cdot I_{\text{torque}}$).
* **Node G7 - Locomotion & Vestibular Ganglion:** Interfaces with a 3-wheel omni-directional holonomic drive base (3x Continuous-rotation STS3215 serial bus servos) and a 9-DOF Intertial Measurement Unit (i.e., Bosch BNO085) providing hardware-fused gravity vector, quaternion orintation, tap / collision detection, and linear acceleration telemetry.

#### 3.2 Tier 2: Claustral, Sensory, Predictive, Hippocampal, and Vocal-Associative Nodes (Linus SBCs)
Five (5) single-board computers (i.e., Raspoberry Pi 5 boards scaled by memory footprint: 2x 2GB RAM, 2x 4GB RAM, and 1x 8GB RAM) interconnected via an air-gapped 8-port 1GbE switch using ZeroMQ message passing:
* **Node C1 (2GB RAM) - Synthetic Claustrum Router:** Executes a continuous-time **Liquid Time-Constant (LTC) / Closed-form Continuous-time (CfC)** neural network (`claustrum_router.py`) across $N=8$ peripheral modalities triggered at 40 Hz.
* **Node C2 (2GB RAM) - Multimodal Visual & Cochlear Sensory Cortex:** Interfaces with a stereo depth VPU camera (i.e., Luxonis OAK-D Lite) and a 4-microphone digital array (i.e., ReSpeaker 4-Mic Array). Runs a real-time **64-band log-Mel biological Cochleagram transform** (`cochlear_sensory_node.py`), extractingspectral-flux onset surprise and binaural cross-correlation Interaural Time Delay (ITD) sound azimuth every 25 ms without pre-trained speech-to-text tokenizers.
* **Node C3 (4GB RAM) - Predictive Cortex & Active Inference Planner:** Executes a hierarchical **Forward-Forward Predictive Coding Network** coupled with an **Active Inference Expected Free Energy ($G(\pi)$) policy planner** (`world_model.py`) and an onlines **Sensorimotor Contingency Tracker** separating `SELF` chnnels from `EXTERNAL_WORLD` channels.
* **Node C4 (8GB RAM) - Hippocampal Episodic Archive & Sleep Engine:** Utilizes 8GB LPDDR4X RAM to maintain an in-memory HNSW vector graph (Qdrant) backed by a PCIe Gen 3 NVMe SSD to record high-surprise episodic trajectories, maintain a semantic concept graph, and stream positive / negative replay batches during Slow-Wave Sleep.
* **Node C5 (4GB RAM + 26 TOPS PCIe NPU Accelerator) - Frontal Associative & Vocal-Motor Hub:** Interfaces with an I2S audio amplifier and speaker (`vocal_associative_cortex.py`).
Executes:
  1. A continuous-time **4-Muscle Parametric Formant & Glottal Syrinc Synthesizer** controlled by a normalized vector $U_{\text{vocal}} = [F_{0, \text{Pitch}}, A_{\text{Breath}}, F_{1, \text{Tongue}}, F_{2, \text{Lips}}] \in [0, 1]^4$ driving a Rosenberg glottal pulse and aspiration noise source through cascaded 2nd-order digital biquad resonators.
  2. An online **Kohonen Self-Organizing Map (SOM) / Vector-Quantized Autoencoder** (64 proto-phoneme clusters) that clusters 64-band cochlear frames from Node C2 in real time.
  3. A **Hebbian Auditory-Motor Mirror Matrix** that binds self-generated acoustic clusters heard via Node C2 back to the motor vector $U_{\text{vocal}, t-1}$ that produced them.

---

### 4. CORE OPERATIONAL MECHANICS & DATA FLOW

#### 4.1 Dynamic Tri-Modal Claustral Multiplexing (Waking State)
On each 25 ms waking clock tick $t$:
1. **8-Modality Subcortical & Cortical Aggregation:** Node C1 ingests 32-byte telemetry packets from all $N=8$ peripheral modallities:
  * `Modality 0`: Node G2 (Insula Homeostasis & Social Valence)
  * `Modalities 1-4`: Nodes G3-G6 (Tentacles #1-#4)
  * `Modality 5`: Node G7 (Omni-Wheels & Vestibular IMU)
  * `Modality 6`: Node C2 (OAk-D Vision + 64-Band Cochlear Summary)
  * `Modality 7`: Node C5 (Frontal / Vocal Syrinx & SOM Cluster State)
2. **Recurrent Self-Ingestion:** Node C1 constructs a 48-float input tensor $X_t$ comprising the 32 modalilty state floats ($8 \times 4$), the 8 per-modality surprise scalars, and the 8-element prior routing mask $M_{t-1}$, and passes $X_t$ alongside its own recurrent hidden state $H_{t-1} \in \mathbb{R}^{96}$ into the CfC/LTC cell:
   $$H_t = \text{CfCCell}(X_t, H_{t-1}), \quad \text{Logits}_t = W_{\text{gate}} H_t + \beta \cdot S_{\text{surprise}, t}$$
3. **Salience-Modulated Dynamic Top-K Gate ($K=2 \rightarrow K=3$):** During baeline locomotion and tactile exploration, Node C1 enforces a strict bandwidth cap of $K_{\text{base}} = 2$. Whenever acoustic or vocal surprise on Modallity 6 or Modality 7 exceeds a threshold $\theta_{\text{audio}}$, the gate dynamically expands to $K_{\text{max}} = 3$:
   $$K_t = \begin{cases} 3 & \text{if } \max(S_6, S_7) \ge \theta_{\text{audio}} \\ 2 & \text{otherwise} \end{cases}$$
   This enabled simultaneous tri-modal phase-locking across `[Visual Object + Tactile Tentacle + Acoustic / Vocal Contour]` for ostensive symbol grounding while preventing full-network seizure synchronization.

#### 4.2 Enactive Self-World Separation, Vocal Grounding, and Behyaviorall Etiquette
1. **Sensorimotor Contingency (`SELF` vs. `EXTERNAL_WORLD`):** For each phase-locked modality $m \in M_t$, Node C3 updates an exponential moving contingency score $C_m \in [0, 1]$ measuring how reliably motor command $U_{m, t-1}$ predicts state transition $\Delta S_{m, t}$:
   $$C_{m, t} = (1 - \lambda) C_{m, t-1} + \lambda (1 - \Vert{}S_{m, t} - \hat{S}_{m, t}\Vert{}_2) \quad \text{for } \Vert{}U_{m, t-1}\Vert{}_2 > \epsilon$$
   Modalities maintaining $C_m \ge 0.5$ (tentacles, wheels, vocal syrinx) are flagged as `SELF`; perturbations yielding low contingency ($C_m < 0.5$) are classified as `EXTERNAL_WORLD` agents or obstacles.
2. **Self-Supervised Vocal Babbling & Mirror Imitation:** When Modality 7 (Node C5) is phase-locked into the active locus, Node C5 emits a 25 ms formant waveform synthesized from $U_{\text{vocal}, t-1}$. On tick $t$, Node C2 captures the resulting room acoustics, and Node C5's SOM maps the 64-band cochleagram to Best Matching Unit cluster $b^*$. A local Hebbian update binds cluster $b^*$ to $U_{\text{vocal}, t-1}$:
   $$W_{\text{mirror}}[b^*] \leftarrow W_{\text{mirror}}[b^*] + \eta_{\text{mirror}} \left(U_{\text{vocal}, t-1} - W_{\text{mirror}}[b^*]\right)$$
   Depending on which policy minimizes Expected Free Energy $G(\pi)$, the agent autonomously converges on vocal mimicry (reducing auditory prediction error), passive acoustic comprehension (linking human acoustic clusters to predicted physical consequences), or instrumental vocalization (emitting acoustic contours that actuate external caregivers to relieve homeostatic hunger / strain).
3. **Inhibitory Etiquette Priors ("Synthetic Superego"):** Because Expected Free Energy $G(\pi)$ penalizes predicted edeviations from preferred homeostatic vector $V_{\text{pref}} = \mathbf{0}$ (which includes `Social Discomfort` on Node G2), negative social valence spikes paired with acoustic tokens or visual proximity consolidate during Slow-Wave Sleep into top-down inhibitory priors that automatically dampen actuator velocity / torque around protected entities.

#### 4.3 Autonomous Slow-Wave Sleep Step-Down & Local Forward-Forward Consolidation when battery hunger or synaptic weigth norm saturation triggers `SLEEP_SYNC`:
1. **Observer Severance:** Node C1 zeroes its recurrent self-state ($H_{t-1} = \mathbf{0}$) and broadcasts a closed routing mask (`node_id = 255`), silencing all actuators and the vocal syrinx.
2. **Layer-Wise Forward-Forward Updates:** At the 1 Hz sleep clock, Node C4 streams positive waking episodes ($X_{\text{pos}}$) and shuffled / hallucinated negative counterfactuals ($X_{\text{neg}}$) to Node C3. Each `ForwardForwardLayer` updates its weights locally without global backpropagation by pusing squared-activation goodness $g(h) = \frac{1}{D}\sum_j h_j^2$ above threshold $\theta_{\text{FF}}$ for $X_{\text{pos}}$ and below $\theta_{\text{FF}}$ for $X_{\text{neg}}$:
    $$\mathcal{L}_{\text{FF}} = \log\left(1 + e^{-(g(h_{\text{pos}}) - \theta_{\text{FF}})}\right) + \log\left(1 + e^{-(g(h_{\text{neg}}) - \theta_{\text{FF}})}\right)$$

---

### 5. BINARY TELEMETRY & CONTROL PACKET SCHEMAS

All inter-node packets use deterministic little-endian packed binary structures:

```c
// 1. 32-Byte Subcortical & Sensory Telemetry Frame (Nodes G2...G7, C2, C5 -> Node C1)
typedef struct __attribute__((packed)) {
    uint32_t tick_id;           // Monotonic hardware clock counter
    uint8_t  node_id;           // Modality index (0...7)
    uint8_t  mode_flags;        // Bit 0: Wake(1) / Sleep(0), Bit 1: Reflex, Bit 2: Phase_Locked
    uint16_t surprise_q15;      // Normalized prediction / torque / flux error (0...65535 -> 0.0...1.0)
    float    state_vec[4];      // Modality state (Angles, IMU, Valence V_t, or Vocal State)
    float    delta_vec[2];      // Velocity / current draw / pitch-amplitude delta
    uint8_t  crc8;              // Checksum
} ClaustrumFrame32_t;

// 2. 24-Byte Dynamic Phase-Lock Routing Mask (Node C1 -> Nodes C3, C5, G3...G7)
typedef struct __attribute__((packed)) {
    uint32_t tick_id;           // Monotonic clock counter
    uint8_t  active_id;         // Dynamic gate width: 2 (Baseline), 3 (Tri-Modal), or 0 (Sleep)
    uint8_t  win_ids[3];        // Winning modality IDs (255 = unused slot / gate closed)
    float    win_weights[3];    // Softmax phase-lock gain weights
} ClaustrumMask24_t;

// 3. 24-Byte Active Inference Motor / Vocal Command (Node C3 -> Target Modalilty)
typedef struct __attribute__((packed)) {
    uint32_t tick_id;           // Monotonic clock counter
    uint8_t  targetnode_id;     // Target peripheral modality (1...7)
    uint8_t  is_self_channel;   // 1 if contingency_score >= 0.5 (SELF), 0 if EXTERNAL_WORLD
    uint16_t surprise_q15;      // Cortical prediction error scalar
    float     action_u[4];      // 4-DOF servo target or 4-muscle Formant Syrinx command
} MotorCommand24_t;
```

---

### 6. ALTERNATIVE EMBODIMENTS & DEFENSIVE BREADTH CLAUSE

To prevent third-party patent circumvention through trivial substitution of components, algorithms, or physical form factors, the disclosed architecture explicitly encompasses the following alternative embodiments:

**Compute Substrates & Memory Configurations:** Applies across any combination of single-board computers (i.e., 1GB / 2GB / 4GB / 8GB / 16GB Raspberry Pi, Radxa, Orange Pi, Intel M100/x86 SBCs, NVIDIA Jeston, Apple Silicon), NPUs (Hailo-8/8L, Coral Edge TPU, Rockchip RK3588), MCUs (RP2040, RP2350, ESP32, STM32, Teensy), FPGAs, neuromorphic ASICs, or single-host virtualized proceses using in-memory or mmap on-disk vector stores.

**Auditory & Vocal Synthesis Modalities:** The audotory-motor loop is not limited to a 64-band Mel cochleagram, Kohonen SOM, and 2-formant Rosenberg glottal synthesizer; it encompasses Gammatone / ERB cochlear filterbanks, VQ-VAEs, spiking cochlear models, Differentiable Digital Signal Processing (DDSP) synthesizers, articulatory vocal-tract physics models, ultrasonic / piezoelectric transducers, or discrete symbol / terminal I/O channels.

**Interconnect, Neural, and Somatic Variations:** Encompasses all bus protocols (ZeroMQ, ROS2, CAN-FD, RS-485, SPI, shared memory), continuous-time neural routers (LTC, CfC, S4, Mamba, RWKV, SNNs), and somatic morphologies (3-arm, 4-arm, hexapod, biped, quadruped, worm, aquatic, or wheeled platforms).

**Clock & Pacemaker Frequencies:** The waking pacemaker frequency is not restricted to 40 Hz and may operate dynamically across any high-frequency band (i.e., 5 Hz to 1,000 Hz) or variable event-driven intervals, while the offline consolidation state may operate at any stepped-down frequency, bistable duty cycle, or asynchronous idle window.