# BlinkLock 👁️🔒

> Blink 3 times fast to lock your screen. Wink to unlock. PIN fallback included.

## Setup

```bash
pip3 install opencv-python mediapipe numpy
python3 blinklock.py
```

## How it works

MediaPipe FaceMesh tracks eyelid landmarks. EAR (Eye Aspect Ratio) measures how open your eye is — open eye is ~0.3, closed eye drops below 0.22. When EAR drops and rises quickly, that's a blink. When it stays low longer, that's a wink.

## State machine

| State | What it means |
|-------|--------------|
| IDLE | Watching, not counting |
| COUNTING | First blink detected, waiting for 2 more within 2 seconds |
| LOCKED | 3 blinks registered, screen is locked |

Wink (hold eye closed ~0.5s) → unlock. Or type `1234` + Enter.

## Hardware concept

Same debounce logic used in firmware for physical buttons. A button press bounces (flickers) — firmware ignores noise by requiring a stable signal for minimum duration. EAR does the same: eye must be closed for at least 2 frames to count as a blink.

## Tuning

| Problem | Fix |
|---------|-----|
| Blinks not registering | Lower `EAR_THRESHOLD` to 0.20 |
| Too sensitive | Raise `EAR_THRESHOLD` to 0.25 |
| Wink not unlocking | Lower `BLINK_MAX_FRAMES` to 8 |
| Window too short | Increase `BLINK_WINDOW` to 3.0 |
