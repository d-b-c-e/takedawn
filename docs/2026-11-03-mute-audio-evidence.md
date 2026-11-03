# Burnout 3 mute and audio evidence

Qualified runtime source is retained on `napo/burnout3-provider-free-20261103` at `E:/Source/_work/napo-burnout3-provider-free-20261103`.
Source links below refer to that preserved checkout. This local-main landing
contains documentation only; native/harness integration is a separate task.

Offline notes follow-up506338, based on clean game documentation HEAD
e83fb1f107ef92468ec62d374b48bb0f27f75d6a. This adds no source/binary behavior,
install or new runtime qualification. Public origins remain local-commits-only.

**Retained GO504015 selected SDL's dummy audio driver successfully. Native mute
is a separate sample-processing choice. The waveOut fallback still exists.**
The accepted backend evidence does not establish a permanent common-consumer
output fence, absence of audio APIs, or physical silence.

## What each control establishes

| Control or observation | Meaning and limit |
|---|---|
| Child `SDL_AUDIODRIVER=dummy` | Requested backend before initialization. An environment value alone does not establish which backend opened successfully. |
| Actual stderr:12, `audio initialized: driver dummy` | `SDL_GetCurrentAudioDriver()` reported dummy after a successful SDL output open in this exact run. This supports the selected dummy output path. |
| `RECOMP_AUDIO_MUTE=1` / SDK `g_audio_muted` | Mix periods are zeroed. Initialization still occurs; this does not prevent a host output device from opening. |
| `noHost=true`, controllers0, no-host input latch | Host pad/keyboard/touch acquisition and motor paths are fenced by the separate input contract. These fields are not an audio-backend witness. |
| WINMM/waveOut/COM/dynamic imports | Retained capabilities needing consumer-specific resolution. Their presence is not proof that those audio backends ran; their absence alone would not clear dynamic paths. |
| Silent dummy output path | Application/backend evidence for the qualified tuple. No microphone, speaker, headset, system mixer or physical-silence measurement was made. |

The [game admission code](E:/Source/_work/napo-burnout3-provider-free-20261103/src/game/provider_free.c) requires the child dummy
environment and latches no-host input before runtime initialization. It sets
native mute as part of the private route. The [runner](E:/Source/_work/napo-burnout3-provider-free-20261103/tools/provider-free/Run.ps1)
clears inherited game/SDL diagnostic variables and supplies the child environment.
The earlier environment-snapshot failure and subsequent repairs remain in the
[qualification history](2026-11-03-provider-free-qualification.md). Do not infer
these flags or their timing for the older installed0.1.1 EXE.

## Exact consumer path and retained fallback

Qualified SDK commit **b72d5553d8b675caac8358ef80db5300b75dc83d** is retained at
`E:/Source/_work/napo-burnout3-sdk-20261103`; game binary source is
**8c08d15789f46b1fe1b698feb53e34b9413889b0**. These are the diagnostic source
identities, distinct from held bases and later harness/documentation commits.

In SDK `src/apu/apu_core.c::mcpx_apu_monitor_init`, `xa2_init()` succeeds before
the consumer returns at the selected-backend branch. For this Windows/Vulkan
build, that interface is implemented by `src/apu/apu_sdl2.c`, not the separate
host `apu_xaudio2.c`. The interface's historical name does not identify XAudio2
as the actual selected backend.

`apu_sdl2.c::xa2_init` initializes SDL audio, opens output with
`SDL_OpenAudioDevice(NULL,0,...)`, sets initialized state, logs the actual driver
and returns1. In the retained GO, dummy success therefore takes the common
consumer's early return before direct waveOut initialization.

If SDL subsystem/device initialization instead returns0, the same common
consumer logs backend-unavailable and calls `waveOutOpen(WAVE_MAPPER,...)`.
Native mute does not remove this branch. The executable's complete import text
contains WINMM plus **waveOutClose, waveOutOpen, waveOutPrepareHeader,
waveOutReset, waveOutUnprepareHeader and waveOutWrite**. Its pinned SDL2 DLL also
retains waveOut/waveIn, COM, DirectSound/WASAPI strings and dynamic resolution.
The SDL `0` capture argument selects an output stream; it does not qualify all
unrelated COM, video/window, dynamic-loading or input behavior.

The accepted observation is specific to the successful dummy branch. Do not
extend it to a failed initialization, changed build/SDK/SDL, missing driver log,
changed environment, another backend, or a future recovery path without fresh
consumer evidence. A future route requiring unconditional host-output exclusion
would need a qualified fence at the common monitor/fallback consumer while
retaining required guest audio/DSP/pacing. No such source change is delivered
by this notes task. The Burnout1 fallback incident in the hub is a useful failure
case; its corrected consumer and binary are separate from this B3 tuple.

## Retained evidence, with scope

All paths below are under
`E:/Source/_work/napo-burnout3-provider-free-20261103/` and must be retained.
Retail-derived binaries, disc and runtime captures remain private/ignored.

| Evidence | Identity / use |
|---|---|
| `bin/burnout3.exe` | Qualified CPU08 EXE SHA256 `5BC60EB186630148123FEED03C3F0B9FABBBB4C9E757CAB456D1F9AA8E3A1E10`. |
| `bin/SDL2.dll` | Qualified dependency SHA256 `53E8FA7E9ED43C30BD650C39396F0D9D2067D2FFF0B926C9433B75202592E4AA`. |
| `_local/provider-free/cpu08/live-04/stderr.txt:12` | Actual successful dummy-driver log; full file SHA256 `8C8CE86E8781AF6E09D38603C89AB1DE6B6E2D0C9021B8C509B0DAC729D64F20`. |
| `_local/provider-free/cpu08/pe-imports.stdout.txt` | Complete EXE imports, including the six waveOut symbols. |
| `_local/provider-free/audio-audit-504433/SDL2-imports.txt` and `SDL2-audio-strings.txt` | Complete dependency import table and bounded backend string inventory. Strings are signals, not executed-call receipts. |
| `_local/provider-free/audio-audit-504433/inventory.json` | Initial46 unresolved audio signals before resolutions; not the final qualified result. |
| `_local/provider-free/audio-audit-504433/resolution.json` and `audit.json` in that folder | Final audio-only companion:46hits,0unresolved, gatePass true,10 grouped resolution notes with pinned source/build/log support. |

The retained audit uses catalog SHA256
`9A81C581ACD4EE70BCF97273A458372EAA7F83055BAE80DA78BA050C05C13AA2`.
The hub's `tools/desktop-routes/examples/burnout3-audio-resolution.json` is a
worked example; the private resolution and actual retained support are the
evidence for this tuple. No validator or binary analysis was rerun here.

Companion file hashes read during this notes pass:

- `audit.json`: `A0160A016ED8DE50C94126E4AACF0A2DB50D7B98F5A2172247883460CF267104`.
- `inventory.json`: `7BD11C9B99141389AF1AAA0D2DDFEAC7B809CC2C042FAE86C22BAF101E21F9B2`.
- `resolution.json`: `E5725A31560B9B1E787FAAE1694C64C68D5FF2F305E0BCFC7A165E48775431A6`.

These JSON files resolve **audio only** for that EXE/SDL2/source/run support.
They do not requalify input, network, focus, arbitrary transitive modules or a
different catalog. Original runtime qualification, failed attempts and accepted
scene/control/capture/restore results remain preserved, without expansion.

## Next-use boundary

For any separately authorized future run, freeze the new exact EXE/SDK/SDL2 and
child environment, follow the common success/fallback path, and require an actual
selected-driver observation. Missing success or a fallback/open message cannot
be converted into a dummy-output pass from mute, noHost or environment values.
Stop the affected workload and retain its evidence/cleanup result; do not retry
an unchanged grant. Ordinary owner audio settings and global volume/environment
are not edited to manufacture a silent test.

This notes increment requests no live/CPU slot and performs no audio/device
initialization. Wheel/FFB/motion, ordinary controls, triples, installation and
owner acceptance remain separate. Existing DSP/effects documentation is in
[audio-gp.md](audio-gp.md); it does not replace backend admission evidence.
