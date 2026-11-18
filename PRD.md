# **FairFlow PRD**

Nov 17, 2026 · @Deeya

## **Summary**

FairFlow is a phone-first watering advisor that tells a golf course superintendent, valve by valve each morning, whether to water, hold or check, and shows exactly why. It needs no new hardware: it combines open weather and soil data and the course's own map with what the course already knows (rain gauge readings, watering history, hand checks). The superintendent stays in charge, can override any call, and FairFlow learns from those overrides.

FairFlow should feel like a trusted colleague, not a dashboard. Each morning it opens with a short briefing. Walking the course, the superintendent says or types what they see in one go, confirms once, and FairFlow replies with what changed. They can ask it anything about the calls ("Why is Green 3 on water?") and get a short answer built from the stored numbers. Recommendations are made and stored per valve, so every past call can be explained exactly.

The MVP serves three roles on one course: the superintendent who makes the call, the crew who carry it out, and the general manager who answers for water use. Crew can talk or type what they see on the course, so problems like a broken head or a dry spot reach the superintendent without a trip back to the shop.

This PRD replaces the Oct 28 PRD/FRD. It keeps that document's water balance approach and trust principles, adds what we learned from the first user test (Steve LeDuc, U-M Golf Course), and is scoped so the whole product can be built in Lovable with Supabase.

## **Problem and evidence**

Superintendents make a daily watering call that decides turf condition and water cost, and today they make it mostly by hand and by eye. The tools they have either need new hardware or give numbers they do not trust enough to act on.

**What superintendents told us**

| Who | What they said | What it means for FairFlow |
| :---- | :---- | :---- |
| Steve LeDuc, superintendent, U-M Golf Course | Checks soil moisture sensors on every green each morning and adjusts the schedule himself; Toro Lynx gives no predictive recommendations | The decision is manual and time-consuming; a clear daily call has value |
| Steve LeDuc | Weather and satellite forecasts are often wrong | Local inputs (his own rain gauge, hand checks) must outrank gridded data |
| Steve LeDuc | Wants to spend less time looking at watering stats | The daily check has to take about a minute |
| Steve LeDuc | Hard to find broken lines and heads, and to know where to send crew | Crew notes from the field are worth capturing |
| Thomas Robbins, superintendent, Ann Arbor Golf & Outing | Uses city water and a Hunter Hydrawise system; enters soil data and moisture by hand; sometimes stops watering in late July because of cost, leaving fairways burnt | Water cost is a real constraint; the tool must work alongside any controller and protect greens first when water is short |
| Thomas Robbins | Golfers prioritize well-managed greens, and business drops when greens dry out | Greens conditioning is the outcome that matters most |

**What published research says**

* Golfers rank course conditions, especially greens, as the top driver of satisfaction ([Golf Digest, 2013](https://www.golfdigest.com/story/golfer-satisfaction-survey); data is old).  
* In a USGA Green Section survey of 117 respondents, superintendents ranked lack of skilled labor first now, water supply and cost second now and first for the future, and extreme weather third ([USGA Green Section Record](https://www.usga.org/content/usga/home-page/course-care/green-section-record/63/issue-02/what-are-the-biggest-challenges-facing-the-golf-course-maintenan.html)).  
* In 2024, 83% of superintendents hand-water, 87% still rely on visual assessment, and 21% use ET-based scheduling ([GCEP 2024 water survey](https://www.gcmonline.com/research/news/from-acre-feet-to-outcomes-interpreting-the-2024-gcep-water-survey), 1,766 superintendents; page only partly read, confirm before quoting).

**What we have not proven yet.** That distrust of existing tools is widespread rests on two interviews and one user test. Whether the superintendent or the general manager is the buyer is also open.

## **Users and roles**

The superintendent is the primary user and owns every watering decision; crew and the general manager get narrower views of the same data.

| Role | Who | Main device and moment | Needs from FairFlow |
| :---- | :---- | :---- | :---- |
| Superintendent (owner) | Head or assistant superintendent who sets irrigation each morning | Phone on the course at dawn; tablet in the cart; desktop for setup | Today's call per valve in about a minute, the reason behind it, and the power to override |
| Crew | Irrigation tech, hand-waterers, maintenance staff | Phone in the field, often gloved or with dirty hands | Which zones to hand-water or check today, and a fast way to report problems by voice |
| Manager | General manager or green committee | Desktop or tablet, weekly | Water use, what was decided and why, without editing anything |

**Permissions**

| Action | Superintendent | Crew | Manager |
| :---- | :---- | :---- | :---- |
| See the briefing, calls and reasons | Yes | Yes | Yes |
| Ask the advisor about the calls | Yes | Yes | Yes |
| Follow or override a call | Yes | Suggest only, unless the superintendent allows it | No |
| Log rain, irrigation or a hand check | Yes | Yes | No |
| Log turf health or report an issue | Yes | Yes | No |
| Turn a note into a task or resolve it | Yes | Own notes only | No |
| Approve or reject learned settings | Yes | No | No |
| Edit course setup, zones, valves and criteria | Yes | No | No |
| Run the engine check | Yes | No | No |
| Invite or remove team members | Yes | No | No |
| See and export water-use summary | Yes | No | Yes |
| Receive alerts | Yes | Assigned tasks only | No |
| Read and post in Community | Yes | Yes | No |

A course has one or more superintendents. Learned criteria belong to the course, and changes to them are made only by a superintendent.

## **Goals, non-goals and success metrics**

The MVP succeeds if superintendents open FairFlow most mornings and act on its calls without re-checking every zone by hand.

**Goals**

* A superintendent can name the zones to water within one minute of opening the app.  
* Every call shows its reason, the source and age of each input, and a confidence level.  
* The app works with no new hardware and alongside any irrigation controller.  
* Anyone on the course can log an observation by talking or typing, and confirm it, in under 20 seconds.

**Non-goals for the MVP**

* Controlling or scheduling the irrigation system directly. FairFlow recommends; people decide.  
* Live connections to controllers or soil probe vendors (probe readings can be uploaded as a file).  
* Disease, fertility or pesticide advice.  
* Native app store apps. The MVP is a responsive web app that can be added to the home screen; store apps come after the pilot.

**Success metrics** (starting targets to confirm with pilot courses)

| Metric | Target |
| :---- | :---- |
| Setup time to first recommendation | Under 10 minutes |
| Time to name the zones to water (user test) | Under 1 minute |
| Morning briefing opened before 7 am | 5 of 7 days during the pilot |
| Valve-days followed or overridden by less than 25% | Majority |
| Advisor answers marked helpful | Over 80% |
| Observations logged by talk or type per week, and share confirmed without edits | Set after first pilot weeks |
| Learned-setting suggestions approved | More than half |
| Zones with a turf health entry per week | Set after first pilot weeks |
| Managers who open the water-use summary monthly | At least 1 per pilot course |
| Pilot courses that say they would pay or renew | At least 2 |

## **MVP scope and features**

The MVP has 21 features, scoped so Lovable can build all of them with Supabase. Core features deliver a trustworthy daily call per valve; supporting features build trust or save effort and ship if time allows.

| ID | Feature | Priority | Requirements |
| :---- | :---- | :---- | :---- |
| F1 | Setup from course details and a map | Core | One screen of course details: name (the address fills in and sets the weather location) and number of holes; water source, price per 1,000 gal and controller brand are optional. Then upload any map (irrigation map, course layout or scorecard, as a photo or PDF). Claude reads it and fills in holes, zones, turf types and, where printed, valve numbers; it estimates areas from the map's scale and sprinkler output from turf type, labeled as estimates. No map: a standard layout from the hole count. The superintendent fixes only what's flagged. Setup moves in either direction without losing anything and can be finished later. First recommendation in under 10 minutes. |
| F2 | Weather data | Core | A Supabase function fetches daily rain history, reference ET and the forecast from Open-Meteo for the course location (NWS forecast optional as a cross-check). Each value stores its source and time; a failed fetch uses the last good value, marked stale, and lowers confidence. |
| F3 | Inputs without forms | Core | Rain, irrigation and hand checks come from talk or type (F12); forms are the backup. Spoken values outrank weather data. Followed calls count as run once confirmed: the next morning's briefing asks "Did last night's runs go as planned?" with a one-tap "Yes, all ran", or spoken exceptions. Optional irrigation history CSV with row-level errors. |
| F4 | Water balance engine | Core | One Supabase function computes soil water per valve per day: yesterday's value minus ET times turf coefficient, plus effective rain and irrigation, capped by soil capacity; compares against the threshold to decide water, hold or check; converts inches to run minutes per valve. Deterministic and versioned. Also flags: too wet (Hold, too wet), forecast rain that never came (the next run catches up and the briefing says so), and a valve that stays dry despite followed runs (Check, likely a head or line problem). |
| F5 | Zones list | Core | Zones sorted by urgency, each showing its most urgent valve's status, run time range in minutes and one-line reason; swipe to Follow all of a zone's valves. Shows when data was last updated. |
| F6 | Zone detail and the math | Core | Each valve's call, run time and confidence; the day's water balance as plain numbers; every input with source and age; track record; what would raise confidence. |
| F7 | Follow and override | Core | Follow per zone (swipe) or per valve. Override a valve or a whole zone by tap or by voice, with an optional reason; hand checks adjust the estimate. All adjustments stay visible. |
| F8 | Track record | Supporting | "Agreed with you on X of Y days" per zone, from the stored records. The 60-day replay comes later. |
| F9 | Optional data uploads | Supporting | Each course chooses how much data to add. Open data alone works; irrigation history, weather station or rain gauge exports, and soil probe readings are optional CSV uploads, at setup or later in Settings. Each upload shows which valves it improves, raises their confidence, and can be removed anytime. |
| F10 | My criteria: learning from decisions | Core | Set threshold, depth per cycle, rain credit and no-water days. After 10 Water decisions for a turf type, FairFlow proposes the average soil water at which the superintendent actually watered. Nothing changes without approval; undo and reset always available. |
| F11 | Team roles | Core | Invite crew and managers by email or link; permissions as in Users and roles. |
| F12 | Talk or type observations | Core | Home is a mic and a text box. Speech is transcribed (or the phone keyboard's dictation is used); Claude turns it into items tied to a zone or valve. A "Here's what I heard" card shows every item for one-tap confirm; at most one follow-up, as a tap choice. Nothing is saved before Confirm. FairFlow replies with what changed, in English or Spanish. Spoken values that disagree with the data are used and the gap is mentioned. Entries can be edited anytime with history kept. Replies can be read aloud (off by default). |
| F13 | Turf health log and outcome learning | Core | Turf condition per zone in under 10 seconds, by voice or tap. When at least 3 stress entries for a zone follow days below a soil water level, FairFlow proposes raising that zone's threshold, showing the entries, framed as a pattern, not proof. |
| F14 | Water-use log and manager summary | Core | Gallons per valve from run minutes and flow; summary by week and season with estimated cost, followed vs overridden and why; CSV and PDF export for managers. |
| F15 | Alerts | Core | In-app only for the MVP: a banner on Home each morning when a valve needs water or a check, and one mid-morning banner (default 10 am) if nothing has been logged. Email and push come later. |
| F16 | Recommendation records | Core | One stored record per valve per day with the call, run time, confidence, reason, the water balance, a snapshot of every input with source and age, settings used, engine version, decision and outcome. Never overwritten: recalculations create a new version. |
| F17 | Mac: morning briefing and questions | Core | Claude writes a 2 to 4 sentence morning briefing from the day's records, and refreshes it when something changes during the day (a confirmed entry, an override, a new crew issue). Anyone can ask questions about the calls in the composer; answers come only from stored records, name their numbers and sources, and never change a call. Delivered by Mac, an AI persona of a retired superintendent with an avatar and an About card. Anyone can ask where any assumption came from. |
| F18 | Engine check page | Core | An admin screen runs three hand-calculated examples (sand green, loam fairway, clay rough) through the engine and shows pass or fail. Run after any change to the math. |
| F19 | Community board | Core | Superintendents and crews post questions, reply, mark replies Helpful, filter by topic (Irrigation, Turf, Equipment, Weather, Crew) and search past answers. Posts show the author's role; showing the course name is optional, and no course data is shared unless the author attaches it. One-tap Translate between English and Spanish. Members can report a post; the team moderates. An in-app banner shows replies to your posts. Mac never treats Community posts as a data source. |
| F20 | Demo mode | Core | A fictional 18-hole course (Pine Hollow) with 30 days of history, open to the team through a test login and to anyone through a shareable link with no sign-in, also reached from "Try the demo" on sign-in. Controls: view as superintendent, crew or manager; preset scenarios (dry spell, heavy rain, broken head, tournament); "Next morning" to move a day ahead; Reset. Mac and every screen work on the demo data. An always-visible "Demo" banner; nothing touches real courses, and Community is read-only in the demo. |
| F21 | Greens first when water is short | Core | A "Water is short" switch (also by voice: "water's tight this week") and a turf priority order, greens, tees, fairways, rough by default. While on, the engine keeps higher-priority turf at full run times and trims lower-priority turf first; trimmed valves are tagged, and Mac's briefing says what got less. The superintendent can override any trimmed valve. |

**Later phases:** email and push notifications, full offline mode, the 60-day back-test replay, automatic soil (USDA) and slope (USGS elevation) lookups, storm alerts, seasonal water-use reports for regulators, a firm-and-fast tournament mode, LiDAR or satellite stress layers, controller integrations, multi-course accounts, and App Store and Google Play apps (the same code wrapped with Capacitor).

## **How a recommendation is produced**

FairFlow makes one call per valve per day. A Supabase function runs the water balance for every valve before 5:30 am local, stores a record for each, and the morning briefing is written from those records.

**Zones and valves.** A zone is an area the superintendent names (Green 3, Fairway 7). Each zone has one or more valves (controller stations), and each valve waters part of the zone with its own sprinkler output. Valves take their zone's turf, soil and slope unless set separately. Setup builds this structure from an uploaded map (F1); the superintendent only fixes what is flagged. Observations can name a zone ("Green 3 is dry" applies to all its valves) or a valve ("3B on the back left").

\\text{Soil water}\_{today} \= \\text{Soil water}\_{yesterday} \- (ET\_{ref} \\times K\_{turf}) \+ \\text{effective rain} \+ \\text{irrigation}  
The result is capped between zero and the valve's available water capacity over its root depth. A valve is **water** when soil water is below its threshold, or will fall below it today with no rain forecast. It is **hold** when soil water is above the threshold or forecast rain covers the gap. It is **check** when an input is stale or missing. A spoken value that disagrees with the weather data is used, not flagged, and the reply mentions the gap. A zone shows its most urgent valve's status.

**When water is short (F21).** With "Water is short" on, the engine keeps turf higher in the priority order (greens, tees, fairways, rough by default) at full run times and trims lower-priority valves first, down to a hold if needed. Trimmed valves are tagged, and the briefing names what got less: "Water's short this week. Greens get full runs; fairways get half."

**Extra flags.** Soil water at capacity after rain becomes "Hold, too wet", since overwatering invites disease. If forecast rain that caused a hold never came, the next run catches up and the briefing says so. A valve that stays dry for several days despite followed runs becomes Check, with "likely a head or line problem", because FairFlow can't see broken equipment.

Calls are shown only as run minutes, the number superintendents set on the controller. The engine works in inches and converts per valve using its area and flow. Until confirmed, both are estimates from the map and turf type, tagged "Estimate", and lower that valve's confidence one level:

\\text{Run minutes} \= \\frac{\\text{target inches} \\times \\text{valve area (sq ft)} \\times 60}{96.3 \\times \\text{valve flow (gpm)}}  
**Inputs, defaults and what overrides them**

| Input | Default | What the course adds | Effect on the call |
| :---- | :---- | :---- | :---- |
| Rain | Open-Meteo daily rain for the course location | Spoken or typed gauge reading; optional station or gauge CSV | Theirs wins; the reply mentions a large gap |
| Reference ET | Open-Meteo FAO-56 ET, history and forecast (free for non-commercial use; paid license if sold) | None | Drives daily water loss |
| Forecast rain | Open-Meteo forecast | None | Drives "hold, rain expected" |
| Soil | Picker per zone: sand, sandy loam, loam, clay loam, clay; greens default to sand | Picked per zone or valve | Sets water capacity and infiltration |
| Slope | Picker: flat, gentle, moderate, steep; default gentle | Picked per zone or valve | Cuts effective rain; triggers cycle-and-soak |
| Sprinkler output | Estimate by turf type, labeled, until confirmed | Per valve, in/hr or gpm | Converts inches to minutes |
| Irrigation applied | Followed calls, once confirmed as run | Spoken corrections; optional CSV history | Keeps the balance accurate |
| Turf coefficient and root depth | Default per turf type | Typed | Changes threshold and depth |
| Current soil water | Model estimate | Spoken or tapped hand checks; optional soil probe readings | Corrects the estimate |

**What each recommendation record stores (F16)**

| Field | Example | Why it is kept |
| :---- | :---- | :---- |
| Valve, zone, date | Valve 3B, Green 3, Nov 18 | One record per valve per day |
| Call and run time | Water, 8 to 12 min | What to do |
| Confidence and reason | Medium; "Soil water 41%, threshold 45%" | Shown on every screen |
| Water balance | 47% − 0.21 in ET \+ 0.10 in rain \+ 0 irrigation \= 41% | The "show the math" view |
| Inputs snapshot | Each input's value, source and age | Explains a past call exactly, even after data changes |
| Settings used | Threshold 45%, learned Nov 2 | Shows when personal criteria applied |
| Engine version | v0.1 | Ties every call to the code that made it |
| Decision | Followed by Steve at 6:12 am, or overridden to 6 min, "event tomorrow" | Track record and learning |
| Outcome | Turf entry two days later: healthy | Outcome learning |
| Change history | Every edit, who and when | Nothing is lost when entries change |

Every assumption (a default, an estimate from the map, a learned setting, or a value someone entered) stores where it came from, when, and who set it, so Mac can always explain it. Records are never overwritten. A late entry that changes a past day creates a new version of the affected records and marks the old ones superseded.

**The advisor (F17).** The advisor is Mac, an AI character who thinks and talks like a retired superintendent, with an illustrated avatar (original art, not a real person) on every reply. Tapping the avatar opens an "About Mac" card: an AI watering advisor that uses only the course's records and public weather and soil data, can say where any number came from, and gives no disease or chemical advice. Mac is always labeled as AI and never claims to be a person.

* **It speaks first.** The top of Home each morning is a 2 to 4 sentence briefing: "Morning. Green 1 and Green 3 need water tonight, 8 to 12 minutes on each valve. Fairway 7 needs a look: its weather data is a day old. Everything else holds." On days after a run, it opens with "Did last night's runs go as planned?" and a one-tap "Yes, all ran". The briefing refreshes when something changes during the day.  
* **One message in, one answer out.** Logging takes one message and one Confirm; at most one follow-up, as a tap choice on the confirm card.  
* **Ask it anything about the calls.** "Why is Green 3 on water?", "What changed since yesterday?", "How much did we put on fairways this week?" Questions need no confirm. Answers are 2 to 3 sentences built only from stored records, naming the numbers and their sources, with a link to the zone. If the records can't answer, it says so. It does not advise on disease, fertility or pesticides, and a question never changes a call. Questions about assumptions get the source, date and who set it: "Where did the 45% come from?" → "It started at 50%, the default for greens. On Nov 2 you approved 45% after your last 10 Water calls."  
* **Claude phrases; the engine decides.** Every number in a briefing or answer comes from the stored records. The language model only puts them into words.

**How talk or type works.** Claude, called from a Supabase function, turns each transcript into structured items; once confirmed, the engine recalculates the affected valves.

| You say | FairFlow understands | Saved as |
| :---- | :---- | :---- |
| "Half an inch in the gauge this morning" | Rain 0.50 in, whole course, today | Rain log |
| "Green 3 feels dry, edges are wilting" | Green 3 (all valves): drier than expected; turf slightly dry | Hand check and turf entry |
| "Ran 7B for 12 minutes last night" | Valve 7B: 12 min irrigation, yesterday | Irrigation log |
| "Head stuck on the left side of 7" | Fairway 7: broken head | Note, open issue |
| "Skip the greens tonight, tournament tomorrow" | All greens: Hold; reason: event | Override (from crew, a suggestion unless the superintendent allows crew changes) |

Zones and valves are matched by name first, then by the phone's location. Numbers are shown large on the confirm card, and out-of-range values are flagged (over 3 in of rain or 60 minutes of irrigation). Anything that can't be placed is kept as a note.

**How FairFlow learns.** Two simple, visible rules propose changes the superintendent approves. From decisions (F10): after 10 Water decisions for a turf type, it proposes the average soil water at which they actually watered ("You tended to water at 45%. Use 45%?"). From outcomes (F13): when at least 3 stress entries for a zone follow days below a soil water level, it proposes raising that zone's threshold and shows the entries.

## **Design principles**

FairFlow should feel like a trusted colleague on the grounds staff: it speaks first each morning, answers any question about its calls with the numbers behind them, and never needs more than one exchange to log something. Calm, exact and quick to explain itself. These principles settle design disagreements.

1. **The answer first.** Every screen leads with the decision (water, hold, check), then the reason, then the numbers, then the sources, each one tap deeper.  
2. **Show the why, and where it came from.** Every number names its source and age, for example "Rain 0.10 in, your entry, today" or "ET 0.21 in, Open-Meteo, 1 day old".  
3. **Honest certainty.** Show a range and a confidence level, never a single falsely precise number. Say what would raise confidence.  
4. **The superintendent is in charge.** Override is as visible as Follow. An override is never questioned, only recorded and learned from.  
5. **Built for the course, not the office.** Large targets, one-handed use, readable in direct sun and at dawn, usable with gloves. Talking or typing a sentence is always the fastest way in; forms are the backup.  
6. **Premium, not decorative.** The look borrows from a private club (deep greens, serif headings, quiet surfaces) but never at the cost of speed or legibility.  
7. **Never surprise.** Nothing changes silently: learned settings, team changes and data fallbacks are always announced.

## **Information architecture and key flows**

Home, where you talk or type, is the start screen for superintendents and crew; managers start on Zones. Details and actions are always one tap from Home, the zone list or the map.

![FairFlow information architecture · 18 screens, 3 roles, talk-first][image1]

FairFlow information architecture · 18 screens, 3 roles, talk-first

The bottom tab bar holds Home, Zones, Map, Notes and Community for superintendents and crew. Settings opens from a profile button at the top right; for crew it holds only personal options (language, display, notifications). Managers see Zones, Map and Water use, all read-only. Water use (S13) is reached from Settings for superintendents.

**Key flows**

1. **First run (under 10 minutes).** Sign in, enter the course name and holes, upload any map, fix only what FairFlow flags, and read the first briefing.  
2. **Morning briefing.** Open Home. Mac has already said what matters: "Morning. Green 1 and Green 3 need water tonight, 8 to 12 minutes on each valve. Fairway 7 needs a look. Everything else holds."  
3. **Walk and talk.** Hold the mic: "Half an inch in the gauge, Green 3 is dry on the edges." The confirm card shows Rain 0.50 in and Green 3 drier and slightly dry. Tap Confirm. Mac replies: "Got it. Green 3 stays on Water, now 10 to 14 minutes on 3A and 3B. Everything else holds."  
4. **Ask.** "Why is Fairway 7 on check?" Mac answers in two sentences from the stored record, with a link to the zone.  
5. **Decide.** Open Zones, swipe Follow on routine zones (all their valves), open the rest, Follow or Override per valve. An override can also be spoken ("Skip the greens tonight").  
6. **Report from the field.** A crew member says "Head stuck on the left of fairway 7", confirms, and it appears as an open issue in Notes, a pin on the map, and a line in the next briefing.

## **Screen requirements**

The MVP has 18 screens. Each is designed phone-first at 390 px wide, then adapted for tablet (768 px and up) and desktop. Requirements use "shall".

| \# | Screen | Roles | Feature |
| :---- | :---- | :---- | :---- |
| S1 | Sign in and join a course | All | F11 |
| S2 | Course details and map upload | Superintendent | F1 |
| S3 | Check your course | Superintendent | F1, F3 |
| S4 | Home: briefing, talk or type, ask | Superintendent | F12, F13, F17 |
| S5 | Course map view | All | F5 |
| S6 | Zone detail and the math | All (actions vary) | F6, F7, F8, F16 |
| S7 | Override sheet | Superintendent | F7 |
| S8 | Quick log forms (backup) | Superintendent, Crew | F3, F7, F13 |
| S9 | Confirm what I heard | Superintendent, Crew | F12 |
| S10 | Notes and issues | Superintendent, Crew | F12 |
| S11 | Crew home | Crew | F11, F12, F17 |
| S12 | Settings: My criteria, Team, Data, Notifications | Superintendent | F10, F11, F13, F15 |
| S13 | Water use | Superintendent, Manager | F14 |
| S14 | Zones list | All (Manager home, read-only) | F5, F7 |
| S15 | Engine check (admin) | Superintendent | F18 |
| S16 | Community board | Superintendent, Crew | F19 |
| S17 | Community thread | Superintendent, Crew | F19 |
| S18 | Demo banner and controls | Anyone in demo mode | F20 |

### **S1. Sign in and join a course**

* Shall sign in with email link or Google; no passwords to remember in the field.  
* An invite link shall open straight into the course with the right role, then show a one-screen "what you can do" note for that role.  
* A first-time superintendent with no course shall land on S2. A "Try the demo" link opens demo mode (S18) without signing in.

### **S2. Course details and map upload**

* Step 1 of 3, one screen: course name with search that fills in the address and sets the weather location; number of holes as chips (9, 18, 27); and a collapsed optional section for water source, water price per 1,000 gal and controller brand, with "Skip for now".  
* Step 2 of 3: a large "Take a photo or upload" area that accepts an irrigation map, course layout or scorecard, as an image or PDF. No drawing.  
* While Claude reads the map, progress shows what it has found ("Reading your map… found 12 holes so far").  
* "No map? Use a standard layout" builds one from the hole count: a green, tee and fairway per hole with one valve each, all marked as estimates.  
* If the map can't be read, FairFlow says so and offers the standard layout or another upload. Setup moves both ways: every step after the first has Back (and the phone's back gesture works), the step names in the progress bar (Details, Map, Check) can be tapped to jump to any step already reached, everything entered is saved as it's typed, and "Finish later" resumes at the same step.

### **S3. Check your course**

* Step 3 of 3: "Here's your course" with totals (holes, zones, valves) and everything already filled in.  
* At the top, an amber card, "3 things to check", lists only what the AI was unsure of, each as a one-tap question ("Hole 7 green: 2 or 3 valves?").  
* Below, holes as collapsible rows ("Hole 1 · 6 valves · Green, tee, fairway"). Tapping one allows edits to zone names, turf, soil and slope, and each valve's station, area and sprinkler output.  
* Areas and sprinkler output carry an "Estimate" tag until changed, with a hint on where to find output (controller program, irrigation map or nozzle chart). Confirming them raises confidence.  
* Corrections can also be spoken ("Hole 7 green has three valves") through the same confirm card.  
* An optional card offers an irrigation history CSV. "Looks right" runs the first calculation and opens the first briefing. Everything stays editable later in Settings. Going back to change the hole count or upload a different map re-reads it, keeps every fix already made, and flags only what changed.

### **S4. Home: briefing, talk or type, ask**

* Header (deep green): course name, date and "Updated 5:30 am".  
* **Morning briefing** at the top of the feed, written by Mac, with his avatar: 2 to 4 sentences naming the zones that need water (with minutes per valve), anything to check and why, and anything new (crew issues, a suggestion to review). Zone names are tappable. Under it: "Water 4 · Hold 11 · Check 2 · See all zones". After a night with runs, it opens with "Did last night's runs go as planned?" and "Yes, all ran" (one tap), or the mic for exceptions ("3B didn't run, the pump tripped"). If nobody answers, followed calls are counted as run but those valves drop one confidence level and the question stays. The briefing refreshes, with its update time, when something changes during the day.  
* **Feed:** today's entries, questions and Mac's replies, newest at the bottom like a conversation. Each confirmed entry shows a short summary and who logged it; tap to edit or undo at any time, with every change kept in the entry's history.  
* **Questions** get an answer straight in the feed with no confirm card: "Why is Green 3 on water?" → "Soil water on 3A and 3B is 41%, under your 45% threshold, after 0.21 in of ET yesterday and no rain. 3B runs 2 minutes longer for the slope." Each answer has a thumbs up or down.  
* **Prompt chips** above the composer when useful: "Rain in the gauge?", "How do the greens look?", "What changed since yesterday?".  
* **Composer,** docked above the tab bar: a 64 px mic and a text field, "Tell Mac or ask anything". The text field works with the phone keyboard's own dictation in English and Spanish, so the in-app mic is a faster path, not the only one.  
* A "Use a form instead" link opens S8.  
* Managers do not see Home; they start on S14 and can ask questions from there.

### **S5. Course map view**

* Each zone shall be marked with a pin showing its status color, icon and word, so the map reads without color.  
* Open issues shall appear as pins where they were reported.  
* Tapping a zone shall open a compact card (status, run time, reason) that expands into S6.  
* A layer toggle shall offer: status (default), confidence, and regional wetness (context only). A base map toggle switches between the course's uploaded map, with zones and valves marked where the AI found them, and a satellite view of the course location; the choice is remembered.

### **S6. Zone detail**

* Top: zone name, status chip (its most urgent valve) and confidence.  
* **Valves:** one row per valve with station number, status chip and run time range ("3A · Water · 8 to 12 min", "3B · Water · 10 to 14 min"). Swipe a valve row to Follow it alone.  
* **Show the math,** per valve: yesterday's soil water, minus ET, plus rain and irrigation, equals today, against the threshold, with the soil-water bar.  
* Track record: "Agreed with you on 34 of 45 days".  
* Inputs list: each input with value, source and age; defaults and stale values marked. Tapping any number shows where it came from (source, date, who set it), with Change and Ask Mac.  
* "Raise confidence" list: up to 3 actions.  
* History: a 14-day soil water line per valve with turf entries, overrides and notes marked; tapping a past day opens that day's stored record.  
* "Ask about this zone" opens the composer with the zone filled in.  
* Sticky bottom bar: Follow all valves (primary) and Override (secondary, equal size).

### **S7. Override sheet**

* Bottom sheet with: which valves (all in the zone by default), new status (water, hold, check), new run time (stepper in 1-minute steps), and optional reason chips ("Looks drier", "Event tomorrow", "Forecast is wrong", "Other") plus voice reason.  
* Shall save in one tap and confirm with "Logged. FairFlow will learn from this."

### **S8. Quick log sheet**

* The backup to talking. Reached from "Use a form instead" on Home and from Zone detail. Four tabs: Rain, Irrigation, Hand check, Turf.  
* Rain: inches with a number pad, whole course or chosen zones, defaulting to today.  
* Irrigation: minutes per valve or zone.  
* Hand check: drier, as expected, wetter, as three large buttons. Turf: six large condition buttons (healthy, slightly dry, wilting, dry spots, too wet, possible disease), optional photo and note, saved in one tap.

### **S9. Confirm what I heard**

* A bottom sheet titled "Here's what I heard" shall appear within 3 seconds of speaking or sending text, with the transcript collapsed above (tap to read or correct).  
* One row per item: icon, plain summary, large numbers and a zone chip, for example "Rain 0.50 in · whole course · today". Each row can be edited or removed.  
* Unclear items appear as amber rows asking for a pick ("Which green? 1 · 3 · 7") rather than a guess. Out-of-range values are flagged ("4.0 in of rain? Check the number").  
* Confirm (full width, 56 px) saves every item at once; Edit and Cancel sit beside it. Nothing is saved before Confirm.  
* For crew, an override-type item shows as "Suggest to superintendent" and is sent as a suggestion, not applied, unless the superintendent has allowed crew to change calls in Settings \> Team.  
* After Confirm, the card collapses into the feed and Mac replies in one or two lines naming what changed: "Got it. Green 3 moves to Water, 8 to 12 minutes tonight. Logged the broken head on 7."  
* No signal: the entry stays on the phone while the app is open, with "Waiting for signal" in the feed; transcription and the confirm card follow when signal returns. Audio recorded with the in-app mic is kept with the entry.

### **S10. Notes and issues**

* List of notes, newest first, filterable by zone, type, author and open or done.  
* Each note shows author, time, zone, type, transcript preview and audio play.  
* The superintendent can turn a note into a task, assign it to a crew member, and mark it done.

### **S11. Crew home**

* The same composer and feed as Home, showing the crew member's own entries, their assigned tasks, and a short list of zones to hand-water or check today.  
* Confirming saves rain, irrigation, hand checks, turf entries and issues; override-type items go to the superintendent as suggestions, unless the superintendent allows crew to change calls.  
* No Follow or Override buttons.

### **S12. Settings**

* **My criteria:** a "Water is short" switch and the turf priority order (greens, tees, fairways, rough by default); per turf type and zone, the threshold, depth per cycle, rain credit and no-water days; where each value came from (typed, learned); pending suggestions to approve or reject; undo and reset.  
* **Team:** invite by email or link, set role, remove, and choose whether crew can change calls (suggest only by default, or directly).  
* **Data:** optional uploads (irrigation history, weather station or rain gauge, soil probes), each showing which valves it improves, with remove, source status with last update, units (inches and °F by default, metric switch), display mode (light, dark, automatic), language (English or Español; the app and replies follow it), and read replies aloud (off by default).  
* **Notifications:** attention alerts on or off, alert time window, quiet hours, the mid-morning nudge (on by default, time adjustable). Alerts appear only in the app in the MVP; email and push come later.

### **S13. Water use**

* Shall lead with this season's gallons and estimated cost, and the change against the prior season when logged.  
* A weekly bar chart of gallons, filterable by turf type and zone.  
* A decisions panel: share of calls followed vs overridden, top override reasons, and learned-setting changes this season.  
* Export as CSV or PDF; managers see this screen read-only from their tab bar.

### **S14. Zones list**

* A summary line, "Water 4 · Hold 11 · Check 2", where each count filters the list.  
* Zones grouped Check, then Water, then Hold, and by urgency within each group.  
* Each row (at least 64 px): status chip with icon and word, zone name, run time range in minutes, one-line reason and a confidence marker.  
* Swipe right on a zone to Follow all its valves; tap to open S6, where valves can be followed one by one. Each row shows its valve count ("2 valves"). A map button opens S5.  
* Managers see this as their home, read-only.

### **S15. Engine check (admin)**

* Reached from Settings \> Data, superintendents only.  
* Runs three hand-calculated examples through the engine (sand green, loam fairway, clay rough), each showing expected and actual soil water and run minutes, with Pass or Fail.  
* Shows the engine version and when it last passed. A failed check shows a banner on Settings until it passes again.  
* Run after any Lovable change that touches the water balance.

### **S16. Community board**

* The Community tab, for superintendents and crew. Topic chips at the top (All, Irrigation, Turf, Equipment, Weather, Crew) and a search field.  
* Post cards show the title, the author's role and area ("Superintendent · Saline"), and replies and Helpful counts. A post in another language shows Translate.  
* A "New post" button opens a sheet: title, details (typed or spoken), topic, optional photo, and a "Show my course name" switch, off by default.

### **S17. Community thread**

* The post, then replies, oldest first. Each reply has Helpful and Report.  
* The reply composer works like Home: type or hold the mic.  
* Attaching course data (a zone's chart, say) is a deliberate choice per post; nothing is shared automatically.

### **S18. Demo banner and controls**

* An amber banner on every screen: "Demo course · nothing here is real". Tapping it opens the controls.  
* Controls sheet: View as (Superintendent, Crew, Manager); Scenario (Dry spell, Heavy rain, Broken head, Tournament); "Next morning" moves the demo a day ahead and reruns the engine and briefing; "Reset demo" restores day 1\.  
* Opened from "Try the demo" on S1, a shareable demo link, or the team's test login. Leaving the demo returns to sign-in.

## **Visual system**

The look is a private club's: deep green, ivory and brass, with serif headings. Status colors stay separate from the brand: blue means water, gray means hold, amber means check. Green never signals a status.

### **Color**

All text and status pairs shall meet WCAG AA contrast (4.5:1 for text, 3:1 for icons and large text) in both modes. Hex values are starting points to verify in the prototype.

| Token | Use | Light mode | Dark mode |
| :---- | :---- | :---- | :---- |
| brand | Header, primary buttons, logo | Deep green \#1F3D2B | Sage \#8FB89A |
| brand-on | Text on brand | Ivory \#F7F4EC | Near-black \#0F1A14 |
| accent | Highlights, active tab, selected | Brass \#9C7A3C | Brass \#D4B26A |
| surface | Page background | Ivory \#F7F4EC | Night green \#0F1A14 |
| surface-raised | Cards, sheets | White \#FFFFFF | \#18261D |
| text | Body text | \#1A1F1C | \#E9EEE9 |
| text-muted | Sources, ages, secondary | \#55605A | \#A3B0A7 |
| water | Water status | Blue \#1D5FA8 | Blue \#6AAFF2 |
| hold | Hold status | Gray \#5F6670 | Gray \#A7AEB8 |
| check | Check status | Amber \#8A5A00 (text), \#F2A900 (fill) | Amber \#F5C04A |
| danger | Errors, destructive actions | \#B3261E | \#F2B8B5 |

### **Status chips**

Status is always shown three ways at once: color, icon and word, so it reads in glare, for colorblind users, and on a grayscale screen.

| Status | Icon | Chip style |
| :---- | :---- | :---- |
| Water | Droplet | Solid blue fill, white text |
| Hold | Pause | Gray outline, gray text |
| Check | Triangle with exclamation | Amber fill, dark text |

Confidence is shown as a 3-step marker (three short bars: 1 low, 2 medium, 3 high) plus the word in the zone detail.

### **Typography**

* **Headings:** a sturdy serif with good weight at small sizes, such as Source Serif 4 (semibold). Avoid thin display serifs; they fail in sunlight.  
* **Interface and numbers:** a clear sans, such as Inter, with tabular figures so amounts line up.  
* **Scale (phone):** screen title 28 px serif; section heading 20 px serif; zone name 18 px sans semibold; body 16 px; amounts 20 px semibold; metadata (source, age) 14 px. Nothing below 14 px.  
* Units always follow the number with a space ("0.21 in", "78 °F").

### **Layout and spacing**

* 8 px spacing grid; 16 px page margins on phone, 24 px on tablet.  
* Touch targets at least 48 x 48 px; primary actions 56 px tall; the mic button 64 px on Home.  
* Primary actions sit in the bottom third of the screen for one-handed use.  
* Tablet (768 px and up): Home feed or Zones list on the left, zone detail or map on the right.  
* Cards use 12 px corner radius, a 1 px hairline border, and no heavy shadows.

### **Components**

Zone row, status chip, confidence marker, soil-water bar (with threshold tick), input row (value, source, age, default or stale tag), bottom sheet, segmented control, number stepper, primary and secondary buttons, floating mic and map buttons, banner (info, warning, stale data), toast, empty state, and skeleton loading rows, Mac's avatar and About card, the morning briefing card, valve rows, the answer card with thumbs up and down, the talk composer (mic plus text field), the conversation feed, and the confirm card with editable item rows.

### **Light and dark mode**

* Shall support light, dark and automatic (follows the phone setting). Automatic is the default.  
* Dark mode shall use deep green-black surfaces, not pure black, and lighter status tints so they stay legible at dawn.  
* Light mode is tuned for direct sun: no light-gray text on ivory, and no status information carried by tint alone.  
* The map shall switch to a darker basemap in dark mode.

### **Voice and tone**

Mac's voice: short, plain and respectful, in the first person, like a retired superintendent talking to a colleague. Warm, never jokey about a call, and always clear that he is AI. Lead with the action ("Water Green 3 tonight"), then the reason. Use "your" for the course's own inputs ("your rain entry"). Never blame the user for an error; say what happened and how to fix it. Replies after a confirm are one or two lines, name what changed, and stop: "Got it. Green 3 moves to Water, 8 to 12 minutes tonight." When someone's number disagrees with the data, Mac uses theirs and says so: "Got it, 0.50 in. The weather data showed 0.10 in, so I'm using yours."

## **Non-functional requirements**

| Area | Requirement |
| :---- | :---- |
| Performance | Home usable within 3 seconds on a phone on a weak 4G connection; the last briefing and calls show instantly on reopen; confirm card within 3 seconds of speaking |
| Reliability | Records ready before 5:30 am local via a Supabase scheduled function; if today's records are missing when someone opens the app, the engine runs on demand; a failed weather fetch never blocks a call |
| No signal | Unsent entries stay on the phone while the app is open and send when signal returns, with a visible "waiting for signal" label; full offline mode comes later |
| Platform | Responsive web app built in Lovable, installable to the home screen (iOS Safari, Android Chrome); tablet and desktop layouts |
| Accessibility | WCAG 2.1 AA; status never by color alone; supports system text size up to 200%; screen reader labels on every control |
| Security and privacy | Sign-in required; each course sees only its own data; role permissions enforced in Supabase row-level security; API keys only in Supabase secrets; course data not shared or sold |
| Voice data | Audio and transcripts belong to the course; crew are told when recording starts; entries can be deleted by their author or a superintendent |
| Units and language | Calls in run minutes; rain and ET in inches; °F and acres by default, with a metric switch. Interface, speech and replies in English and Spanish |
| Correctness | The engine is deterministic and versioned; the engine check page (S15) passes before any release; every record stores the engine version that made it |
| Advisor answers | Built only from stored records; every number quoted must exist in the records; out-of-scope questions are declined plainly |

**Build approach.** Everything is built in Lovable with Supabase: screens, sign-in, database and storage, plus Supabase edge functions for the engine, the weather fetch, transcription, Claude map reading and parsing, and the briefing and answers. Keys stay in Supabase secrets. Because there is no separate code review, the engine lives in one versioned function, the engine check page (S15) is rerun after any change to the math, and the "show the math" view lets anyone spot a wrong number.

## **Risks, open questions and validation**

| Risk | Why it matters | Mitigation |
| :---- | :---- | :---- |
| Calls are not trusted | Steve's top concern; the reason existing tools go unused | Briefing and answers that show their numbers; show the math; track record; override always available |
| Lovable changes break the math | No unit tests or code review in a Lovable-only build | One versioned engine function; engine check page; version stored on every record |
| Advisor invents a number | One wrong answer damages trust | Answers built only from stored records; numbers checked against the record; out-of-scope questions declined |
| Misheard numbers or zones | "Point five" vs "point oh five" changes a call | Numbers shown large on the confirm card; range checks; nothing saved before Confirm |
| AI cost and speed | Parsing, briefings and answers each call Claude | One call per entry or question; briefing once a day; cost per course measured in the pilot |
| AI misreads the map | Wrong zones or valve counts skew every call | Unsure items flagged for one-tap fixes; estimates labeled and lower confidence; everything editable later; standard layout if the map can't be read |
| Weather data too coarse for greens | Regional data misses local rain | Spoken gauge readings outrank it; hand checks; honest confidence |
| Scheduled job not available | The 5:30 am run depends on Supabase scheduling | Run on demand when the app opens and today's records are missing |
| Turf entries mislead learning | Weather, traffic and disease also affect turf | Minimum counts, evidence shown, framed as a pattern, approval required |
| People won't talk to an app | Some prefer tapping, or work near others | Typing and keyboard dictation work the same way; forms remain as backup |
| Premium look hurts legibility | Serif type and ivory backgrounds can wash out in sun | Sturdy serif for headings only; contrast tested outdoors |
| Open-Meteo license | Free for non-commercial use only | Fine for the class pilot; budget a paid license or switch to NWS before selling |
| Bad advice in Community | A wrong tip could hurt turf and FairFlow's trust | Posts labeled as peer advice; Helpful votes; report and moderation; Mac never uses posts as data |

**Open questions**

* What time do superintendents set the day's irrigation, and so when must records be ready? (5:30 am is a placeholder.)  
* Should crew be able to change calls directly? It is a Team setting; ask each pilot superintendent which default they want.  
* Review the engine's placeholder constants (water-holding by soil, root depth, turf coefficients, threshold and refill target) with a superintendent before the pilot.  
* Is the buyer the superintendent or the general manager?  
* Which maps do pilot courses have on hand (irrigation as-builts, layouts, scorecards), and do they show valve numbers and a scale?  
* Does our Lovable and Supabase plan support scheduled functions?  
* Is the phone's keyboard dictation good enough for the pilot, or do we need a transcription service for the in-app mic?  
* How many crew members carry phones on the job, and how many work in Spanish?

**Validation plan**

1. Run 4 to 6 more user tests with superintendents already contacted (including Ann Arbor Golf & Outing and Radrick Farms), using a task-based script per Session 5 and the demo link, so testers need no setup. Time how long it takes to name the zones to water.  
2. Retest with Steve LeDuc to see whether confidence, sources and the track record answer his accuracy concern.  
3. Test S9 and S11 with at least 2 crew members on the course, outdoors, in sun.  
4. Back-test a season of U-M Golf irrigation and weather records if the course can share them.  
5. Track the success metrics during the pilot.

[image1]: <data:image/png;base64,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>