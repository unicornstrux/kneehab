# Diet Tab Design Spec

## Summary
Add a Diet reference card as a new day-type button (🥩) in the Today tab selector row. Static reference content — no interactivity beyond standard card rendering. Version bump to v11.5.

## Navigation
- New button in the day-type selector: `🥩` with block key `'N'` (Nutrition)
- Renders in the Today tab area via `renderBlock('N')` like all other day types
- Added to `BLOCKS` object in JS

## Content Sections

### Protein
- **140–180g per day**
- Distribute across meals, ~35-45g per feeding
- 2+ hr gap before collagen (competes for absorption)

### Daily Stack
- **Collagen 15g + Vitamin C 200mg + Creatine 5g** — single combined dose
- Day A/B: 45-60 min before training
- Activity days: 45-60 min before heading out
- Rest days: before bed (targets nocturnal GH synthesis window)
- Always 2+ hrs after any protein meal

### Timing
- Stop eating 2-3 hrs before bed — late eating blunts GH pulse (Baar's second synthesis window) and degrades sleep quality
- On rest days, collagen before bed is the exception — small dose, not a meal

### Cold Exposure
- Avoid within 4-6 hrs of Day A/B — suppresses mTOR signaling that drives muscle adaptation (Roberts 2015)
- OK on Recovery days, Activity days, or morning before training
- Endurance/Zone 2 unaffected — only blunts strength/hypertrophy pathway
- Dose-dependent: colder + longer = more blunting. Even short dips (2-5 min) not proven safe post-training

### Heat (Sauna / Hot Bath)
- No blunting effect on muscle adaptation
- Increases blood flow and may aid recovery
- Post-training or evening — no timing restriction

## Implementation Notes
- Block key: `'N'` (Nutrition) — avoids collision with removed `'E'` key
- Button icon: 🥩
- Card structure: use `steps` array with one step per section (Protein, Daily Stack, Timing, Cold Exposure, Heat)
- Each step contains `detail` array with individual items as card rows
- `pills` array: daily, reference, static
- `note`: brief intro line
- `briefing`: include ok/sub/caution arrays for consistency
- No timers needed — this is reference content
- Schedule card (s-rec style HTML in the schedule pane area) is NOT needed — this renders only in Today tab via BLOCKS

## Evidence Base
- Protein target: Peter Attia (1.6-2.2 g/kg, distributing 35-50g per feeding for MPS)
- Collagen + Vit C: Keith Baar / Shaw et al 2017 (AJCN), 45-60 min pre-load window
- Creatine: extensive meta-analytic support, no absorption interaction with collagen
- Daily collagen: Baar (interviews), nocturnal GH pulse is training-independent
- Cold exposure: Roberts et al 2015 (J Physiology), Petersen & Fyfe 2021 systematic review
- Heat: no contraindication evidence for adaptation
- Late eating / GH: Baar's second synthesis window framework

## Version
- Bump to v11.5 (new feature = medium change)
- Update all 3 version locations (title, ms-title, cover-h1)
- Add changelog entry
