Beat Pulse — Engine & Game Overview

I've made a single file zero dependency 4 lane HTML5 Canvas rhythm game in vanilla JS. No assets, assets loading, CORS issues or anything else required except a browser tab.

How to Play

1. Open index.html in browser of choice

2. Click CLICK TO PLAY to activate browsers AudioContext

3. Use your keyboard to hit the notes as they pass the receptor at the bottom of the screen

Lane 1: D (Pink)

Lane 2: F (Cyan)

Lane 3: J (Teal)

Lane 4: K (Yellow)

4. Get a sweet combo, accuracy and complete the chart!


Technical Info & Audio Sync Quality
When making a rhythm game the most important part is the timing and audio sync. This game uses the following technologies to achieve great quality:

 1. Hardware Audio Clock Sync
Issue: When using requestAnimationFrame your frame rate can drop causing your calculations using Date.now() or performance.now() to become desynchronized with the audio output buffer of the sound card.
Fix: All note positions and logic are calculated using audioContext.currentTime which is locked to the hardware sample rate. The current beat value is calculated as
`currentBeat = ((audioContext.currentTime - startTime - offset) BPM) / 60`
This keeps notes at perfect positions even when the frame rate drops below 15fps.

2. Beat Grid Based Charting
Songs are written in beats (beat 12.5) not pixels or seconds
When rendering note positions this formula is used:
`y = TARGET_Y - ((note.beat - currentBeat) SCROLL_SPEED)`
By basing everything on the beat value instead of pixels or seconds the chart will scroll at the same speed on any display refresh rate (60hz, 120hz, 144hz+).

3. Hit Window & Accuracy

Accuracy is measured in milliseconds from when the note's target beat has passed:
PERFECT: Within 40ms of the target beat (300 points + combo multiplier)
GOOD: Within 100ms of the target beat (100 points)
BAD/MISS: Outside 100ms window or didn't hit the note at all (Combo reset)

4. Input Latency Compensation
Issues with bluetooth headsets or screen refresh rates can cause input lag. On the start screen there is an Audio Offset slider that can be adjusted between -200 and +200 milliseconds to fine tune the input timing.

5. Procedural Music Synthesis
The music for the demo is procedurally generated using the Web Audio API:
Kick - Sine oscillator that ramps from 150Hz to 0.01Hz
Snare- Filtered white noise buffer with fast exponential decay
Synth Bass - Synthwave pattern played on a sawtooth oscillator
