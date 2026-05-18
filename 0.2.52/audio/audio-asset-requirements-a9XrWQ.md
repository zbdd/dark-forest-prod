# Audio Asset Handoff

This folder is the delivery handoff for the runtime cue roster defined in
`docs/design/audio-spec.md` and loaded by `client/src/game-audio.ts`.

## Supported filenames

Use either basename with any of these extensions:

- `.ogg` **preferred**
- `.mp3`
- `.wav`
- `.m4a`

## Cue roster

| Basename | Runtime status | Trigger | Length target | Loop | Feel | Mix / generation notes |
| --- | --- | --- | --- | --- | --- | --- |
| `no-anger-ambient` | Active | Starts immediately for the base run ambience while anger is below the low-anger gate. Crossfades down once anger reaches `25%` of the threshold. | `8s`–`16s` seamless loop | Yes | Calm but uneasy forest bed; this is the default exploration layer before tension rises. | Keep it sparse and low-pressure so the low-anger layer can feel like a clear escalation. Soft wind, distant creaks, low pads, and subtle wildlife are all fine. |
| `pack-spawn-sting` | Active | Fires when a new roaming pack is spawned at the anger threshold. | `0.4s`–`0.9s` target | No | Short animal snarl/roar warning with a sharp attack and quick decay. | Keep it readable over the HUD and toast layer. The shipped cue currently comes from the OpenGameArt `Gorilla Sounds` pack and should stay terse, centered, and free of long tails. |
| `low-anger-ambient` | Active | Crossfades in once anger reaches `25%` of the threshold and crossfades back out if anger drops below that gate or a new run begins. | `6s`–`12s` seamless loop | Yes | Subtle forest drone / wind bed that gently raises tension without sounding like full combat. | Keep it low-energy and non-melodic so the spawn sting still cuts through. Prefer a clean loop point with short fade-in/out baked into the clip. Stereo is fine, but keep it centered and light. |
| `ui-drop` | Active | Fires when a valid worker drop opens the radial action dial. | `0.1s`–`0.4s` | No | Light UI open / drop cue. | Keep it short and readable so rapid worker interactions do not stack into mush. |
| `action-start` | Active | Fires when a player commits a dial action. | `0.1s`–`0.4s` | No | Soft confirm / start cue. | Should feel lighter than completion cues. |
| `action-complete` | Active | Fires when fortify, explore, or expansion land successfully. | `0.2s`–`0.6s` | No | Positive completion cue. | Shared across multiple action completions, so avoid anything too specific. |
| `build-complete` | Active | Fires when a build timer resolves into a finished building. | `0.2s`–`0.7s` | No | Slightly stronger positive completion cue. | Distinct from the generic action-complete cue. |
| `fortify-absorb` | Active | Fires when fortification absorbs a pack hit. | `0.2s`–`0.7s` | No | Metallic / shield-like impact. | Should cut through combat without becoming harsh. |
| `combat-popup-open` | Active | Fires when a new confrontation popup mounts for pending combat. | `0.1s`–`0.5s` | No | Tense popup / confrontation reveal cue. | Keep it short so repeated combats do not drag. The shipped cue now uses a sharp plate impact rather than a reused UI-style placeholder. |
| `combat-card-slam` | Active | Fires when the confrontation timeline enters the card-slam reveal phase. | `0.1s`–`0.5s` | No | Firm card / panel impact. | Should support the visual slam without feeling like a second full clash. |
| `combat-token-pop` | Active | Fires for each little combat token when it appears and again when it pops away during the confrontation timeline. | `0.05s`–`0.25s` | No | Pleasant subtle UI pop. | Keep it light and pleasing because the cue can overlap many times in quick succession. It should read as tactile feedback, not as a second clash layer. |
| `combat-clash` | Active | Fires when the confrontation timeline enters the token-cancel clash phase. | `0.1s`–`0.6s` | No | Sharp shield/weapon impact cue. | This is the main combat hit for the current popup pass, so keep the attack crisp and readable. |
| `combat-result-reveal` | Active | Fires when the confrontation result panel is revealed. | `0.1s`–`0.5s` | No | Neutral result-turnover / reveal cue. | Keep it agnostic to win/loss because the actual run-end and combat outcome text handle meaning. |
| `poi-landmark-claim` | Active | Fires when a passive landmark becomes owned and its passive effect turns on. | `0.3s`–`0.9s` | No | Ceremonial landmark-claim resolve. | Should feel more significant than `action-complete` without sounding like an end-of-run victory sting. |
| `poi-reward-found` | Active | Fires when an adventure POI resolves into a permanent `bonus-wood` or `bonus-food` reward. | `0.2s`–`0.7s` | No | Bright treasure/find cue. | Keep it clearly positive and distinct from both generic completion and full victory. |
| `poi-reward-empty` | Active | Fires when an adventure POI resolves with no permanent run bonus. | `0.1s`–`0.4s` | No | Soft searched/empty confirmation. | Should acknowledge resolution without sounding punitive. |
| `survivor-hut-reveal` | Active | Fires when a survivor hut becomes recruitable and the encounter popup opens. | `0.2s`–`0.6s` | No | Gentle encounter-discovered chime. | Keep it readable over UI without sounding like a reward-claim cue. |
| `survivor-recruit` | Active | Fires when a recruit succeeds and a new worker actually joins the colony. | `0.3s`–`0.8s` | No | Hopeful recruit-success resolve. | Must not also play on failure states. |
| `run-win` | Active | Fires when the run ends in victory. | `1s`–`8s` | No | Celebratory end-state sting. | One-shot only; no loop required. |
| `run-loss` | Active | Fires when the run ends in defeat. | `1s`–`12s` | No | Somber / losing end-state cue. | One-shot only; trim or compress long source files if needed. |

## Technical targets

- Sample rate: `44.1 kHz` or `48 kHz`
- Bit depth: `16-bit` or standard compressed equivalent
- Keep the combined delivered audio for this issue well under the brief's SFX/music budgets
- Favor short files; the runtime already applies conservative volumes

## Current cue sources

- `ui-drop`, `action-start`, `action-complete`, and `fortify-absorb` currently come
  from the OpenGameArt **100 CC0 SFX** pack.
- `combat-popup-open` now uses Kenney **Impact Sounds** (`impactPlate_heavy_001.ogg`, CC0).
- `combat-card-slam` now uses Kenney **Casino Audio** (`card-shove-2.ogg`, CC0).
- `combat-token-pop` now uses Kenney **Casino Audio** (`chip-lay-1.ogg`, CC0).
- `combat-clash` now uses Kenney **Impact Sounds** (`impactMetal_heavy_001.ogg`, CC0).
- `combat-result-reveal` now uses Kenney **Casino Audio** (`card-slide-4.ogg`, CC0).
- `pack-spawn-sting` currently uses the OpenGameArt **Gorilla Sounds** pack
  (`gorilla_snarl_01`).
- `build-complete` currently comes from the OpenGameArt **Fast Hammer SFX** clip.
- `poi-landmark-claim` and `poi-reward-empty` currently come from the OpenGameArt
  **100 CC0 SFX** pack (`gong_02.ogg` and `switch_01.ogg` respectively).
- `poi-reward-found` currently comes from the OpenGameArt **80 CC0 RPG SFX**
  pack (`item_gem_04.ogg`).
- `survivor-hut-reveal` currently comes from the OpenGameArt **100 CC0 SFX**
  pack (`bell_02.ogg`).
- `survivor-recruit` currently uses the OpenGameArt **Short Jingle** clip
  (`PositiveChoice.ogg`).
- `run-win` uses the OpenGameArt **Victory Sting** clip.
- `run-loss` currently uses the OpenGameArt **Medieval Defeat** clip.

## Current runtime behavior

- `ui-drop`, `action-start`, `action-complete`, `build-complete`,
  `fortify-absorb`, `combat-popup-open`, `combat-card-slam`,
  `combat-token-pop`, `combat-clash`, `combat-result-reveal`,
  `poi-landmark-claim`, `poi-reward-found`,
  `poi-reward-empty`, `survivor-hut-reveal`, `survivor-recruit`, `run-win`,
  and `run-loss` are all **one-shot** cues.
- `combat-popup-open`, `combat-card-slam`, `combat-clash`, and
  `combat-result-reveal` fire from the confrontation popup's GSAP timeline,
  not from model-level combat state transitions.
- `combat-token-pop` is the one cue in this set intentionally configured to
  overlap with itself so every token appearance and pop-away beat can sound.
- `pack-spawn-sting` is a **one-shot** cue that fires on `SpawnPack`.
- `poi-landmark-claim`, `poi-reward-found`, and `poi-reward-empty` route
  through the existing POI `TriggerMilestone` seams.
- `survivor-hut-reveal` fires when the recruitable survivor encounter becomes
  available, and `survivor-recruit` fires only after a successful recruit
  resolution.
- `no-anger-ambient` is the base loop and starts first.
- Once anger reaches `25%` of the threshold, the runtime crossfades from `no-anger-ambient` into `low-anger-ambient`.
- `StartRun` stops every layer so runs reset cleanly.
- If these files are absent, the current logic safely stays silent until they are added.
