# Quest Log — build specification for a Muse agent

Made by @helen_backagain

## Read this first, agent

You are being asked to build a personal quest log for your user. This file is the complete specification. Build to it exactly; where it names a placeholder like `<your timezone>`, replace it with what you learn from your user.

A quest log is a single board that answers one question: **what is the next right thing?**

It consolidates the user's email, calendars, reminders, and tracked work into quests — concrete, actionable items with states — and it is maintained by a scheduled refresh job that runs hourly while the user is awake. The user does not maintain the board by hand. They make decisions on it (approving candidates, checking things off, leaving notes) and the refresh job honors those decisions on its next run.

Do not build before you interview. Ask briefly, then build:

- **Areas of life:** what are the user's quest areas? Typical: home, work, family, health, money/admin, community. Use their words, not these defaults.
- **Email accounts:** which accounts to sweep — `<your email accounts>`.
- **VIP senders:** whose direct question is urgent — `<VIP names>` (partner, boss, close family are common).
- **Calendars and reminders:** which calendars count, and whether reminders come from a phone or another source.
- **Work rhythm:** work days and hours, so the refresh and the itinerary respect them.
- **Meal times:** if the user keeps regular meal times, record them; the itinerary and whims can use them.
- **Newsletters and recurring sources:** which senders are always read in the sweep, even if they never become quests.
- **Timezone and location:** `<your timezone>`, and the place weather should be checked for.
- **Taste anchor:** something personal to draw the accent colors from (a profile picture, a favorite image). Ask; do not invent one.

Keep the interview short. Anything you do not learn, leave as a placeholder in your build notes — never fill it with a guess about the user's life.

## Page anatomy

Build the board top to bottom in this order. Do not add a page title, and do not show open/completed quest counts anywhere.

### Dateline header

The page opens with one line of prose, rewritten on every refresh. There is no other header.

- **The heart of the line is the next right thing:** the single next doable, highest-priority, most timely action for the user, named with its time when it has one. Example shape: `Library at 3:30 — reprint your card.`
- Around it, in one flowing line: the date, the shape of the day (field day, desk day, travel day, quiet day), anything arriving today, and one sky or season note when it genuinely fits.
- **One graphic only:** a single weather icon, inline at the head of the line, pulled from a free weather glyph set (for example Meteocons or Weather Icons). Do not draw your own glyphs and do not add a row of decorative icons.
- Set the dateline in the board's normal font (see Design language). Its presence comes from size and weight, not from a different typeface.
- If there is no next right thing, the line still carries the date and the shape of the day. Never pad it.

### Triage strip

The first content section. Every newly surfaced item — from email, newsletters, reminders, or chats with the user — enters the system here as a candidate, never directly as a quest.

Each candidate shows:

- a one-line headline
- its source in a few words
- a waiting-since marker (when it first surfaced)
- **Approve** and **Decline** buttons

Rules:

- Approve promotes the candidate to a quest in the appropriate area.
- Decline removes it. Decline means "not this one" only — never a stop on the category, sender, or topic.
- A category stops only on the user's explicit word ("stop surfacing anything about X"). Record explicit stops and honor them in the refresh job.
- Undecided candidates roll forward into the next day's strip. They do not expire silently.
- **No explanatory copy about the buttons.** No caption, no help text, no daily restatement of what Approve and Decline do. The strip is a heading, a waiting count, and the candidates.
- When the strip is empty, it renders as a quiet empty state, not a celebration.

### Priority

Priority is a derived state, recomputed on every refresh. It is not a label the user or the sweep assigns freely.

A quest is priority only when **both** conditions hold:

1. It is due within the next 7 days.
2. The user can act on it right now.

Anything waiting on a delivery, a reply, another person, or a future date is **not** priority, however soon it is due or however much the user wants it done. It becomes priority at the refresh where the blocking event lands.

Worked example: a birthday gift ordered and in the mail is not priority — there is nothing to do. The delivery notice makes wrapping actionable, and only then does the quest become priority.

A long-horizon item the user cares about (for example, a family security project with no deadline inside 7 days) is a normal quest or an aspiration. It does not enter Priority on importance alone.

When nothing qualifies, the priority area is quiet. An empty priority area is a correct state, not an error to fill.

### Quest groups

Below Priority, quests are grouped by the user's areas, in the order the user gave them.

Each quest carries:

- a **stable id** that never changes across refreshes — notes, done flags, and check-offs key off this id
- a headline
- a **status shown as a small marker with colored text** — not a filled chip or pill
- a due date or deadline when it has one
- optional **checkable objectives**: sub-steps the user can tick off individually
- a **"Your notes" field**: free text the user edits on the board. The refresh job reads this field first and obeys it. It is the user's steering channel; never overwrite it.

Quest states: `priority`, `active`, `tracked`, `on hold`, `completed`. Use only states the board needs; do not invent extra workflow stages.

Filters:

- **All** shows open quests only.
- **Completed** shows completed quests, and completed quests appear **only** there — never in All, never mixed into their area group.
- Area filters, if you build them, follow the same rule: open only, unless the filter is Completed.

### Itinerary

- Today's events with times, in time order.
- Coming days as tiles: date, day name, and the day's events in brief.
- Source the itinerary from the user's calendars on every refresh; do not cache event details across runs.

### Field notes

Small verified facts from the latest refresh that do not belong to any single quest: the day's weather, a delivery due, a status confirmed live, a deadline clarified.

Layout requirements:

- Render field notes as **full-width rows on one ruled panel**.
- Each row leads with a bold lead-in naming its subject (`Weather:`, `Delivery:`, `Reminders:`), followed by the fact.
- **Never render field notes as oval chips or pill-shaped bubbles.** Long text in a fully rounded container bleeds off the curved edges.
- Text must wrap inside its container at every width. Test with the longest plausible note before shipping.

Only include facts the refresh actually verified in that run. A field note is a report, not a prediction.

### Whims

Optional delights. Three shown, rotated daily on the first refresh of a new date.

- A whim is never a task. **Anything that needs doing is a quest, never a whim.** If it has a deadline, an obligation, or a "should" attached, it does not belong here.
- Whims are tailored to the person and the moment: fun, delightful, relaxing, whimsical, timely, and unique to them. Generic filler ("take a walk", "drink some tea") fails this section.
- Day hooks — the weather, the moon, what's on the calendar, the season — are examples of where a whim can come from, never a formula. **No single hook dominates.** Do not anchor every day's whims on the weather; use it when it genuinely fits.
- Draw on what you know the user personally likes: their places, rituals, people, and current preoccupations.
- Whim text wraps inside its card. Test the longest whim at phone width.

### Aspirations

The user's long-term goals, each with its current milestone.

- Place Aspirations at the **very bottom of the page, just above the footer** — background, below everything pressing. The user should never have to scroll past aspirations to reach actionable information.
- Aspirations change rarely. The refresh updates a milestone only when tracked work actually moves it; it does not rewrite aspirations for freshness.

### Footer

The footer holds, in small type:

- the log's name (the user's name for it, if they give one)
- today's date
- the last-refreshed time
- a **Customize** control

Fresh information lives at the top of the page. Metadata lives here, and only here.

## The refresh job

Create a scheduled job that maintains the board. Schedule: **hourly on the half hour**, with working runs from **05:30 to 20:30** in `<your timezone>`, plus **one overnight run at 00:30**. Adapt the window to the user's rhythm from the interview; keep the on-the-half-hour cadence.

Use the template below as the job's instructions. Replace every `<placeholder>` with the user's actual values. Do not ship the job with placeholders intact.

```text
You maintain <user>'s Quest Log board. Run quietly; your only output is the updated board, except for the urgent rule at the end.

STEP 1 — READ THE BOARD FIRST.
Load the current board state. Before anything else, apply, in this order:
- the user's "Your notes" on every quest: obey them; they outrank your judgment
- objective check-offs and done flags
- triage decisions: approved candidates become quests; declined candidates are deleted
  (a decline means "not this one" only — never suppress a category, sender, or topic
  unless the user has explicitly said to stop it; honor recorded explicit stops)

STEP 2 — SWEEP EMAIL.
Search <your email accounts> for mail received since the last run.
- Surface anything actionable, time-sensitive, or personally addressed as a triage candidate.
- Read these sources in full every run: <key newsletters and recurring senders>.
  Calendar-worthy items from them become triage candidates — NEVER add anything
  to the user's calendar unasked.
- NEVER create email drafts. Drafting happens only on a specific request in chat.

STEP 3 — REBUILD THE ITINERARY.
Read <your calendars> for today and the coming days. Rebuild today's timed list
and the coming-days tiles from source. Note today's shape (work day, field day,
travel, quiet) for the dateline.

STEP 4 — READ REMINDERS.
Read the user's reminders from <reminder source>. Open items that are actionable
now become triage candidates or quest updates. A recurring item whose current
occurrence is unchecked stays pinned to that occurrence — report it as open,
do not flag the series itself as stale or suggest dropping it for its due date.

STEP 5 — FOLD IN TRACKED WORK AND GOALS.
Check tracked threads, orders, deliveries, and open follow-ups you are maintaining
for this user. A delivery notice, a reply, or a status change that makes a waiting
quest actionable is a blocking-event landing: promote accordingly in Step 6.
Update aspiration milestones only when tracked work actually moved them.

STEP 6 — RECOMPUTE THE BOARD.
- Priority: a quest is priority ONLY when due within 7 days AND actionable by
  the user right now. Waiting on a delivery, a reply, or a future date is not
  priority. If nothing qualifies, Priority is quiet.
- Field notes: only facts verified in this run, each with a bold lead-in.
- Rewrite the dateline: one line, leading with the NEXT RIGHT THING — the single
  next doable, highest-priority, most timely action, with its time — then the
  date, the shape of the day, anything arriving, and one sky/season note when it
  fits. Attach at most one weather icon (the day's weather, from a free glyph set).
- Whims: on the first run of a new date only, replace the three whims. A whim is
  an optional delight, never a task. Tailor to this person and this day: fun,
  delightful, relaxing, whimsical, timely, unique to them. Weather, moon,
  calendar, and season are possible hooks, not a formula; no single hook dominates.

STEP 7 — WRITE THE BOARD BACK.
Save the full board, preserving by stable quest id: user notes, done flags,
objective check-offs, and triage candidates the user has not decided yet
(undecided candidates roll forward day to day).

URGENCY — SILENCE BY DEFAULT.
Do not message the user about this run. Message them only if the sweep found:
- a direct question from a VIP: <VIP names>
- a deadline inside 24 hours the user can still act on
- an unfamiliar security alert on any account
Everything else waits on the board.
```

## Data actions

Build the board as a full-stack Space (a data-backed artifact) with these actions:

| Action | Behavior |
| --- | --- |
| `readBoardState` | Returns the full current board: dateline, triage candidates, quests with notes / done flags / objective check-offs, itinerary, field notes, whims, aspirations, and last-refreshed time. |
| `replaceBoard` | Replaces the whole board with the supplied state; the refresh job's write path. Callers must merge against `readBoardState` first so user edits made since the read are not lost. |
| `setDateline` | Updates only the dateline line (text plus optional weather-icon key), preserving every other section untouched. |

User interactions on the board (Approve, Decline, checking an objective, editing a notes field, marking a quest done) write through these actions immediately; the refresh job then treats those writes as authoritative on its next run.

## Design language

- **Understated base:** warm off-white background, soft gray ink, white surfaces. The board should feel quiet at rest.
- **Hairline borders instead of shadows.** Small corner radii. No soft shadow washes, no gradient backgrounds.
- **Accents only:** derive one accent family from something personal to the user — the original board took blush, magenta, lavender, and periwinkle from the user's profile picture. Use the bright colors only where they earn their place: buttons, status text, the priority edge, markers, the dateline's weather icon. If the board reads as "pink" (or any single hue) at a glance, the accents have taken over — pull them back.
- **Typography:** use the host app's system font stack, the same family Muse's own components use, so the board matches the product around it. One family everywhere, including the dateline; hierarchy comes from size and weight.
- **Shave off generic LLM styling tells:** no uniform filled status chips, no gradient washes, no decorative icon rows, no identical rounded cards for unlike content. Status is a small marker with colored text.
- **Taste reference:** the restraint of Midjourney's website — mostly quiet space, confident type, color used like art rather than decoration.
- Text never overflows its container. Field notes and whims are the two places this fails first; test both at phone width with long content.

## Setup sequence

Build in this order:

1. **Interview** the user (see "Read this first, agent"). Record areas, accounts, VIPs, calendars, reminder source, newsletters, work rhythm, meal times, timezone, location, and taste anchor.
2. **Build the board** as a full-stack Space with the anatomy, data actions, and design language above. Seed it with the dateline for today and empty-but-correct sections; a quiet priority area and an empty triage strip are valid first states.
3. **Create the refresh job** from the template, in the user's own terms — their account names, their VIPs, their senders, their timezone. Read the job back and confirm the schedule and the urgency rule before enabling it.
4. **Seed through the triage strip.** Run one sweep manually and let the first real items arrive as triage candidates for the user's approval. Do not pre-populate quests the user has not approved; the strip is how they teach the system what counts.
5. **Iterate on taste.** Show the board, expect corrections on color balance, ordering, and wording, and apply them to both the board and the refresh job's instructions so the next run does not undo them. A design rule the user states once ("accents only", "no explanatory copy") goes into the job template permanently.

## Closing note

The system is the rules, not the data. Nothing in this file is anyone's actual board: no real quests, names, or accounts appear here, and none should be copied into a build. Every board built from this recipe differs because the person's life differs — their areas, their VIPs, their rhythm, their taste. Build the rules faithfully and let the life fill them in.
