# Loop Radio

**[Try it live →](https://peterlodri-sec.github.io/loop-radio)**

Single-file procedural Web Audio engine. Type a genre — browser generates music live. No server, no dependencies, no install.

```
stoner desert rock → phrygian scale, 72 BPM, sawtooth oscillators
dark ambient       → slow drone, no drums, long reverb tail
daft punk / house  → 128 BPM, minor scale, four-on-the-floor kick
8bit / chiptune    → 150 BPM, major scale, square wave
lofi (default)     → 85 BPM, pentatonic, triangle wave, hi-hat groove
```

## How to run

Download `index.html` and open it in any modern browser. That's it.

```bash
# Or serve locally
python -m http.server 8080
```

## How it works

Everything is in one 360-line HTML file using only native browser APIs:

**Keyword parser** — maps your genre text to synthesis parameters: BPM, harmonic scale, waveform type, rhythm density, reverb.

**Lookahead scheduler** — runs on a 28ms interval, scheduling Web Audio nodes 120ms into the future. Standard trick for glitch-free browser audio: the audio thread always has instructions ahead of time, so main-thread jitter doesn't cause clicks.

**Three synthesis layers:**
- Kick: exponential frequency envelope on an `OscillatorNode` (160 Hz → 0 in 150ms)
- Hi-hat: white noise through a `BiquadFilterNode` (highpass at 8 kHz)
- Melody: oscillator + ADSR, waveform determined by genre

**WebGL2 background** — pulsing rings via signed-distance-field GLSL shader. Ring radius scales with a `fract(T * BPM / 60)` sawtooth so rings pulse on the beat.

**ENTHEA Tier-2 telemetry** — fires a `page_view` and `radio_start` event (with genre and BPM) to the vaked telemetry endpoint. No PII, no cookies, no fingerprinting. Disable by removing the telemetry script block.

## Genre parameters

| Genre | BPM | Scale | Waveform | Drums |
|-------|-----|-------|----------|-------|
| lofi (default) | 85 | pentatonic | triangle | yes |
| stoner / desert rock | 72 | phrygian | sawtooth | yes |
| doom / sludge | 60 | phrygian | sawtooth | yes |
| dark ambient / drone | 50 | phrygian | sine | no |
| daft punk / house | 128 | minor | sawtooth | yes |
| techno / industrial | 140 | minor | sawtooth | yes |
| jazz | 145 | dorian | triangle | yes |
| 8bit / chiptune | 150 | major | square | yes |

## Fork and host

The file is designed to be forked. Change the genre map, add synthesis layers, swap the GLSL shader, add your own telemetry endpoint. Every change stays in one file.

```bash
git clone https://github.com/peterlodri-sec/loop-radio
# edit index.html
# open index.html in browser
```

## License

AGPL-3.0. Derivative works must remain open source.

## Links

- [Live demo](https://peterlodri-sec.github.io/loop-radio)
- [Blog post — how it works](https://pocoo.vaked.dev/posts/2026-06-24-loop-radio)
- [peterlodri-sec](https://peterlodri-sec.github.io)
- [pocoo.vaked.dev](https://pocoo.vaked.dev)
- [protocol.vaked.dev](https://protocol.vaked.dev)
