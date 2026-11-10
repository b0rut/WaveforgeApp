<div align="center">
  <a href="https://waveforge-app.com">
    <img src="https://waveforge-app.com/favicon.svg" width="80" height="80" alt="Waveforge Logo" />
  </a>
  <h1 align="center">🎚️ Waveforge</h1>
  <p align="center"><strong>Precision DJ Loudness &amp; 4× True-Peak Mastering Workstation for Touring DJs and Producers</strong></p>
  <p align="center"><em>100% Offline, Local-First ITU-R BS.1770-4 Normalization &amp; Lossless MP3 FrameGain with Immutable Source Protection</em></p>

  <p align="center">
    <a href="https://waveforge-app.com"><img src="https://img.shields.io/badge/Official_Website-waveforge--app.com-00E5C3?style=for-the-badge&logo=safari&logoColor=0A0F0E" alt="Official Website" /></a>
    <a href="https://waveforge-app.com/tools/dj-headroom-calculator.html"><img src="https://img.shields.io/badge/Free_Tool-DJ_Headroom_Calculator-FF6B2B?style=for-the-badge&logo=audacity&logoColor=white" alt="DJ Headroom Calculator" /></a>
    <a href="https://waveforge-app.com/tools/rekordbox-crate-checker.html"><img src="https://img.shields.io/badge/Free_Tool-Rekordbox_Crate_Checker-0EA5E9?style=for-the-badge&logo=files&logoColor=white" alt="Rekordbox Crate Checker" /></a>
  </p>

  <p align="center">
    <img src="https://img.shields.io/badge/Release-v1.3.1-00E5C3?style=flat-square&logo=github" alt="Release v1.3.1" />
    <img src="https://img.shields.io/badge/DSP-64--bit_Float_Rust-orange?style=flat-square" alt="64-bit Float Rust DSP" />
    <img src="https://img.shields.io/badge/Standards-ITU--R_BS.1770--4_%7C_EBU_R128-brightgreen?style=flat-square" alt="ITU-R BS.1770-4" />
    <img src="https://img.shields.io/badge/Platforms-Windows_%7C_macOS_Universal_%7C_Linux-blue?style=flat-square" alt="Windows, macOS Universal, Linux" />
  </p>

  <br />
  <a href="https://waveforge-app.com">
    <img src="docs/screenshots/03-track-inspector-and-meter.png" alt="Waveforge v1.3.1 Track Inspector, BS.1770-4 Meter and Dual Waveform HUD" width="1000" style="border-radius: 12px; box-shadow: 0 14px 40px rgba(0,0,0,0.7); margin-top: 10px; margin-bottom: 20px;" />
  </a>
</div>

---

## 🎚️ What is Waveforge?

**Waveforge** is a standalone, 100% offline desktop audio mastering and crate loudness calibration workstation engineered for touring DJs, club residents, and electronic music producers. Built in memory-safe Rust with 64-bit double-precision DSP, Waveforge eliminates sudden booth volume jumps and mixer DAC clipping across mixed crates of masters from different eras and labels.

Instead of relying on software-only Auto-Gain flags that vanish when you plug a USB stick into standalone club hardware, Waveforge calibrates actual audio headroom against **ITU-R BS.1770-4** integrated loudness (`LUFS_int`) and **Representative DJ Drop Loudness** (`LUFS_rep`) while keeping your original source files completely untouched.

👉 **[Explore Waveforge v1.3.1 at waveforge-app.com](https://waveforge-app.com)** · **[Try Free Online DJ Engineering Tools](https://waveforge-app.com/tools/dj-headroom-calculator.html)**

---

## ⚡ Audio DSP & Safety Architecture

<div align="center">
  <p>
    <img src="https://img.shields.io/badge/WAV-16%2F24%2F32--bit_%26_Broadcast-00E5C3?style=for-the-badge" alt="WAV" />
    <img src="https://img.shields.io/badge/FLAC-Bit--Perfect_Lossless-0EA5E9?style=for-the-badge" alt="FLAC" />
    <img src="https://img.shields.io/badge/AIFF-Club_Standard_PCM-8B5CF6?style=for-the-badge" alt="AIFF" />
    <img src="https://img.shields.io/badge/MP3-Lossless_FrameGain_(1.5_dB)-FF6B2B?style=for-the-badge" alt="MP3 FrameGain" />
    <img src="https://img.shields.io/badge/ALAC_%2F_AAC_%2F_M4A-Supported-22C55E?style=for-the-badge" alt="ALAC and M4A" />
  </p>
</div>

### How Waveforge Processes Your Crate Safely

```text
[Local DJ Crate: WAV / AIFF / FLAC / ALAC / MP3 / M4A] (Strictly Read-Only Source)
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────────────┐
│          64-BIT FLOAT LOUDNESS & INTERSAMPLE ANALYSIS ENGINE            │
│  • ITU-R BS.1770-4 K-Weighting + Dual-Stage Gating (-70 LUFS / -10 LU)  │
│  • Representative DJ Loudness (Upper Energy / Drop Focus LUFS_rep)      │
│  • 4× Oversampled Polyphase Sinc True-Peak Detection (Annex 2 dBTP)     │
└────────────────────────────────────┬────────────────────────────────────┘
                                     │
                     ┌───────────────┴───────────────┐
                     ▼                               ▼
       [Preserve Dynamics Mode]            [Match Target Mode]
       • Pure Linear Scalar Gain           • Full Target Gain Plan
       • 0% Limiting / No Compression      • 4× Lookahead Brickwall Limiter
       • Lossless MP3 global_gain (±1.5dB)   Engages Only Above Ceiling
                     │                               │
                     └───────────────┬───────────────┘
                                     ▼
┌─────────────────────────────────────────────────────────────────────────┐
│          ATOMIC STAGING & INDEPENDENT DISK VERIFICATION GATE            │
│  • Writes to Separate Output Folder via Atomic .tmp File Staging        │
│  • Preserves ID3v2, Rekordbox GEOB, Traktor NML, Vorbis & RIFF Chunks   │
│  • Reopens Every Output File from Disk to Independently Remeasure LUFS  │
└─────────────────────────────────────────────────────────────────────────┘
```

1. **ITU-R BS.1770-4 & Representative Drop Calibration**:
   - Computes both Integrated Loudness (`LUFS_int`) and drop-focused Representative DJ Loudness (`LUFS_rep`) so quiet atmospheric breakdowns do not trick tracks into blowing out the drop.
   - Ships with practical booth presets (`-9.0 LUFS` Peak-Time, `-11.0 LUFS` Club Standard, `-12.0 LUFS` Dynamic Club, `-14.0 LUFS` Streaming) plus a continuous slider.
2. **4× Oversampled True-Peak Protection**:
   - Uses ITU-R BS.1770-4 Annex 2 polyphase sinc interpolation to catch intersample overshoots before they distort club mixer DAC reconstruction stages (`-1.0 dBTP` default ceiling).
3. **Lossless MP3 FrameGain Without Re-Encoding**:
   - For MP3 crates in `PreserveDynamics`, modifies the 8-bit `global_gain` header directly in `1.505 dB` steps—avoiding decode/re-encode generational loss while preserving metadata.
4. **Independent Physical Disk Remeasurement**:
   - Every exported file is reopened directly from storage through a fresh decoder pass to verify actual decodable frames, metadata retention, and post-write LUFS / dBTP compliance.

---

## 🎛️ The 7-Stage DJ Mastering Workflow

```text
[1. Drop Crate] ➔ [2. Gain Plan] ➔ [3. Inspect] ➔ [4. Waveform HUD] ➔ [5. Offline Key] ➔ [6. Disk Audit] ➔ [7. Multi-Deck Mixer]
 Read-Only Scan     LUFS vs dBTP     BS.1770-4      Original vs Out     Ed25519 Signed    5-Gate Receipt    Raw vs Norm A/B
```

| Stage | View / Component | What It Does |
|---|---|---|
| **1. Crate Dropzone** | **Drag & Drop Ingestion** | Scans recursive folders locally for `WAV`, `AIFF`, `FLAC`, `MP3`, `ALAC`, `AAC`, and `M4A` without modifying source files. |
| **2. Batch Planning** | **Loudness & Gain Matrix** | Displays format, sample rate, `LUFS_int`, `LUFS_rep`, Sample Peak, `4×` True Peak (`dBTP`), and transparent gain / headroom shortfall. |
| **3. Track Inspector** | **Hardware AudioMeter** | Real-time BS.1770-4 metering, peak-hold notch, codec telemetry, and inline stereo waveform preview. |
| **4. Waveform HUD** | **Dual-Lane Comparison** | Synchronized Original (cyan) vs. Processed (orange) waveform lanes with `2x`/`4x`/`8x` zoom and `-1.0 dBTP` ceiling lines. |
| **5. Offline Activation** | **Ed25519 Licence Unlock** | One-time local key activation (`WF-XXXXX-...` or signed offline `WF1...` token) with zero cloud dependency during audio DSP. |
| **6. Audit Receipts** | **Disk Verification Gate** | Verifies Decodable frames, Frame Match, Metadata retention (`LIST`/`bext`/`cue`/`id3`), and post-write True Peak; exports JSON & CSV logs. |
| **7. Mixer Preview** | **Multi-Deck A/B Console** | Up to 8 virtual decks (`CH 01–08`), instant **Raw vs. Norm** switching, Intro/Break/Drop jumps, and 10s rolling RMS/Peak breathing scope. |

---

## ✨ Key Capabilities

- 🔒 **100% Local & Air-Gapped Audio Processing**: Unreleased promos, dubplates, and pre-release masters never leave your computer. Zero analytics tracking of your audio files and zero cloud uploads.
- 🛡️ **Strict Source Media Immutability**: Source files are opened strictly read-only. Normalized outputs are written to a dedicated destination directory via atomic `.tmp` staging.
- 💎 **Lossless Bitstream & Full PCM Preservation**: Supports 16/24/32-bit float `WAV` (including Broadcast WAV `bext`), `AIFF`, `FLAC`, `ALAC`, `MP3`, and `M4A` while retaining `ID3v2`, Pioneer `GEOB`, Traktor `NML`, Vorbis comments, RIFF metadata chunks, and embedded artwork.
- 🎚️ **Choice of Dynamics Policy**: Pick **`PreserveDynamics`** for pure linear scalar gain (zero dynamic compression, explicit headroom shortfall reporting) or **`MatchTarget`** when hitting a uniform LUFS target requires transparent lookahead peak limiting.
- 🖥️ **Native Cross-Platform Desktop Builds (`v1.3.1`)**: Built natively for **Windows 10/11 (x64)**, **macOS 12+ Universal** (Apple Silicon M1–M4 & Intel x86_64), and **Linux x64** (`AppImage` & `.deb`).

---

## 🚀 Licensing & Free Online Engineering Tools

| Tier / Offering | Price | Availability | Key Capabilities Included |
|---|---|---|---|
| **Free Online DJ Tools** | **€0.00**<br>*(No Account)* | Web Browser<br>*(Local JS)* | • [**DJ Headroom & Intersample Peak Calculator**](https://waveforge-app.com/tools/dj-headroom-calculator.html)<br>• [**Rekordbox, Traktor & Serato XML Crate Health Checker**](https://waveforge-app.com/tools/rekordbox-crate-checker.html) |
| **Waveforge Lifetime Desktop** | **€34.99**<br>*(One-Time)* | Windows · macOS Universal · Linux | • Unlimited offline batch normalization, `4×` True-Peak limiter & Lossless MP3 FrameGain<br>• Dual-Lane Waveform HUD, 8-Deck Mixer Preview & JSON/CSV disk verification receipts<br>• **2 concurrent workstation activations** with self-service deactivation & key recovery<br>• Free `v1.x` updates & **30-day money-back guarantee** (Stripe, PayPal, Crypto) |

---

## 📸 Workstation Tour (`v1.3.1` Native App Captures)

<div align="center">
  <h3>📂 Empty Dropzone &amp; Multi-Format Ingestion</h3>
  <p><em>Drag tracks or recursive library folders locally with instant target loudness, true-peak ceiling, and mode controls</em></p>
  <img src="docs/screenshots/01-empty-library-dropzone.png" alt="Waveforge Empty Dropzone and Ingestion Setup" width="950" style="border-radius: 10px; box-shadow: 0 8px 28px rgba(0,0,0,0.5); margin-bottom: 28px;" />

  <h3>📊 Batch Loudness &amp; Transparent Gain Planning Matrix</h3>
  <p><em>Inspect Source Integrated LUFS, Representative DJ LUFS, 4× True Peak (dBTP), and exact planned gain per track before processing</em></p>
  <img src="docs/screenshots/02-batch-ingestion-and-planning.png" alt="Waveforge Batch Loudness Analysis and Gain Plan" width="950" style="border-radius: 10px; box-shadow: 0 8px 28px rgba(0,0,0,0.5); margin-bottom: 28px;" />

  <h3>🎛️ Track Inspector &amp; BS.1770-4 AudioMeter</h3>
  <p><em>Per-track loudness breakdown vs. club target, codec telemetry, and embedded artwork inspection</em></p>
  <img src="docs/screenshots/03-track-inspector-and-meter.png" alt="Waveforge Track Inspector and Loudness Meter" width="950" style="border-radius: 10px; box-shadow: 0 8px 28px rgba(0,0,0,0.5); margin-bottom: 28px;" />

  <h3>🔬 Dual-Lane Waveform Comparison HUD</h3>
  <p><em>Decoded Original (cyan) vs. Processed Output (orange) on a shared amplitude scale with 2x/4x/8x zoom and -1.0 dBTP ceiling guides</em></p>
  <img src="docs/screenshots/04-waveform-comparison-hud.png" alt="Waveforge Dual-Lane Waveform Comparison HUD" width="950" style="border-radius: 10px; box-shadow: 0 8px 28px rgba(0,0,0,0.5); margin-bottom: 28px;" />

  <h3>🔑 Air-Gapped Ed25519 Workstation Activation</h3>
  <p><em>Simple offline activation via 16-character software key or cryptographically signed offline token with non-invasive hardware ID</em></p>
  <img src="docs/screenshots/05-offline-license-activation.png" alt="Waveforge Offline License Activation Modal" width="950" style="border-radius: 10px; box-shadow: 0 8px 28px rgba(0,0,0,0.5); margin-bottom: 28px;" />

  <h3>✅ Independent Disk Verification &amp; Audit Reports</h3>
  <p><em>Physical post-write remeasurement verifying decodable frames, preserved metadata, LosslessMp3FrameGain, and true-peak ceilings</em></p>
  <img src="docs/screenshots/06-batch-verification-results.png" alt="Waveforge Audit Results and Disk Verification Receipts" width="950" style="border-radius: 10px; box-shadow: 0 8px 28px rgba(0,0,0,0.5); margin-bottom: 28px;" />

  <h3>🎧 Multi-Deck Mixer Preview (v1.3.1)</h3>
  <p><em>Audition up to 8 tracks side by side with Raw vs. Norm switching, Intro/Break/Drop section jumps, and 10s rolling Peak/RMS scope</em></p>
  <img src="docs/screenshots/07-multi-deck-meter-mixer.png" alt="Waveforge v1.3.1 Multi-Deck Mixer Preview" width="950" style="border-radius: 10px; box-shadow: 0 8px 28px rgba(0,0,0,0.5); margin-bottom: 28px;" />
</div>

---

## 🛠️ Quick Start Workflow

1. Download **Waveforge `v1.3.1`** for Windows, macOS Universal, or Linux at **[waveforge-app.com/#download](https://waveforge-app.com/#download)**.
2. Drag your DJ folder or individual tracks (`WAV`, `AIFF`, `FLAC`, `MP3`, `M4A`) into the **Library** workspace.
3. Choose your target loudness (for example `-11.0 LUFS` Club Standard), `-1.0 dBTP` true-peak ceiling, and `Preserve Dynamics` or `Match Target`.
4. Select a separate output directory and click **Process** — source files remain completely untouched.
5. Check the **Audit** view and **Mixer Preview** to verify true peaks and loudness consistency before syncing your USB crate to **Rekordbox**, **Serato**, **Traktor**, or **Engine DJ**.

---

## 🎯 Ideal For

- 🎧 **Club & Festival DJs**: Balance older vinyl rips, streaming promos, and slammed modern masters to consistent acoustic impact before booth soundcheck.
- 🎚️ **Electronic Producers & Mastering Studios**: Catch `+0.5` to `+3.0 dBTP` intersample DAC reconstruction overshoots before sending promos or club test mixes out.
- 📻 **Radio Residents & Archive Curators**: Batch-normalize mixed-format crates losslessly (including native MP3 `FrameGain`) with full JSON/CSV audit trails.

---

## 📚 Technical Research & Official Links

- 🌐 **Official Website & Downloads**: [https://waveforge-app.com](https://waveforge-app.com)
- 📖 **Visual Step-by-Step Guide**: [https://waveforge-app.com/#guide](https://waveforge-app.com/#guide)
- 🧮 **Free DJ Headroom Calculator**: [https://waveforge-app.com/tools/dj-headroom-calculator.html](https://waveforge-app.com/tools/dj-headroom-calculator.html)
- 🗂️ **Free Rekordbox XML Crate Checker**: [https://waveforge-app.com/tools/rekordbox-crate-checker.html](https://waveforge-app.com/tools/rekordbox-crate-checker.html)
- 🔬 **Engineering Articles**:
  - [Why Digital Masters Clip on Pioneer CDJ Mixers (Intersample Peaks)](https://waveforge-app.com/articles/why-club-masters-clip-pioneer-cdj.html)
  - [ITU-R BS.1770-4 Loudness Normalization for DJs](https://waveforge-app.com/articles/itu-bs1770-true-peak-oversampling-guide.html)
  - [Lossless MP3 FrameGain vs. Lossy Re-Encoding](https://waveforge-app.com/articles/lossless-mp3-framegain-vs-reencoding.html)
- ⚖️ **Comparisons**:
  - [Waveforge vs. Platinum Notes](https://waveforge-app.com/compare/waveforge-vs-platinum-notes.html)
  - [Waveforge vs. MP3Gain](https://waveforge-app.com/compare/waveforge-vs-mp3gain.html)
- 📄 **Pricing, Technical Docs & Compliance**: [Pricing Spec](https://waveforge-app.com/pricing.md) · [Technical Index (`llms-full.txt`)](https://waveforge-app.com/llms-full.txt) · [Open-Source Notices & SBOM](https://waveforge-app.com/opensource/)

---

<div align="center">
  <sub>© 2026 Waveforge · Documentation assets in this repository are MIT-licensed; the Waveforge desktop application is distributed under a commercial lifetime licence (<a href="https://waveforge-app.com/opensource/">open-source notices</a>). Pioneer DJ, Rekordbox, Serato, Traktor, and Engine DJ are trademarks of their respective owners.</sub>
</div>
