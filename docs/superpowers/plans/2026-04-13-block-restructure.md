# v11.3 Block Restructure Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Restructure Day A/B block ordering — move CARs to daily habits, relocate Dead Hang after floor work, move Couch Stretch to Day B, merge Spinal into Core, default to Lateral Step-Up.

**Architecture:** Single-file app (`index.html`). Changes span 3 layers: (1) BLOCKS JS data object driving the Today tab, (2) static HTML summary cards in the Schedule reference tab, (3) static HTML exercise reference sections. All in `index.html`.

**Tech Stack:** Vanilla HTML/CSS/JS, single file

**Version:** v11.2 → v11.3 (medium change — structural reorganization)

---

### Task 1: Update version to v11.3

**Files:**
- Modify: `index.html:9` (title tag)
- Modify: `index.html:1118` (ms-title)
- Modify: `index.html:1221` (cover-h1)

- [ ] **Step 1: Update all 3 version locations**

Line 9:
```html
<title>kneehab v11.3</title>
```

Line 1118 (search `ms-title`):
```html
<div class="ms-title">kneehab v11.3</div>
```

Line 1221 (search `cover-h1`):
```html
<div class="cover-h1">kneehab<br><em>v11.3</em></div>
```

- [ ] **Step 2: Commit**

```bash
git add index.html
git commit -m "bump version to v11.3"
```

---

### Task 2: Move Shoulder CARs from Mobility to Daily Habits

**Files:**
- Modify: `index.html` — `sA_mob` detail array (~line 2697), `sB_mob` detail array (~line 2756), Daily Habits `steps` array (~line 2873-2882), reference hub `src-mob` section (~line 1928), Daily Habits note (~line 2872), Rule 10 (~line 2402)

- [ ] **Step 1: Remove Shoulder CARs from Day A Mobility (`sA_mob`)**

Delete this line from the `sA_mob` detail array (line ~2697):
```js
               {n:'Shoulder CARs',rx:'1 × 5 each arm',d:'Full controlled articular rotation — maintains shoulder capsule health.'}
```

Also remove the trailing comma from the Pigeon Pose entry above it (it becomes the last item in the array).

- [ ] **Step 2: Remove Shoulder CARs from Day B Mobility (`sB_mob`)**

Delete this line from the `sB_mob` detail array (line ~2756):
```js
               {n:'Shoulder CARs',rx:'1 × 5 each arm',d:'Full controlled articular rotation — upper body warm-up prep.'}
```

Remove trailing comma from Pigeon Pose entry above.

- [ ] **Step 3: Add Shoulder CARs to Daily Habits steps array**

After the `sD_qlr` entry (line ~2882), add a new step:
```js
      {id:'sD_car',  time:'Any', title:'Shoulder CARs',                sub:'1 × 5 each arm',
       detail:[{n:'Shoulder CARs',rx:'1 × 5 each arm',d:'Full controlled articular rotation — maintains shoulder capsule health. Slow, deliberate, full ROM. Equipment-free — do anywhere.'}]},
```

- [ ] **Step 4: Update Daily Habits note**

Line ~2872, change:
```js
    note:'Informal habits — do opportunistically throughout the day. Tibialis Raise and Dead Hang are in the Primer block. Backward Walk is part of the evening ruck. These remaining habits are desk/floor friendly and benefit from frequency.',
```
to:
```js
    note:'Informal habits — do opportunistically throughout the day. Tibialis Raise is in the Primer block; Dead Hang is in the transition block. Backward Walk is part of the evening ruck. These remaining habits are desk/floor friendly and benefit from frequency.',
```

- [ ] **Step 5: Update Rule 10**

Line ~2402, change:
```html
<div class="rule-row"><div class="rule-n">10</div><div class="rule-c"><div class="rule-t">Informal daily habits: short foot, SL balance, wall slide, lateral quad roll</div><div class="rule-d">No recovery demand. Do opportunistically. Tibialis raise and dead hang in Primer block. Backward walk part of evening ruck.</div></div></div>
```
to:
```html
<div class="rule-row"><div class="rule-n">10</div><div class="rule-c"><div class="rule-t">Informal daily habits: short foot, SL balance, wall slide, lateral quad roll, shoulder CARs</div><div class="rule-d">No recovery demand. Do opportunistically. Tibialis raise in Primer block. Dead hang in transition block. Backward walk part of evening ruck.</div></div></div>
```

- [ ] **Step 6: Remove Shoulder CARs from reference hub Mobility section (`src-mob`)**

Delete this element from `src-mob` (line ~1928):
```html
<div class="ex-card"><div class="ex-top"><span class="ex-name">Shoulder CARs</span><span class="ex-rx">1 × 5 each arm</span></div><div class="ex-desc">Full controlled articular rotation of the shoulder joint through full range. Slow, deliberate, full ROM. Maintains shoulder capsule health.</div></div>
```

- [ ] **Step 7: Update reference hub Daily Habits section**

In `src-daily` (around line 2068), add after the existing entries (after Wall Slide or Lateral Quad Roll card):
```html
<div class="ex-card"><div class="ex-top"><span class="ex-name">Shoulder CARs</span><span class="ex-rx">1 × 5 each arm</span></div><div class="ex-desc">Full controlled articular rotation — maintains shoulder capsule health. Slow, deliberate, full ROM. Equipment-free — do anywhere, anytime.</div></div>
```

- [ ] **Step 8: Update briefing arrays**

In the Day D briefing `ok` array (line ~2894), change:
```js
          'Tibialis Raise and Dead Hang moved to Primer block in Day A/B — no longer needed here.'],
```
to:
```js
          'Tibialis Raise is in the Primer block; Dead Hang is in the transition block — no longer needed here.',
          'Shoulder CARs moved here from Mobility block — do anytime, no equipment needed.'],
```

- [ ] **Step 9: Commit**

```bash
git add index.html
git commit -m "move Shoulder CARs from Mobility to Daily Habits"
```

---

### Task 3: Move Dead Hang from Primer to after floor work

**Files:**
- Modify: `index.html` — `sA_prime` (~line 2687-2689), `sB_prime` (~line 2747-2749), insert new blocks after `sA_glut` (~line 2707) and after `sB_core` (~line 2763), summary cards (~lines 1614, 1632), briefing arrays

- [ ] **Step 1: Remove Dead Hang from Day A Primer (`sA_prime`)**

Change `sA_prime` (lines 2687-2689) from:
```js
      {id:'sA_prime', time:'T+5',   title:'Primer',                     sub:'~3 min',
       detail:[{n:'Tibialis Raise',rx:'1 × 20 · 1s hold top',d:'Anterior tibialis strengthening — ATG anterior chain driver. Addresses dorsiflexion deficit and reduces knee valgus tendency under load.'},
               {n:'Dead Hang',rx:'60s total',d:'Spinal decompression, grip strength, and scapular stability. Skip if shoulder discomfort.',timer:60}]},
```
to:
```js
      {id:'sA_prime', time:'T+5',   title:'Primer',                     sub:'~1 min',
       detail:[{n:'Tibialis Raise',rx:'1 × 20 · 1s hold top',d:'Anterior tibialis strengthening — ATG anterior chain driver. Addresses dorsiflexion deficit and reduces knee valgus tendency under load.'}]},
```

- [ ] **Step 2: Insert Dead Hang block after Day A Glute**

After `sA_glut` (after line ~2707), insert:
```js
      {id:'sA_hang', time:'T+21',   title:'Dead Hang',                   sub:'60s · bar',
       detail:[{n:'Dead Hang',rx:'60s total',d:'Spinal decompression, grip strength, and scapular stability. Placed after floor work — all prior blocks are floor-based. Skip if shoulder discomfort.',timer:60}]},
```

- [ ] **Step 3: Update Day A Knee Block time**

The knee block (`sA_knee`, line ~2708) time shifts. Change:
```js
      {id:'sA_knee', time:'T+24',   title:'Knee & Lower Body',
```
to:
```js
      {id:'sA_knee', time:'T+22',   title:'Knee & Lower Body',
```

- [ ] **Step 4: Remove Dead Hang from Day B Primer (`sB_prime`)**

Change `sB_prime` (lines 2747-2749) from:
```js
      {id:'sB_prime', time:'T+5',   title:'Primer',                     sub:'~3 min',
       detail:[{n:'Tibialis Raise',rx:'1 × 20 · 1s hold top',d:'Anterior tibialis strengthening. Daily frequency is the mechanism.'},
               {n:'Dead Hang',rx:'60s total',d:'Spinal decompression, grip strength, scapular stability. Skip if shoulder discomfort.',timer:60}]},
```
to:
```js
      {id:'sB_prime', time:'T+5',   title:'Primer',                     sub:'~1 min',
       detail:[{n:'Tibialis Raise',rx:'1 × 20 · 1s hold top',d:'Anterior tibialis strengthening. Daily frequency is the mechanism.'}]},
```

- [ ] **Step 5: Insert Dead Hang block after Day B Core**

After `sB_core` (after line ~2763), insert:
```js
      {id:'sB_hang', time:'T+20',   title:'Dead Hang',                   sub:'60s · bar',
       detail:[{n:'Dead Hang',rx:'60s total',d:'Spinal decompression, grip strength, scapular stability. Placed after floor work — transition to bar before upper body. Skip if shoulder discomfort.',timer:60}]},
```

- [ ] **Step 6: Update Day A summary card (Schedule tab)**

Line ~1614, change:
```html
<div class="card-row"><div class="cr-left"><div class="cr-time">T+5</div></div><div class="cr-right"><div class="cr-val">Primer <span style="color:var(--muted);font-size:12px;">3 min</span></div><div class="cr-note">Tibialis Raise · Dead Hang</div></div></div>
```
to:
```html
<div class="card-row"><div class="cr-left"><div class="cr-time">T+5</div></div><div class="cr-right"><div class="cr-val">Primer <span style="color:var(--muted);font-size:12px;">1 min</span></div><div class="cr-note">Tibialis Raise</div></div></div>
```

Also add a Dead Hang row after the Glute row (after line ~1618). Insert before the Knee block row:
```html
<div class="card-row"><div class="cr-left"><div class="cr-time">T+21</div></div><div class="cr-right"><div class="cr-val">Dead Hang <span style="color:var(--muted);font-size:12px;">60s</span></div><div class="cr-note">Spinal decompression · bar</div></div></div>
```

Update the Knee block row time from T+24 to T+22.

- [ ] **Step 7: Update Day B summary card (Schedule tab)**

Line ~1632, same Primer change:
```html
<div class="card-row"><div class="cr-left"><div class="cr-time">T+5</div></div><div class="cr-right"><div class="cr-val">Primer <span style="color:var(--muted);font-size:12px;">1 min</span></div><div class="cr-note">Tibialis Raise</div></div></div>
```

Add Dead Hang row after Core row (after line ~1635), before Knee Maintenance:
```html
<div class="card-row"><div class="cr-left"><div class="cr-time">T+20</div></div><div class="cr-right"><div class="cr-val">Dead Hang <span style="color:var(--muted);font-size:12px;">60s</span></div><div class="cr-note">Spinal decompression · transition to bar</div></div></div>
```

- [ ] **Step 8: Update Day B briefing `sub` array**

Line ~2788, change:
```js
      sub:['Short on time: mobility + dead hang + upper body is the minimum.',
```
to:
```js
      sub:['Short on time: mobility + upper body is the minimum.',
```

- [ ] **Step 9: Update Rule 15**

Line ~2408, change:
```html
<div class="rule-row"><div class="rule-n">15</div><div class="rule-c"><div class="rule-t">Low energy: do Primer + Mobility only, or Day R</div><div class="rule-d">~10 min minimum. High fatigue and poor sleep impair synthesis — doing less is correct. Day R (Recovery & Mobility) is the dedicated recovery option.</div></div></div>
```
to:
```html
<div class="rule-row"><div class="rule-n">15</div><div class="rule-c"><div class="rule-t">Low energy: do Primer + Mobility + Core only, or Day R</div><div class="rule-d">~15 min minimum. All floor-based. High fatigue and poor sleep impair synthesis — doing less is correct. Day R (Recovery & Mobility) is the dedicated recovery option.</div></div></div>
```

- [ ] **Step 10: Commit**

```bash
git add index.html
git commit -m "move Dead Hang from Primer to after floor work on both days"
```

---

### Task 4: Move Couch Stretch from Day A to Day B Mobility

**Files:**
- Modify: `index.html` — `sA_mob` detail array, `sB_mob` detail array, summary cards, reference hub `src-mob` description

- [ ] **Step 1: Remove Couch Stretch from Day A Mobility (`sA_mob`)**

Delete this entry from `sA_mob` detail array (line ~2695):
```js
               {n:'Couch Stretch',rx:'1 × 30s each side',d:'Targets rectus femoris — the hip flexor that crosses the knee joint and directly increases patellofemoral pressure when tight. Day A only — prep for split squat and curtsy loading.',timer:30},
```

- [ ] **Step 2: Add Couch Stretch to Day B Mobility (`sB_mob`)**

Insert after Kneeling Ankle Drive and before Pigeon Pose in `sB_mob` detail array (after line ~2754):
```js
               {n:'Couch Stretch',rx:'1 × 30s each side',d:'Targets rectus femoris — the hip flexor that crosses the knee joint. Day B placement: active recovery stretch for rec fem after Day A loading. Stretching during the remodeling window promotes better collagen fiber alignment.',timer:30},
```

- [ ] **Step 3: Update Day A summary card Mobility note**

Line ~1615, change:
```html
<div class="cr-note">90/90 · Cat-Cow flow · Lizard · Kneeling Ankle · Couch Stretch · Pigeon · CARs</div>
```
to:
```html
<div class="cr-note">90/90 · Cat-Cow flow · Lizard · Kneeling Ankle · Pigeon</div>
```

Update the Mobility time span from `7 min` to `5 min`.

- [ ] **Step 4: Update Day B summary card Mobility note**

Line ~1633, change:
```html
<div class="cr-note">Same as Day A minus Couch Stretch</div>
```
to:
```html
<div class="cr-note">90/90 · Cat-Cow flow · Lizard · Kneeling Ankle · Couch Stretch · Pigeon</div>
```

Update the Mobility time span from `7 min` to `6 min`.

- [ ] **Step 5: Update `sA_mob` sub timing**

Change `sA_mob` sub from `'~7 min'` to `'~5 min'`.

- [ ] **Step 6: Update `sB_mob` sub timing**

Change `sB_mob` sub from `'~7 min'` to `'~6 min'`.

- [ ] **Step 7: Update reference hub Couch Stretch description in `src-mob`**

Line ~1926, change:
```html
<div class="ex-card"><div class="ex-top"><span class="ex-name">Couch Stretch</span><span class="ex-rx">1 × 30s each side</span></div><div class="ex-desc">Shin against wall behind, other foot forward. Upright torso, squeeze rear glute. Targets rectus femoris — the hip flexor that crosses the knee joint directly. Day A (Lower) only — prep for split squat and curtsy loading.</div></div>
```
to:
```html
<div class="ex-card"><div class="ex-top"><span class="ex-name">Couch Stretch</span><span class="ex-rx">1 × 30s each side</span></div><div class="ex-desc">Shin against wall behind, other foot forward. Upright torso, squeeze rear glute. Targets rectus femoris — the hip flexor that crosses the knee joint directly. Day B only — active recovery for rec fem after Day A loading.</div></div>
```

- [ ] **Step 8: Commit**

```bash
git add index.html
git commit -m "move Couch Stretch from Day A to Day B Mobility (active recovery)"
```

---

### Task 5: Merge Spinal block into Core

**Files:**
- Modify: `index.html` — `sA_spine` and `sA_core` blocks, `sB_spine` and `sB_core` blocks, summary cards, reference hub Spinal & Shoulder section, `src-postA` id

- [ ] **Step 1: Merge Day A Spinal into Core**

Delete the entire `sA_spine` block (lines ~2698-2699):
```js
      {id:'sA_spine', time:'T+15',  title:'Spinal',                     sub:'~2 min', navPage:'base',
       detail:[{n:'Bird Dog — pick variation',rx:'90s total',d:'Anti-rotation posterior chain activation. <strong>Variations (pick one per session):</strong> (1) Slow Return — 3–5s controlled return to start, doubles eccentric anti-rotation demand; (2) Elbow-to-Knee — extend, round to touch elbow to knee, extend again — dynamic spinal control through range; (3) Bird Dog Row — hold leg extended, light DB in opposite hand, row from floor to hip — anti-rotation under dynamic upper body load.',timer:90}]},
```

Update `sA_core` — prepend Bird Dog as first exercise, update time and sub:
```js
      {id:'sA_core', time:'T+12',   title:'Core',                       sub:'~6 min', navPage:'base',
       detail:[{n:'Bird Dog — pick variation',rx:'90s total',d:'Anti-rotation posterior chain activation. <strong>Variations (pick one per session):</strong> (1) Slow Return — 3–5s controlled return to start, doubles eccentric anti-rotation demand; (2) Elbow-to-Knee — extend, round to touch elbow to knee, extend again — dynamic spinal control through range; (3) Bird Dog Row — hold leg extended, light DB in opposite hand, row from floor to hip — anti-rotation under dynamic upper body load.',timer:90},
               {n:'Hollow Body Hold',rx:'60s total',d:'Anterior core tension under spinal neutral. Trains the bracing position that supports loaded single-leg work.',timer:60},
```

- [ ] **Step 2: Merge Day B Spinal & Shoulder into Core**

Delete the entire `sB_spine` block (lines ~2757-2759):
```js
      {id:'sB_spine', time:'T+15',  title:'Spinal &amp; Shoulder',      sub:'~3 min', navPage:'base',
       detail:[{n:'Bird Dog — pick variation',rx:'90s total',d:'Anti-rotation posterior chain activation. <strong>Variations:</strong> (1) Slow Return; (2) Elbow-to-Knee; (3) Bird Dog Row.',timer:90},
               {n:'WLYT',rx:'60s total',d:'Lower and mid trapezius activation. Corrects scapular depression deficit — pairs with upper body pulling.',timer:60}]},
```

Update `sB_core` — prepend Bird Dog and WLYT as first exercises, update time and sub:
```js
      {id:'sB_core', time:'T+13',   title:'Core',                       sub:'~7 min', navPage:'base',
       detail:[{n:'Bird Dog — pick variation',rx:'90s total',d:'Anti-rotation posterior chain activation. <strong>Variations:</strong> (1) Slow Return; (2) Elbow-to-Knee; (3) Bird Dog Row.',timer:90},
               {n:'WLYT',rx:'60s total',d:'Lower and mid trapezius activation. Corrects scapular depression deficit — pairs with upper body pulling.',timer:60},
               {n:'Hollow Body Hold OR RKC Plank',rx:'60s total',d:'Alternate each session. Hollow Body: anterior core under spinal neutral. RKC Plank: full-body co-contraction, significantly harder than standard plank. Both 60s total.',timer:60},
```

- [ ] **Step 3: Update Day A summary card — remove Spinal row, update Core**

Line ~1616, delete:
```html
<div class="card-row"><div class="cr-left"><div class="cr-time">T+15</div></div><div class="cr-right"><div class="cr-val">Spinal <span style="color:var(--muted);font-size:12px;">2 min</span></div><div class="cr-note">Bird Dog (pick variation)</div></div></div>
```

Update Core row (line ~1617):
```html
<div class="card-row"><div class="cr-left"><div class="cr-time">T+12</div></div><div class="cr-right"><div class="cr-val">Core <span style="color:var(--muted);font-size:12px;">~6 min</span></div><div class="cr-note">Bird Dog (pick variation) · Hollow Body · Side Plank · Hip Flexor (pick one)</div></div></div>
```

- [ ] **Step 4: Update Day B summary card — remove Spinal row, update Core**

Line ~1634, delete:
```html
<div class="card-row"><div class="cr-left"><div class="cr-time">T+15</div></div><div class="cr-right"><div class="cr-val">Spinal &amp; Shoulder <span style="color:var(--muted);font-size:12px;">3 min</span></div><div class="cr-note">Bird Dog (pick variation) · WLYT</div></div></div>
```

Update Core row (line ~1635):
```html
<div class="card-row"><div class="cr-left"><div class="cr-time">T+13</div></div><div class="cr-right"><div class="cr-val">Core <span style="color:var(--muted);font-size:12px;">~7 min</span></div><div class="cr-note">Bird Dog (pick variation) · WLYT · Hollow/RKC · Side Plank · Hip Flexor</div></div></div>
```

- [ ] **Step 5: Update reference hub — merge Spinal & Shoulder section into Core**

Replace the "Spinal & Shoulder" sub-head and section (lines ~1932-1947) with updated content that folds Bird Dog variations and WLYT into the Core reference section. Change the sub-head:
```html
<div class="sub-head">Spinal &amp; Shoulder</div>
<p class="note" style="margin-bottom:8px;">Zero knee compression. Day A: Bird Dog only (~2 min). Day B: Bird Dog + WLYT (~3 min). Bird Dog has 3 variations — pick one per session.</p>
```
to a note within the Core section. Move the `src-postA` exercise cards (Bird Dog variations + WLYT) into the Core exercise group, before the existing Core exercises (Hollow Body, etc.).

Update the Core group header:
```html
<span class="eg-title">Core</span>
<span class="eg-freq">Day A ~6 min · Day B ~7 min</span>
```

The `src-postA` id should remain on the div containing Bird Dog + WLYT cards (needed by `initExHub`).

- [ ] **Step 6: Update Glute time on Day A**

`sA_glut` time changes from `T+21` to `T+18` to reflect earlier Core finish.

- [ ] **Step 7: Commit**

```bash
git add index.html
git commit -m "merge Spinal block into Core on both days"
```

---

### Task 6: Default to Lateral Step-Up, remove alternation rule

**Files:**
- Modify: `index.html` — `sA_knee` detail array (~line 2712), Rule 09 (~line 2401), Day A briefing `ok` array (~line 2732), summary card knee note

- [ ] **Step 1: Update Curtsy/Lateral entry in `sA_knee`**

Line ~2712, change:
```js
               {n:'Curtsy Squat OR Lateral Step-Up',rx:'Alternate each session',d:'<strong>Curtsy Squat:</strong> 3 × 8 weak / 2 × 8 strong · 2s pause at bottom. Rear foot crosses behind stance foot. Stance-leg glute med resists femoral adduction. Apply asymmetry rule. <strong>Lateral Step-Up:</strong> 2 × 10 each · 15cm box · 3s eccentric. Frontal plane quad + glute. Stand beside box, step up laterally, control descent. <strong>Levers:</strong> 15cm→20cm→25cm / eccentric tempo / bodyweight→loaded. Alternate between the two each session — both are frontal plane but different vectors.'},
```
to:
```js
               {n:'Lateral Step-Up',rx:'2 × 10 each · 15cm box · 3s eccentric',d:'Frontal plane quad + glute med. Stand beside box, step up laterally, control descent. Targets glute med — primary upstream PFPS driver. <strong>Levers:</strong> 15cm→20cm→25cm / eccentric tempo / bodyweight→loaded. <strong>Swap option:</strong> Curtsy Squat (3 × 8, 2s pause) — adductor/VMO emphasis in crossover pattern.'},
```

- [ ] **Step 2: Delete Rule 09**

Line ~2401, delete:
```html
<div class="rule-row"><div class="rule-n">09</div><div class="rule-c"><div class="rule-t">Curtsy Squat and Lateral Step-Up alternate each session</div><div class="rule-d">Both are frontal plane but different vectors. Do one per Day A, not both. Alternate each session.</div></div></div>
```

- [ ] **Step 3: Update Day A briefing `ok` array**

Line ~2732, change:
```js
          'Curtsy and Lateral Step-Up alternate each session — do one, not both.'],
```
to:
```js
          'Lateral Step-Up is the default — swap in Curtsy when you want adductor emphasis.'],
```

- [ ] **Step 4: Update Day A summary card Knee note**

Line ~1619, change `Curtsy OR Lat Step-Up` to `Lat Step-Up (or Curtsy)` in the `cr-note`.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "default to Lateral Step-Up, Curtsy as swap option"
```

---

### Task 7: Add v11.3 changelog entry

**Files:**
- Modify: `index.html` — changelog section (before line ~1266)

- [ ] **Step 1: Add changelog entry**

Insert a new changelog block before the v11.1 entry (before line ~1266):

```html
    <div class="changes-title">Changes v11.3 — block restructure</div>
    <ul class="changes-list">
      <li><strong>Shoulder CARs → Daily Habits.</strong> Removed from Mobility block on both days. Equipment-free — do anywhere, anytime. Added to Daily Habits alongside Short Foot, SL Balance, Wall Slide.</li>
      <li><strong>Dead Hang relocated.</strong> Moved from Primer (T+5) to after floor work on both days. All blocks from Mobility through Core/Glute are floor-based — Dead Hang now bridges the transition to bar/standing work. Primer is Tib Raise only.</li>
      <li><strong>Couch Stretch → Day B.</strong> Moved from Day A Mobility to Day B Mobility. Active recovery for rectus femoris after Day A loading — stretching during the remodeling window promotes better collagen alignment. Lizard + Kneeling Ankle still prep hip flexors on Day A.</li>
      <li><strong>Spinal block merged into Core.</strong> Bird Dog (and WLYT on Day B) prepended to Core block. Eliminates standalone single-exercise block header. Bird Dog is functionally anti-rotation core work.</li>
      <li><strong>Lateral Step-Up is now the default</strong> in the Knee Block. Curtsy Squat available as optional swap — no longer mandatory alternation.</li>
    </ul>
```

- [ ] **Step 2: Commit**

```bash
git add index.html
git commit -m "add v11.3 changelog entry"
```

---

### Task 8: Update timings across all blocks

**Files:**
- Modify: `index.html` — various `time:` fields in BLOCKS, summary card time values

After all structural changes are in place, do a final pass to ensure all `time:` fields are consistent with the new flow:

- [ ] **Step 1: Verify and fix Day A timings**

Expected Day A flow:
- `sA_col`: T=0
- `sA_prime`: T+5 (Tib Raise, ~1 min)
- `sA_mob`: T+6 (5 items, ~5 min)
- `sA_core`: T+11 (Bird Dog + Hollow + Side Plank + Hip Flexor, ~6 min)
- `sA_glut`: T+17 (~3 min)
- `sA_hang`: T+20 (Dead Hang, ~1 min)
- `sA_knee`: T+21 (~17 min)
- `sA_bike`: T+38

Update each block's `time:` field and the summary card rows to match.

- [ ] **Step 2: Verify and fix Day B timings**

Expected Day B flow:
- `sB_col`: T=0
- `sB_prime`: T+5 (Tib Raise, ~1 min)
- `sB_mob`: T+6 (6 items, ~6 min)
- `sB_core`: T+12 (Bird Dog + WLYT + Hollow/RKC + Side Plank + Hip Flexor, ~7 min)
- `sB_hang`: T+19 (Dead Hang, ~1 min)
- `sB_knee`: T+20 (~5 min)
- `sB_ub`: T+25 (~20 min)

Update each block's `time:` field and the summary card rows to match.

- [ ] **Step 3: Update summary card times to match**

Ensure all `cr-time` divs in both Day A and Day B summary cards reflect the updated timings from steps 1-2.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "reconcile all block timings after restructure"
```

---

### Task 9: Update memory

- [ ] **Step 1: Update project memory**

Update `MEMORY.md` and relevant memory files to reflect v11.3 structure:
- Day A/B flow changes
- Spinal block no longer exists
- CARs in Daily Habits
- Dead Hang placement
- Couch Stretch on Day B
- Lateral Step-Up default

- [ ] **Step 2: Commit memory updates**

```bash
git add -A .claude/
git commit -m "update project memory for v11.3"
```
