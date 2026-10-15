# Walking App — Product Plan (draft, not yet built)

Reference app: **Weight Loss Walking by Slimkit** (iOS, Montibus Ltd / Verv Inc., App Store id 950669893; Android "Walking by Slimkit").
Note: slimkit.health and the App Store are blocked from the research environment, so the feature list below comes from App Store listing excerpts, review aggregators, Trustpilot and BBB as surfaced by web search.

## 1. What Slimkit Walking does today

| Area | Feature |
|---|---|
| Onboarding | Quiz (goal, weight, level, diet prefs) → "personalized plan" → hard paywall |
| Training plans | 3‑month interval walking plan, 5–7 workout days/week, 3 difficulty levels; indoor (treadmill) and outdoor |
| Workout | Audio coach cues ("speed up", "slow down"), interval timer, GPS route map, pace / distance / time / calories |
| Easy mode | Free walk with GPS route, no plan |
| Steps | Daily step goal screen (phone pedometer only) |
| Media | Plays your music / podcasts under the coach audio |
| History | Detailed statistics, manually edit a workout |
| Nutrition | Meal plans (upsell) |
| Platforms | Apple Watch start/stop + live metrics; writes workouts to Apple Health; Runkeeper/Fitbit/MapMyRun sync (older) |
| Pricing | ~$9.99/mo, $41.99/6 mo, $59.99 lifetime (varies by region) |

### What users complain about (the opportunities)
1. **Billing**: surprise renewals, charges labeled "Verv", hard to cancel, slow refunds. → Transparent pricing is a differentiator.
2. **Paywall before value**: quiz promises a plan, then demands payment.
3. **Inaccurate calories** (reported ~2× a treadmill's number).
4. **Steps only count while the app runs / from the phone**; Apple Watch steps and calories don't sync in.
5. **GPS inaccuracy, crashes, freezes**, battery drain in background.
6. **Meal plans** ignore preferences (e.g., vegetarian), few vegetables.

## 2. Core feature set (parity, done better)

1. **Onboarding**: short quiz (goal, current activity, age/height/weight, available days, indoor/outdoor). Show the generated plan *before* any paywall.
2. **Interval walking plans**: 12‑week plans × 3 levels (Beginner / Intermediate / Advanced), plus short 4‑week plans. Plan adapts when you miss days.
3. **Guided workout screen**: current interval (warm‑up / brisk / easy / cool‑down), big countdown ring, audio + haptic cues, music ducking, pause/resume, lock‑screen controls.
4. **Free walk**: GPS route, pace, distance, elevation; treadmill mode uses CoreMotion only.
5. **All‑day steps**: read from HealthKit (merges iPhone + Apple Watch), so steps count without the app open. Daily goal ring + streaks.
6. **Calories done right**: use Apple's Active Energy from HealthKit when available; otherwise MET × weight × time with speed/grade, shown as a range when uncertain.
7. **History & stats**: calendar, weekly/monthly charts (Swift Charts), per‑workout map + splits, edit/delete workouts.
8. **Apple Watch app**: start/stop guided interval workouts standalone, heart rate, haptic interval cues, complications.
9. **Apple Health**: read steps, distance, active energy, heart rate, weight; write workouts with route.
10. **Subscription (StoreKit 2)**: free tier is useful; clear price, trial end reminder notification, one‑tap "Manage subscription" link in Settings.

## 3. New features Slimkit doesn't have

**Smarter coaching**
- **Heart‑rate zone intervals**: intervals driven by your HR zone (Watch), not just time — "speed up until zone 3".
- **Adaptive plan**: next week's difficulty adjusts from completion rate, perceived effort (1–5 after each walk) and HR recovery.
- **Cadence coach**: target steps‑per‑minute with an optional metronome/beat‑matched music (e.g., 120 spm brisk walking).
- **Walking Japanese interval training (IWT)** and **Zone 2** plans as named programs.
- **Post‑meal walks**: 10‑minute walk reminders after logged meals (blood‑sugar benefit).

**Everyday steps**
- **Live Activity + Dynamic Island** for the current walk and for "steps left today".
- **Home/lock‑screen widgets**: step ring, streak, next workout.
- **Smart nudges**: "You're 1,200 steps from your goal — a 12‑min walk does it", timed around your calendar/inactivity.
- **Streak freezes** and rest days built into the plan so streaks don't punish recovery.

**Routes & outdoors**
- **Route planner**: "Give me a 3 km loop from here" with MapKit; save favorite routes.
- **Weather‑aware**: suggest indoor/treadmill workout when it's raining or very hot; air‑quality note.
- **Elevation / hill walks** and **rucking** (weighted walking) mode with adjusted calories.
- **Safety**: share live location with a contact, auto‑alert if stopped unexpectedly (opt‑in).

**Motivation & social**
- **Virtual journeys**: walk the Camino, Route 66, Great Wall — progress on a map with landmark postcards.
- **Badges & milestones**, personal records (longest walk, fastest km, best week).
- **Friends & challenges**: weekly step challenge with friends; family sharing on one subscription.
- **Shareable walk cards** (map + stats) for social.

**Health & weight loss**
- **Weight trend** from HealthKit or manual entry, smoothed trend line, goal projection.
- **Simple nutrition**: calorie target + optional quick logging; meal ideas that respect diet preferences (vegetarian, vegan, allergies) — no rigid meal plans.
- **Weekly summary**: steps, minutes, calories, weight trend, plan adherence; optional Claude‑written recap and tips.
- **Accessibility**: low‑impact plans (seniors, rehab, pregnancy), VoiceOver, large text, adjustable coach voice/speed.

**Platform polish**
- **Siri / App Intents / Shortcuts**: "Start my walking workout", Action Button support.
- **Focus filter / CarPlay‑free**, **AirPods** head gestures to skip interval (where available).
- **Offline‑first** with iCloud sync (SwiftData + CloudKit); no account required.
- **Privacy**: no ads, data stays on device/iCloud.

## 4. Proposed tech stack

- SwiftUI, iOS 17+ / watchOS 10+, SwiftData + CloudKit
- HealthKit, CoreMotion (CMPedometer), CoreLocation + MapKit
- HKWorkoutSession on Watch, WatchConnectivity
- ActivityKit (Live Activities), WidgetKit, App Intents
- AVFoundation (audio ducking, speech or recorded coach clips)
- StoreKit 2, WeatherKit, Swift Charts, UserNotifications

Constraint: this repo's dev container is Linux — Swift sources can be written here, but building, running and testing require Xcode on a Mac (or a macOS CI runner such as GitHub Actions `macos` runners).

## 5. Suggested release phases

- **MVP (v1)**: onboarding, 3 interval plans, guided workout with audio cues, free walk with GPS, HealthKit steps + workouts, history, widgets, StoreKit.
- **v1.1**: Apple Watch app, Live Activities, Siri shortcuts, streaks/badges.
- **v1.2**: HR‑zone & adaptive plans, cadence coach, route planner, weather.
- **v2**: virtual journeys, friends/challenges, weight & nutrition, AI weekly recap.
