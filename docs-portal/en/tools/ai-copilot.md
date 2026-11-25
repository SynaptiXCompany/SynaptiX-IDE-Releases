# SynaptiX AI Copilot (`Ctrl+Space`)

<div class="doc-meta-bar">
  <span><strong>Document ID:</strong> SYNX-DOC-TOOL-005</span>
  <span><strong>Shortcut:</strong> <code>Ctrl+Space</code></span>
  <span><strong>Capabilities:</strong> Context-Aware Tag, Rule, Timer, Modbus &amp; MQTT Graph Synthesis</span>
  <span><strong>Safety Model:</strong> Preview-Before-Commit &bull; Mandatory Human-in-the-Loop Verification</span>
</div>

**SynaptiX AI Copilot** is an integrated industrial engineering assistant docked inside SynaptiX IDE. Press `Ctrl+Space` from any workspace to slide open the 380 px Copilot side panel, describe your automation requirement in plain English or Vietnamese, and let Copilot synthesize typed tags, 4-stage control rules, IEC timers/counters, linear calibrators, RS-485 Modbus pollers, and MQTT JSON telemetry pipelines directly onto your project.

![SynaptiX AI Copilot Side Panel & Graph Synthesis](/images/synaptix_ai_copilot.png)

---

## 1. Initial Setup & Workspace Context Awareness

### 1.1 API Key, Multi-Provider Architecture & Credential Protection
1. Press `Ctrl+Space` to open the **SynaptiX AI** panel and click the **[Settings]** gear button in the panel header.
2. **Provider Selection:**
   - **Google Gemini:** Choose low-latency models (`gemini-3.8-flash` default, `gemini-3.7-flash`, or `gemini-3.6-flash`).
   - **OpenAI & Compatible Endpoints:** Native support for `OpenAI (GPT-4o, GPT-4o-mini)`, `Azure OpenAI`, or local OpenAI-compatible inference servers (Ollama, vLLM, LMStudio) for air-gapped industrial facilities.
3. **Hardware-Backed Credential Protection (Windows DPAPI):**
   - API keys and connection credentials are automatically encrypted via Windows DPAPI (`CredentialProtectionService`) with `CurrentUser` scope.
   - Secrets are masked on the UI and never leaked to diagnostics logs or embedded into shared `.splc` project files.

### 1.2 Live Project Context Bar & Suggestion Chips
Unlike generic web chatbots that have no visibility into your hardware I/O layout, SynaptiX AI Copilot continuously inspects your live workspace context:
- **Device Resource Profile:** Respects the exact `DI`, `DO`, `AI`, `VREG`, `VREG_RETAIN`, and `COUNTER` limits of your active [Device Preset](/concepts/ecosystem-overview).
- **Live Context Telemetry:** Displays active **Node Count**, **Tag Catalog Count**, **Compiler Status** (including any active diagnostic error codes so Copilot can explain and fix them), and **Currently Selected Nodes** on the canvas.
- **Dynamic Suggestion Chips:** Offers one-click contextual prompts tailored to whether your canvas is empty, contains uncompiled errors, or has specific blocks selected.

---

## 2. What AI Copilot Can Synthesize

<div class="synx-node-chain" style="margin-bottom: 12px;">
  <div class="synx-canvas-node is-step">
    <div class="synx-node-header">
      <div class="synx-node-header-left">
        <svg class="synx-node-icon" viewBox="0 0 16 16"><path d="M2,8 L12,8 M8,4 L12,8 L8,12" /></svg>
        <span>STEP 1</span>
      </div>
      <span class="synx-node-status">PROMPT</span>
    </div>
    <div class="synx-node-body">
      <div class="synx-node-pin is-in" style="visibility: hidden;">● In</div>
      <div class="synx-node-content">
        <span class="synx-node-tag">Natural Spec</span>
        <span class="synx-node-alias">Functional Prompt</span>
        <span class="synx-node-val">English / Vietnamese</span>
      </div>
      <div class="synx-node-pin is-out">Out ●</div>
    </div>
  </div>

  <div class="synx-node-wire">
    <div class="synx-wire-line"></div>
    <span class="synx-wire-badge">Infer</span>
  </div>

  <div class="synx-canvas-node is-step">
    <div class="synx-node-header">
      <div class="synx-node-header-left">
        <svg class="synx-node-icon" viewBox="0 0 16 16"><path d="M3.5,6 L8,9 L3.5,12 M9.5,7 L13,7 M9.5,10.5 L13,10.5" /></svg>
        <span>STEP 2</span>
      </div>
      <span class="synx-node-status">REASONING</span>
    </div>
    <div class="synx-node-body">
      <div class="synx-node-pin is-in">● In</div>
      <div class="synx-node-content">
        <span class="synx-node-tag">AI Inference</span>
        <span class="synx-node-alias">Context &amp; Budget</span>
        <span class="synx-node-val">Hardware Target Limits</span>
      </div>
      <div class="synx-node-pin is-out">Out ●</div>
    </div>
  </div>

  <div class="synx-node-wire">
    <div class="synx-wire-line"></div>
    <span class="synx-wire-badge">Generate</span>
  </div>

  <div class="synx-canvas-node is-step">
    <div class="synx-node-header">
      <div class="synx-node-header-left">
        <svg class="synx-node-icon" viewBox="0 0 16 16"><path d="M8,1.5 L13.5,4 L13.5,9.5 Q13.5,13.5 8,15 Q2.5,13.5 2.5,9.5 L2.5,4 Z" /></svg>
        <span>STEP 3</span>
      </div>
      <span class="synx-node-status">SYNTHESIS</span>
    </div>
    <div class="synx-node-body">
      <div class="synx-node-pin is-in">● In</div>
      <div class="synx-node-content">
        <span class="synx-node-tag">Project Synthesis</span>
        <span class="synx-node-alias">FBD Logic Network</span>
        <span class="synx-node-val">Tags • Rules • Blocks</span>
      </div>
      <div class="synx-node-pin is-out">Out ●</div>
    </div>
  </div>

  <div class="synx-node-wire">
    <div class="synx-wire-line"></div>
    <span class="synx-wire-badge">Preview</span>
  </div>

  <div class="synx-canvas-node is-step is-done">
    <div class="synx-node-header">
      <div class="synx-node-header-left">
        <svg class="synx-node-icon" viewBox="0 0 16 16"><path d="M2,8 L4,8 M12,8 L14,8 M4,4.5 A3.5,3.5 0 1 1 4,11.5" /></svg>
        <span>STEP 4</span>
      </div>
      <span class="synx-node-status is-active">● DIFF VIEW</span>
    </div>
    <div class="synx-node-body">
      <div class="synx-node-pin is-in">● In</div>
      <div class="synx-node-content">
        <span class="synx-node-tag">Proposal Preview</span>
        <span class="synx-node-alias">Diff Inspection</span>
        <span class="synx-node-val">Apply • Ctrl+Z Undo</span>
      </div>
      <div class="synx-node-pin is-out" style="visibility: hidden;">Out ●</div>
    </div>
  </div>
</div>

| Synthesis Domain | Generated Artifacts |
| :--- | :--- |
| Tag Catalog Provisioning | Allocates unused `DI`, `DO`, `AI`, `VFLAG`, `VREG`, or `VREG_RETAIN` slots, enforces `BOOL` / `INT32` / `REAL` physical types, and populates descriptive aliases and units. |
| Interlocked Control Loops | Builds complete `INPUT -> TRIGGER -> COMPARATOR -> GUARD -> ACTION` chains with proper `Set` / `Reset` latching and E-Stop permissive gating. |
| Timing, Counting, Scaling & Math | Wires [`TON`](/blocks/ton) debounce delays, [`TOF`](/blocks/tof) cooling overruns, [`TP`](/blocks/tp) fixed pulses, [`CTU`](/blocks/ctu) batch counters, [`SCALE`](/blocks/scale) two-point linearizers, and [`MATH`](/blocks/math) arithmetic ALU blocks. Supports multi-branch Fan-Out wiring to multiple Action targets. |
| RS-485 & Direct Hardware Actuation | Generates [`MB_READ`](/blocks/mb-read) polling, [`MB_WRITE`](/blocks/mb-write) registers, and [`DIRECT_WRITE`](/blocks/direct-write) high-speed GPIO/PWM pins aligned with physical field devices. |
| IIoT Edge Telemetry & MQTT | Generates [`MQTT_PUB`](/blocks/mqtt-pub) multi-field JSON builders and [`MQTT_SUB`](/blocks/mqtt-sub) command decoders with automated topic, QoS, and schema extraction. |
| Compiler Error Remediation | Explains structural grammar or write-conflict diagnostics in plain language and proposes corrected wiring. |

### 2.1 Industrial Peripheral Intelligence (`PeripheralTemplateRegistry`)
Rather than hallucinating registers, Copilot accesses a built-in repository of verified industrial device templates via `PeripheralTemplateRegistry`:
- **Smart Energy Meters:** Maps 3-phase Voltage ($V$), Current ($A$), Active Power ($kW$), Power Factor ($PF$), and Energy Accumulators ($kWh$) for Schneider, Selec, and Acrel meters.
- **Variable Frequency Drives (VFD):** Wires standardized Control Words, Frequency References, Output Currents, and Fault Feedback registers for Delta, INVT, Yaskawa, and Danfoss inverters.
- **Transmitters & Environmental Sensors:** Pre-engineered templates for RTD/PT100 temperature probes, relative humidity, pressure transducers, and standard `4..20 mA` / `0..10 V` analog conversions.

### 2.2 5 Industrial Architecture Archetypes
Copilot automatically structures user requirements into industry-proven design patterns:
1. **State Machine / Sequence:** Sequences multi-step processes (`Step 1` $\rightarrow$ `Step 2` $\rightarrow$ `Step N`) with step-transition flags (`VFLAG`) and status registers (`VREG`).
2. **Safety Interlock & Alarm:** Enforces fail-safe E-Stop circuits, high-limit overrides, thermal overloads, and alarms with latching mechanisms.
3. **Analog Regulation:** Pairs analog inputs (`AI`), linear scalers (`SCALE`), arithmetic operations (`MATH`), and dual-threshold comparators with hysteresis to prevent relay chattering.
4. **Edge Telemetry Ingestion:** Optimizes Modbus polling intervals and groups contiguous registers into compact JSON payloads for bandwidth-efficient MQTT transmission.
5. **Recipe & Batch Execution:** Binds critical production parameters to non-volatile retain registers (`VREG_RETAIN`) that persist across power cycles.

### 2.3 Draft Graph Transactions
All synthesis proposals are staged inside an isolated **Draft Graph Transaction**:
- Staged logic is validated against the active hardware profile before touching the canvas.
- Engineers can review a visual diff preview before accepting changes.
- Applying proposals creates a single atomic history state—press `Ctrl+Z` to rollback anytime.

---

## 3. Industrial Prompt Engineering Patterns

Ambiguous prompts produce generic logic. Writing structured engineering prompts that state **1) Tag Pins & Types**, **2) Engineering Units & Thresholds**, **3) Hysteresis / Dwell Timing**, and **4) E-Stop / Fault Interlocks** yields production-grade schematics on the first pass:

| Prompt Dimension | Weak / Ambiguous Prompt | Strong Industrial Engineering Prompt |
| :--- | :--- | :--- |
| Pump Pressure Control with Dwell & E-Stop | *"Turn on the relief valve when pressure gets too high."* | *"Scale analog input `AI0` (`4..20 mA`) to `0.0..16.0 bar` into `VREG0` (`BoilerPressure_Bar`). If `VREG0 >= 12.5 bar` continuously for `5 s` (`TON`), energize relief valve `DO0`, gated by active-high E-Stop healthy contact `DI2 == 1`. Reset `DO0` when `VREG0 <= 10.5 bar`."* |
| Post-Stop Cooling Fan Overrun | *"Keep cooling fan running for a bit after the motor stops."* | *"Control main spindle `DO0` with Start pushbutton `DI0` (rising edge) and Stop pushbutton `DI1`, interlocked by thermal overload healthy `DI3 == 1`. Drive cooling fan `DO1` via a `TOF` off-delay timer so `DO1` stays ON during operation and continues running for `15 s` after `DO0` turns OFF."* |
| RS-485 Sensor to MQTT Telemetry | *"Read temperature from Modbus and send to MQTT."* | *"Poll Slave ID `2` on `RS-485 #1` Holding Register `40001` (`INT16`, scale `0.1`) into `VREG1` (`ChamberTemp_C`) and `40002` (`UINT16`, scale `0.1`) into `VREG2` (`ChamberHum_Pct`) every `1000 ms`. Publish both fields with device ID and UTC timestamp every `10 s` (`QoS 1`) to MQTT topic `plant/oven1/telemetry`."* |

::: tip Iterative Refinement & Full Undo (`Ctrl+Z`)
Every Copilot generation surfaces as a reviewable proposal (`HasPendingAiProposal`) with auto-layout node placement. Even after accepting a generated circuit onto the [Logic Graph (`Ctrl+2`)](/workspaces/logic-graph-tab), the entire multi-node insertion is recorded as a single atomic transaction on the undo stack—press `Ctrl+Z` at any time to roll back cleanly.
:::

---

## 4. Mandatory Human-in-the-Loop Verification Checklist

::: danger Safety-Critical Engineering Rule (IEC 61508-3)
AI-generated control logic is an engineering draft, never a substitute for qualified commissioning verification. You **MUST** complete every item in the 4-step verification checklist below before executing **Atomic Deploy (`Ctrl+F5`)** to physical machinery!
:::

1. **Inspect Physical Tag Types & Polarity (`Ctrl+3`):**
   - Confirm that Normally-Closed (`NC`) hardware safety circuits (E-Stop mushroom buttons, thermal overload relays, low-level float switches) match the exact `== 1` (healthy closed loop) vs. `== 0` (broken wire / tripped) polarity wired in your electrical panel!
   - Verify `INT32` vs. `REAL` (`Float32`) assignments in the [Tag Catalog](/workspaces/tag-catalog-tab) so fractional thresholds (such as `12.5 bar`) are never truncated into integer registers.
2. **Execute Deterministic Compilation (`F7`):**
   - Press `F7` and confirm **0 Errors / 0 Write Conflicts** in the compiler status bar and [Rule Table (`Ctrl+4`)](/workspaces/rule-table-tab).
3. **Run Nominal & Adversarial Offline Simulation (`F5`):**
   - Launch the [Offline Simulator (`F5`)](/tools/simulator-and-live-debug) and verify **three mandatory scenarios**:
     - *Happy Path:* Normal start/stop sequence and threshold crossing after full `TON` dwell duration.
     - *Boundary / Chatter Rejection:* Brief spikes shorter than `TON.PT` or within hysteresis deadband do **not** falsely trip actuators.
     - *Fault / E-Stop Injection:* Dropping the E-Stop or safety `GUARD` contact immediately drives actuators to their safe de-energized state (`0`).
4. **Two-Phase Atomic Deploy & Live Observation (`Ctrl+F5` -> `F8`):**
   - Deploy via [Atomic Deploy (`Ctrl+6`)](/workspaces/deploy-tab) with mechanical loads isolated first, then engage **Online Live Debug (`F8`)** to confirm real sensor polarities before enabling live actuators.

---

## Related Documentation

- [Logic Graph Canvas (`Ctrl+2`)](/workspaces/logic-graph-tab)
- [Blueprints Library (`Ctrl+5`)](/workspaces/blueprints-tab)
- [Simulator & Online Live Debug (`F5` / `F8`)](/tools/simulator-and-live-debug)
- [Integration Application Notes](/tools/application-notes)