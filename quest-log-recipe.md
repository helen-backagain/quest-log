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

An item's status is one of `needs_you`, `working`, `watching`, `waiting`, with done carried by a separate `done` flag (see "The small vocabularies" below for the full table). Priority is group placement, not a status. This follows the real board the recipe was distilled from, and it supersedes the five display states (`priority`, `active`, `tracked`, `on hold`, `completed`) that an earlier version of this recipe named.

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
- a **Customize** control and a **Refresh** button

Fresh information lives at the top of the page. Metadata lives here, and only here.

### Customize

The footer's Customize control opens a small dialog with exactly these choices, saved through a `savePrefs` action (`layout`, `showComingUp`, `showGlance`, `showShared`) so they persist across devices:

- **Layout:** comfortable or compact density
- **Show coming up:** the itinerary tiles, on or off
- **Show glance:** the field notes panel, on or off
- **Show shared:** the shared section, on or off

That is the whole feature. Customize is display only: it never edits quests, areas, sources, or the refresh schedule. Changes to those are made in chat with the agent, which applies them to the board and the job together. The footer also carries a Refresh button beside Customize, which re-reads the board state and re-renders, for the moment the user wants the latest refresh without reloading the page.

## The refresh job

Create a scheduled job that maintains the board. The mechanism: create ONE hourly job, anchored on the half hour, and put the working window inside the job's own instructions as a gate:

- The job fires every hour at :30, around the clock.
- Its first instruction is the gate: if the local time in `<your timezone>` is a working run (05:30 through 20:30) or the overnight run (00:30), do the full refresh; otherwise end immediately and do nothing. Adapt the window to the user's rhythm from the interview; keep the on-the-half-hour cadence.
- The 00:30 overnight run performs the full refresh except the morning timeline check, so the whims and the rest of the board are fresh by morning. The timeline check belongs to the 05:30 run only.
- One job, not several: a single gate inside one set of instructions cannot drift against itself the way three separately scheduled jobs can.

Read the job back after creating it and confirm the anchor, the gate, and the urgency rule before enabling it. Enabling is the user's call, given after that read-back.

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

### What the job reads through

The job template names placeholders (`<your email accounts>`, `<reminder source>`). Behind each placeholder sits a capability, not a specific product. Name them by class when you set the job up, and confirm each one exists before enabling the job:

- **Mail reader:** can search and read the user's mail accounts, newest first, filtered by time received. Used in Step 2.
- **Calendar reader:** can list events with times across the user's named calendars. Used in Step 3.
- **Phone reminders bridge:** can read the user's reminder lists from their phone, including each recurring series' current occurrence. Used in Step 4.
- **Weather check:** can return current conditions and the day's high and low for the user's location. Used for the dateline glyph and the weather field note.
- **Board actions:** `readBoardState`, `replaceBoard`, and `setDateline` on the board Space. Used in Steps 1, 6, and 7.
- **Chat:** one outbound message path, used only by the urgency rule.

The job is an agent job, not a dumb timer: it needs its chat access for the urgency rule and its judgment for triage. If a capability class has no tool behind it on your stack, say so at setup time and let the user choose a source that exists. Never enable the job against an account it cannot actually read; a job that sweeps nothing fails silently every hour and teaches the user to distrust the board.

### The failure rule

Sources fail: phones go offline, accounts disconnect, services time out. The rule is one paragraph and it is not optional:

> When a source is unreachable in a run, keep the last good values from that source exactly as they were, and stamp the affected section with when it was last successfully checked, in the form "Last checked <time>" inside that section's field note or item details. Never guess, never fill a gap with a plausible value, and never present a carried-forward value as fresh. A section stamped "Last checked" is an honest board; a silently stale section is a lying one.

Corollaries:

- A failed source never blocks the rest of the run. Sweep what answers, write the board, stamp what did not.
- The dateline is written from verified facts only. If the weather check failed, the dateline carries no weather glyph and no sky note that run.
- Triage candidates are created only from sources that actually answered. An unreachable mail account produces no candidates and no apology item; the "Last checked" stamp is the whole report.

## Data actions

Build the board as a full-stack Space (a data-backed artifact) with these actions:

| Action | Behavior |
| --- | --- |
| `readBoardState` | Returns the full current board: dateline, triage candidates, quests with notes / done flags / objective check-offs, itinerary, field notes, whims, aspirations, and last-refreshed time. |
| `replaceBoard` | Replaces the whole board with the supplied state; the refresh job's write path. Callers must merge against `readBoardState` first so user edits made since the read are not lost. |
| `setDateline` | Updates only the dateline line (text plus optional weather-icon key), preserving every other section untouched. |

### The board state

`readBoardState` returns the state below, and `replaceBoard` accepts the same state back. Build exactly this shape. Field names are the contract: the board, the refresh job, and every interaction action below read and write these names and no others.

```text
BoardState
  profile        { name: string, firstName: string }
  updatedAt      string            display stamp of the last refresh, e.g. "Thu, Nov 26, 10:30 AM EST"
  eyebrow        string            short date line, e.g. "Thursday, November 26"
  dateline       Dateline
  groups         Group[]           quest groups, in display order
  inbox          TriageEntry[]     the triage strip's undecided candidates
  newToday       { id, headline, area }[]   lightweight pointers to items that surfaced today
  agenda         Agenda            the itinerary
  basics         string[]          field notes: one full-width ruled row per string
  whims          Whim[]
  aspirations    Aspiration[]
  modules        []                reserved; ship empty
  shared         []                reserved; ship empty

Dateline
  text           string            the one line of prose; required, never empty
  glyphs         GlyphKey[]        at most one weather key (see "The small vocabularies"); may be empty

Group
  area           string            the user's own area word, e.g. "Home"
  tone           ToneKey           the group's accent tone (see "The small vocabularies")
  items          Item[]

Item
  id             string            stable id; never changes across refreshes
  headline       string
  summary        string            one or two plain sentences
  status         Status            one of: needs_you, working, watching, waiting
  date           string | null     due date or deadline as "YYYY-MM-DD"; null when the item has none
  type           string            the errand kind: phone, computer, home, drive, person, waiting, needs_cover
  details        string            the long text: facts, links, history, what is being waited on
  objectives     string[]          checkable sub-steps, as plain step strings
  request        string            optional; a specific ask attached to the item, else ""
  source         { label: string, url: string } | null   where the item came from, when a link exists

TriageEntry        an Item, plus:
  area           string            the suggested area, attached by the sweep when the candidate is created
  firstSeen      string            "YYYY-MM-DD", the date the candidate first surfaced

Agenda
  today          { time: string, title: string, detail: string }[]   today's events, in time order
  upcoming       { date: string, time: string, title: string, detail: string }[]   the coming-days tiles

Whim
  id             string
  headline       string
  area           string

Aspiration
  title          string
  milestone      string            the current milestone, in plain words
  progress       string            a short progress label, e.g. "Cert. DN held"
```

### What readBoardState adds on top

The stored state above is what `replaceBoard` writes. `readBoardState` returns that same state plus the user's own layer, merged in:

- a flat `items` list: every item from every group, each carrying its `area`, its `done` flag (boolean), its saved `note` (string, `""` when empty), `noteUpdatedAt` (ISO timestamp or null), and its `objectives` expanded to `{ text: string, checked: boolean }[]`
- the `inbox` with each entry's decision state: an entry with no recorded decision is undecided and stays in the strip; decisions are recorded by `decideInboxItem` (below), never by editing the entry
- `dismissedWhims`: the ids of whims the user has dismissed
- `prefs`: the saved Customize preferences (see "Customize" in the Footer section)

### What the refresh must preserve

`replaceBoard` replaces the data block, and the board preserves the user's layer by stable id. Rely on this, and never route around it:

- an item's saved note and done flag survive any replace that keeps the item's id
- an objective's check-off survives as long as the item id and the step's exact text both survive; rewording a step resets its check-off, so keep step wording stable across runs and change it only when the underlying facts change
- undecided triage entries survive until the user decides or the entry is clearly moot; carry them forward in `inbox` on every write
- whim dismissals survive by whim id; never re-offer a dismissed whim under a new id to get it back on screen
- a quest lives in exactly one group at a time. When a quest qualifies as priority, the refresh moves it into the Priority group; when it stops qualifying, it moves back to its area group. The item's place in the flat `items` list still reports its home area, so the move loses nothing. Never render a quest in two groups at once.

Worked example, one item as `readBoardState` returns it (Dana's board):

```json
{
  "id": "truck-inspection",
  "area": "Home",
  "headline": "Truck inspection at the shop",
  "status": "waiting",
  "done": false,
  "note": "",
  "noteUpdatedAt": null,
  "objectives": [
    { "text": "Appointment booked", "checked": true },
    { "text": "Pick up truck", "checked": false }
  ]
}
```

Due within the window but waiting on the shop, so it sits in Home as `waiting` and stays out of Priority until the shop calls. That is the two-condition rule, expressed in the schema: `status` says who the item is waiting on, `date` says when it is due, and group placement says whether it is priority right now.

### The interaction actions

The three refresh actions above are the refresh job's contract. The user's taps on the board are not: never route a single tap through `replaceBoard`. Build these granular actions for user interactions; each writes one small fact, keyed by stable id, and returns success immediately.

| Action | Args | Behavior |
| --- | --- | --- |
| `decideInboxItem` | `itemId: string`, `decision: "approved" \| "declined"` | Records the user's triage decision. Approved: the candidate leaves the strip and joins its suggested area's quests at once, keeping its id and fields. Declined: the candidate leaves the strip and is gone. A decline means "not this one" only; record nothing about its category, sender, or topic. |
| `setObjectiveChecked` | `itemId: string`, `step: string`, `checked: boolean` | Saves one objective's check-off, keyed by the item id and the step's exact text. |
| `setNote` | `itemId: string`, `note: string` | Saves the user's note on one item and stamps when it changed. An empty string clears the note. The refresh reads notes first and obeys them; this action is the only write path for note text. |
| `setDone` | `itemId: string`, `done: boolean` | Saves one item's done flag. A done item leaves the open views and appears only under the Completed filter. |
| `setDateline` | `dateline: { text: string, glyphs?: GlyphKey[] }` | Updates only the dateline, leaving every other section untouched. For a dateline-only correction between full refreshes. |
| `dismissWhim` | `whimId: string` | Marks a whim dismissed. The strip hides it, shows the next stocked whim, and the dismissal persists across refreshes. |
| `undoAllDone` | none | Reopens every done item at once. Backs the Done bar's Undo all control. |

Two rules for whoever builds these:

- Every interaction writes through its own action at tap time, not on the next refresh. The refresh then reads the result as authoritative.
- `replaceBoard` is reserved for the refresh job. It must merge against a fresh `readBoardState` first, so a note or check-off saved seconds earlier is never lost to a stale write.

## The small vocabularies

These are the one-line choices every builder otherwise invents alone. Fix them here so boards built from this recipe share a language.

### Weather glyph keys

The dateline carries at most one weather glyph. The refresh job maps the day's forecast to exactly one of these keys; the board draws the glyph from a free weather glyph set (Meteocons or Weather Icons) under the same key names.

| Key | Meaning |
| --- | --- |
| `sun` | clear and sunny |
| `partly` | partly cloudy |
| `cloud` | overcast |
| `rain` | rain or showers |
| `snow` | snow |

The full glyph set the board may draw from also includes `moon`, `moon-full`, `delivery`, and `next`, for the rare dateline that leads with one of those instead. One weather key per dateline, chosen by the refresh from the live weather check, never by the board and never carried over from yesterday.

### Status vocabulary

An item's `status` is one of four values. It answers one question: who or what is this item waiting on?

| Status | Means | Marker color |
| --- | --- | --- |
| `needs_you` | the user can act on it now; it waits on them | the board's strongest accent (magenta in the original board) |
| `working` | under way; partly done, still moving | blue |
| `watching` | being monitored; no action owed by anyone yet | soft violet gray |
| `waiting` | blocked on a delivery, a reply, another person, or a future date | muted warm gray |

Status is rendered as a small marker with colored text, never a filled chip. Done is not a status: it is the separate `done` flag, and priority is not a status either: it is group placement, recomputed every refresh (see "What the refresh must preserve").

Group tones: each group also carries a `tone`, the soft color of its area heading, one of `blue`, `green`, `peach`, `purple`. Assign tones in the order the user gave their areas, cycling if there are more areas than tones.

### The triage strip

- Heading text: **"New for your review"**, followed by the waiting count and nothing else. No explanatory copy about the buttons, ever.
- Waiting-since format: each candidate shows "Waiting since <Mon D>" in short form, e.g. "Waiting since Tue 24", taken from its `firstSeen` date. Same-day candidates show "New today". The marker never shows an hour count and never turns red with age; waiting is information, not pressure.
- Each candidate carries its suggested `area` from the moment the sweep creates it, so Approve always has somewhere exact to promote it to.

### Itinerary depth

- `agenda.today`: today's events with times, in time order. All-day events lead the list, before the first timed event.
- `agenda.upcoming`: five day tiles, the five days after today, each with its date, a start time for its first event, a headline, and a one-line detail covering the day's events in brief. Five is the count: enough to see the week bend, few enough to stay a glance.

### Aspirations

An aspiration is exactly three strings: `title`, `milestone`, `progress` (see "The board state"). The `progress` label is the linkage the refresh needs: it names the tracked thread currently moving the milestone, in a few words. When no tracked work moved an aspiration in a run, all three strings carry forward unchanged.

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
