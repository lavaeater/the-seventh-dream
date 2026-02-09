# The Seventh Dream

This project was generated using the [Bevy New 2D](https://github.com/TheBevyFlock/bevy_new_2d) template.
Check out the [documentation](https://github.com/TheBevyFlock/bevy_new_2d/blob/main/README.md) to get started!

## Jam Theme and Exploration

Let's brainstorm.

I am involved in a game jam for the game engine Bevy. I have about 12-24 hours of effective time to make a game for the jam. I will keep it simple and TIGHT, so I will make the game in 2D, obviously. 

The theme is "Extremely Incohesive Fever Dream" which is a fantastically open theme, which I like.

What could that mean and reveal as possibilities for a jam game?

I think I need to focus on a tight game play mechanic and loop and consider everything else to be frills.

To capture the Fever Dream aspect, I am thinking something that starts as perhaps a simple shooter but becomes more and more frantic the longer you play. To capture the "incohesive" part of it, I looked up the Merriam-Webster definition and it comes out as "lacking integration". 

That could mean any number of things in a simple 2D shooter...

Perhaps it would work best in a top-down horror-shooter, that would lend itself to being incohesive. 

The name of the game will be "The Seventh Dream" because it is the seventh Bevy Jam and, well, Dream is in the theme...

## Core ideas

Top-down 2D shooter

Escalating chaos over time

Incohesion = systems that stop integrating cleanly

Fever dream = sensory + logic breakdown

### Incohesion can mean:
* Controls vs visuals mismatch
* UI vs gameplay mismatch
* Physics vs expectations mismatch
* Systems that used to cooperate now conflict 
* Player loses ability to model the game mentally

### Fever dream can mean
* Escalation 
* Pattern recognition → pattern collapse 
* Familiar → uncanny → hostile nonsense

5️⃣ My Favorite “Jam Win” Concept

If I were you, I’d build this:

The Seventh Dream — Reality Desynchronization Shooter

Core:
Top-down survival shooter

Main mechanic:
Reality Instability Meter fills over time.

As it fills:

Systems desync from each other

Game rules contradict

Player must adapt moment-to-moment

Instability Effects Pool (random pick):

Bullets orbit instead of travel straight

Enemies move only when you shoot

Time speeds up when you stand still

UI becomes diegetic enemies

Screen wraps toroidally

Player leaves ghost trail that can damage enemies

Controls rotate 90° every 10 seconds

Score drains unless moving

You only need like 8–12 modifiers to feel deep.

6️⃣ Bevy-Friendly Implementation Strategy

Architecture idea (very jam safe):

Effect {
name
duration
apply_system
remove_system
}


Then:

ActiveEffects: Vec<EffectInstance>


And each frame:

Update durations

Activate new effect when instability threshold crossed

You don’t need ECS wizardry — just toggling resources is enough.

7️⃣ Scope Safety Plan (Important for Jam)

Must-Have (Ship This)

Movement

Shooting

Enemies

Survival timer

3 instability effects

Nice-to-Have

8+ effects

Juice

Audio distortion

If-Time

Meta progression

Narrative text

Boss

8️⃣ Title Integration Ideas

The Seventh Dream

7 major instability tiers

7 effect categories

7 minutes = “true ending”

7 lives across runs

Low effort, high theme score.

9️⃣ Quick Weird Effect Idea Dump (Jam Gold)

Use if stuck:

Mouse cursor is enemy attractor

Player shoots UI buttons

Enemies spawn from screen edges → later from player shadow

Bullets become enemies after timeout

Player sprite changes size with score

Map rotates slowly

Dead enemies form terrain

Shooting pushes player backwards

Enemies only visible while moving

Screen occasionally shows past position

# The Seventh Dream

## Bevy Jam Design + Execution Document

**Theme:** Extremely Incohesive Fever Dream
**Genre:** 2D Top-Down Shooter
**Time Budget:** 12–24 effective hours

---

# Core Vision

**Pillars:**

* Tight gameplay loop
* Escalating chaos over time
* Systems become incohesive (stop integrating cleanly)
* Fever dream = sensory + logic breakdown

**Core Loop:**
Move → Shoot → Dodge → Survive → Instability increases → Rules break → Adapt → Repeat

---

# 24-Hour Build Plan

## Hours 0–2 — Skeleton + Movement + Shooting

**Goal:** Playable immediately

Implement:

* Window + camera
* Player entity
* WASD movement
* Aim (mouse or 8-direction fallback)
* Shooting
* Bullet lifetime or collision despawn

Deliverable:

* Player square shooting smaller squares
* Can kill placeholder enemies

If not done by hour 2 → cut scope.

---

## Hours 2–4 — Enemies + Damage + Spawn Loop

Implement:

* Enemy AI: move toward player
* Health system
* Enemy spawn timer
* Score or survival timer

Simple enemy movement:

```
velocity = normalize(player_pos - enemy_pos) * speed
```

Deliverable:

* Survive waves
* Can lose

You now have a game.

---

## Hours 4–6 — Instability Framework (Critical)

Implement:

* Instability meter (0 → 100)
* Timer fills meter
* Threshold triggers random effect
* Effect duration timer
* Effect removal

Deliverable:

* Console logs showing effects activating and ending

---

## Hours 6–10 — Implement Core Effects

Goal:

* Minimum 5 effects
* Target 8–10 effects

No polish yet.

---

## Hours 10–14 — Juice Pass

Add:

* Screen shake
* Color shift
* Hit flash
* Audio pitch shift (optional)
* Camera micro drift

---

## Hours 14–18 — Art + Theme Pass

Add:

* Dream text overlays
* Title screen
* Death screen
* Background shader or pattern

---

## Hours 18–22 — Stability + Bug Fixing

Fix:

* Crashes
* Effect stacking bugs
* Spawn spikes
* Performance issues

---

## Hours 22–24 — Submission Polish

Add:

* Screenshots
* Controls screen
* Itch description
* Final build

---

# Instability Effects (Top 10)

## 1. Control Rotation

Rotate input vector over time.

```
rotated = rotation_matrix(angle) * input_vec
```

---

## 2. Bullet Curve

Add perpendicular velocity component to bullets.

---

## 3. Enemy Phase Shift

Enemies update only every N frames (jitter teleport feel).

---

## 4. Time Pulse

Oscillate global time scale.

```
time_scale = sin(time * speed) * 0.5 + 1.0
```

---

## 5. Ghost Player Trail

Spawn delayed clones following past positions.

---

## 6. Reverse Recoil Shooting

Player moves backwards when shooting.

---

## 7. UI Desync

Health or ammo UI becomes delayed or noisy.

---

## 8. Screen Wrap (Toroidal World)

World wraps at edges.

---

## 9. Bullet → Enemy Conversion

Expired bullets spawn enemies.

---

## 10. Camera Drift

Camera slowly moves off player center.

---

# Systems Design Sketch (Bevy-Friendly)

## Core Components

```
Player
Enemy
Bullet
Velocity
Health
Lifetime
```

---

## Resources

```
InstabilityMeter { value }
ActiveEffects { Vec<EffectInstance> }
GameTimeScale { value }
RngResource
```

---

## Effect Instance

```
EffectInstance {
    kind: EffectKind,
    remaining: f32
}
```

---

## EffectKind Enum

```
enum EffectKind {
    ControlRotate,
    BulletCurve,
    PhaseEnemies,
    TimePulse,
    GhostTrail,
    ReverseRecoil,
    UiDesync,
    ScreenWrap,
    BulletSpawnEnemy,
    CameraDrift,
}
```

---

## System Order

### Frame Flow

```
Input Systems
Effect Modifier Systems
Movement Systems
Combat Systems
Spawn Systems
Effect Lifetime Update
Rendering / Camera
```

---

## Effect Activation Logic

```
if instability > threshold {
    add random effect
    instability -= threshold
}
```

---

## Effect Application Pattern (Jam Safe)

```
if effects.has(ControlRotate) {
    modify input
}
```

---

# Minimal Art Direction

## Color Palette

Base:

* Black background
* White player
* Red enemies

Instability Colors:

* Purple flashes
* Cyan flashes

---

## Shapes

Player:

* Square or diamond

Enemies:

* Circle or triangle

Bullets:

* Tiny squares or dots

Ghosts:

* Transparent player sprite

---

## Cheap Fever Dream Visual Tricks

### Color Channel Offset

Draw sprite twice slightly offset.

### Scanline Overlay

Fullscreen transparent stripe pattern.

### Fake Chromatic Aberration

Draw red + blue offset ghost copies.

---

## Background Ideas (Cheap, Effective)

* Scrolling noise texture
* Rotating grid
* Parallax star/dot field

---

# High Instability "Signature Moment"

At high instability:

* Sound muffles
* Screen darkens
* Effects trigger faster
* Text overlay:

```
DREAM 7 UNSTABLE
WAKE UP
WAKE UP
WAKE UP
```

---

# If You Only Implement 5 Things

Do these first:

* Control rotation
* Camera drift
* Bullet curve
* Ghost trail
* Reverse recoil shooting

These alone create a memorable experience.

---

# Final Goal

Ship a tight, weird, escalating shooter that feels like the game itself is falling apart — but intentionally.

That is **The Seventh Dream**.
