# Audio Asset Requirements

Issue **#20** wires the runtime audio cues from `mvt-entry.ts`. The game now looks
for the following assets under this folder.

## Supported filenames

Use either basename with any of these extensions:

- `.ogg` **preferred**
- `.mp3`
- `.wav`
- `.m4a`

## Required assets

| Basename | Trigger | Length target | Loop | Feel | Mix / generation notes |
| --- | --- | --- | --- | --- | --- |
| `no-anger-ambient` | Starts immediately for the base run ambience while anger is below the low-anger gate. Crossfades down once anger reaches `25%` of the threshold. | `8s`–`16s` seamless loop | Yes | Calm but uneasy forest bed; this is the default exploration layer before tension rises. | Keep it sparse and low-pressure so the low-anger layer can feel like a clear escalation. Soft wind, distant creaks, low pads, and subtle wildlife are all fine. |
| `pack-spawn-sting` | Fires once whenever a new roaming pack is spawned at the anger threshold. | `0.4s`–`0.9s` | No | Ominous warning sting, sharp attack, short decay. | Keep it readable over the HUD and toast layer. Good candidates: detuned bell hit, metallic scrape, low forest shriek, or short brass stab. Avoid long reverb tails. Mono is fine. |
| `low-anger-ambient` | Crossfades in once anger reaches `25%` of the threshold and crossfades back out if anger drops below that gate or a new run begins. | `6s`–`12s` seamless loop | Yes | Subtle forest drone / wind bed that gently raises tension without sounding like full combat. | Keep it low-energy and non-melodic so the spawn sting still cuts through. Prefer a clean loop point with short fade-in/out baked into the clip. Stereo is fine, but keep it centered and light. |
| `ui-drop` | Fires when a valid worker drop opens the radial action dial. | `0.1s`–`0.4s` | No | Light UI open / drop cue. | Keep it short and readable so rapid worker interactions do not stack into mush. |
| `action-start` | Fires when a player commits a dial action. | `0.1s`–`0.4s` | No | Soft confirm / start cue. | Should feel lighter than completion cues. |
| `action-complete` | Fires when fortify, explore, or expansion land successfully. | `0.2s`–`0.6s` | No | Positive completion cue. | Shared across multiple action completions, so avoid anything too specific. |
| `build-complete` | Fires when a build timer resolves into a finished building. | `0.2s`–`0.7s` | No | Slightly stronger positive completion cue. | Distinct from the generic action-complete cue. |
| `fortify-absorb` | Fires when fortification absorbs a pack hit. | `0.2s`–`0.7s` | No | Metallic / shield-like impact. | Should cut through combat without becoming harsh. |
| `run-win` | Fires when the run ends in victory. | `1s`–`8s` | No | Celebratory end-state sting. | One-shot only; no loop required. |
| `run-loss` | Fires when the run ends in defeat. | `1s`–`12s` | No | Somber / losing end-state cue. | One-shot only; trim or compress long source files if needed. |

## Technical targets

- Sample rate: `44.1 kHz` or `48 kHz`
- Bit depth: `16-bit` or standard compressed equivalent
- Keep the combined delivered audio for this issue well under the brief's SFX/music budgets
- Favor short files; the runtime already applies conservative volumes

## Current cue sources

- `ui-drop`, `action-start`, `action-complete`, and `fortify-absorb` currently come
  from the OpenGameArt **100 CC0 SFX** pack.
- `build-complete` currently comes from the OpenGameArt **Fast Hammer SFX** clip.
- `run-win` uses the OpenGameArt **Victory Sting** clip.
- `run-loss` currently uses the OpenGameArt **Medieval Defeat** clip.

## Runtime behavior

- The pack spawn sting is a **one-shot** cue.
- `ui-drop`, `action-start`, `action-complete`, `build-complete`, `fortify-absorb`, `run-win`, and `run-loss` are all **one-shot** cues.
- `no-anger-ambient` is the base loop and starts first.
- Once anger reaches `25%` of the threshold, the runtime crossfades from `no-anger-ambient` into `low-anger-ambient`.
- `StartRun` stops every layer so runs reset cleanly.
- If these files are absent, the current logic safely stays silent until they are added.
