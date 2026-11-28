# REL — Read, Evaluate, Learn

> Living document. Updated as the idea is discussed and decisions are made. See [Decision log](#decision-log) and [Changelog](#changelog) at the bottom.

---

## Vision

Build a learning medium in the gap between long-form and reels: **short, human-verified cards**, **connected into a graph** so they add up to real depth. A card takes 1–2 minutes and carries one idea. Cards are grouped into **decks** of about 10–15 minutes: the smallest thing that stands on its own and gives the satisfaction of finishing something meaningful, without being so heavy that people quit. **Tracks** of decks take a learner from not knowing to understanding, one sitting at a time.

The product is not a format or an app. It is a **growing library of verified, connected knowledge**, with incentives designed so that learning, not attention, is what gets rewarded. No single medium is the core: a card uses whatever mix of text, visuals, audio, video or interaction best conveys its idea. Web, email, audio, courses and creator tools are all just ways of presenting the same graph.

**One line:** small effort per card, real depth over time, and a format that ends rather than one that keeps you scrolling.

---

## Ubiquitous language

The shared vocabulary for REL. Use these terms, and only these, in conversation, docs and code. When a term changes, update it here first.

### Agreed terms

#### Content

| Term | Definition | Notes |
|---|---|---|
| **Card** | The smallest reusable unit of knowledge: **multimedia content** (one or more media items) that conveys one idea in about **1–2 minutes**. | Replaces "unit". **No fixed internal structure**: whatever mix of media gets the idea across. A card does **not** have to stand alone; it may rely on the cards before it in a deck. A card exists on its own in the graph and is not owned by a deck. **Versioned.** |
| **Media item** | One piece of content inside a card: text, image/diagram, video, audio, or interactive. | A card is made of one or more media items. |
| **Tag** | A label from a **fixed, consistent, reviewed list** (e.g. `consistency-models`). Cards and decks can each carry many tags. | Replaces both "entity" and "topic"; there is no separate topic concept. Tags power tag pages, RSS per tag and filtering. New tags are added only through review. |
| **Deck** | An **ordered** sequence of cards that takes about **10–15 minutes** (roughly 5–10 cards) and is finished in one sitting. | **The smallest unit that stands on its own** and the unit of satisfaction: meaningful enough to feel worth it, light enough to finish. **No single-card decks.** Replaces "path". A card can belong to many decks. Different decks can explain the same subject (share tags) in different ways. A deck inherits its cards' tags as suggestions; the author makes the final choice, just as for cards. Curated by a human. **Versioned.** |
| **Track** | A saved, named sequence of decks, where each step follows a **deck relation** (e.g. "Distributed systems: Replication → Consensus → Raft in production"). | An object with per-learner progress. Spread over several sittings. |
| **Card reuse** | One card included in many decks, and so in many tracks. | An **authoring** idea: write an explanation once and use it everywhere. It has no effect on a learner's progress. |
| **Version** | An immutable snapshot of a card or a deck. Edits create a new version. | Decks are short enough to finish in one sitting, so expired or outdated deck versions mid-progress are not handled for now. |

#### Graph

| Term | Definition | Notes |
|---|---|---|
| **Relation** | A directed, typed link from one card to another (A → B). | Types: see below. |
| **Deck relation** | A directed link from one deck to another. | Tracks are built from deck relations. |
| **Knowledge graph** | All cards plus all relations between them. | The core asset. Decks and tracks are views over it. |

#### Learner activity

| Term | Definition | Notes |
|---|---|---|
| **Card completion** | A learner completes a card in a deck by acting to move forward from it (going to the next card). | Recorded per learner, per deck. Checks are not required. On the last card, the forward action is **Finish deck**. Jumping ahead does **not** complete the skipped cards. Going back never undoes completion. |
| **Deck progress** | For one learner and one deck: which cards in that deck they have completed. | Tracked **per deck, per learner**. A card completed in deck A is shown again, and not yet complete, in deck B, because the learner may come to B much later. |
| **Track progress** | For one learner and one track: their progress on the track's decks, added up. | |
| **Check** | One recall question about the material in a deck, with the answer hidden until the learner tries. | Given at the **end of a deck**, as part of **Finish deck**, and again days later as a spaced check. Always optional: a deck is complete without checks. Not part of any card. Never called a "card". |
| **Recall** | A learner answering a check correctly, usually days after reading the material. | **Separate from completion.** Completion drives progress bars. Recall drives outcome metrics, creator ranking and creator pay. |
| **Time on card** | How long a learner spent on a card before completing it. | Recorded quietly as a quality signal. Never used to block the learner. |

#### Parked

| Term | Notes |
|---|---|
| **Computed deck** | A deck generated from the graph (a target card plus everything it `requires`). Not in scope for now. |
| **Subtopics** | Not in scope for now. |

### Card relation types (draft)

| Relation | Meaning |
|---|---|
| **requires** | A must be understood before B. Drives reading order. |
| **follows** | B is the next development after A in a story (news/continuity). |
| **updates / contradicts** | B revises or corrects A. |

_The earlier_ `about` _relation (card → topic) is replaced by **tags**, which go on both cards and decks._

### Rules

- **Order respects** `requires` _(agreed)_. Within a deck, a card never comes before a card it requires.
- **Tracks follow deck relations** _(agreed)_. Each step from one deck to the next in a track is a deck relation.
- **Progress is per deck, per learner** _(agreed)_. Card reuse does not carry progress between decks.
- **Completion ≠ recall** _(agreed)_. Moving forward marks a card as completed; only a passed check counts as recall.
- **Sizes are targets, not limits** _(agreed)_. Card (1–2 min) and deck (10–15 min) sizes are starting points, tuned for everyone using learner data such as time on card and where learners drop off.
- **No single-card decks** _(agreed)_. A deck must be big enough to be a meaningful, standalone sitting.
- **Checks live at the end of a deck** _(agreed)_. Cards contain no checks. Checks are offered at Finish deck and again days later; they are always optional.
- **No core medium** _(agreed)_. Text is not required; each card uses whatever media best convey its idea.
- **Closure** _(proposed)_. If a card in a deck requires a card outside it, either the deck includes that card, or the deck has a `requires` deck relation to a deck that does.

### Under discussion (not yet agreed)

- **Version pinning.** Does a deck version point to specific card versions (pinned), or always to the latest version of each card?
- **Which version a learner sees.** Does a learner keep the deck version they started, or switch to the newest one on their next visit?
- **Deck relations: authored or derived?** Can they be computed from the card relations that cross deck boundaries?
- **Deck completion.** Is a deck complete when every card is complete, or when Finish deck is pressed?
- **Track progress formula.** Is it decks completed / decks in track, or weighted by cards?
- **Already-seen cards.** In deck B, should a card the learner completed in deck A be marked as "seen before" (and maybe used as a spaced-review moment)?
- **Text equivalent for non-text media.** Text is no longer required. Should every non-text media item still carry a short text equivalent (transcript, caption, alt text)? Without one, verification, search, AI drafting, accessibility and audio output all get harder.
- **Hooks.** The old card started with a hook (a question that makes you want the answer). Is a hook now a deck-level thing (the deck's opening), optional on cards, or dropped?
- **What replaces the fixed structure against clickbait?** The fixed template was one of the anti-clickbait defences (see Incentives). Without it, a written review standard for cards and decks has to do that job.
- **Card ownership.** Who can edit a card that other authors' decks reuse?

---

## The problem

Learning today forces a choice between two bad options: long-form that asks too much before paying back, and short-form that pays back instantly but leaves nothing behind.

| | Long-form (videos, blogs, courses) | Short-form (reels, shorts, threads) | The gap (REL) |
|---|---|---|---|
| **Effort before you get value** | High: 20–60 min | Seconds | Low per card |
| **Depth** | High | Low: half-knowledge | Builds up across cards |
| **Completion** | Poor; most people drop off | High, but nothing is retained | Each card completes; decks are short |
| **Structure** | One creator's sequence | None; driven by an algorithm | A graph of prerequisites |
| **Optimised for** | Watch time and views | Attention and scrolling | Learning outcomes |

The real issue isn't attention span. People still binge 3-hour podcasts. Long-form gives nothing back until you've invested a lot of time, with no promise you'll get there; reels give an instant hit that leaves nothing. Both are shaped by platforms that earn more when you watch longer, not when you learn more.

**The trap to avoid is the illusion of learning.** Content that's easy to follow feels like learning and is forgotten within days. Short text can fall into the same trap as reels unless the format offers recall, spacing and connection to those who want them.

---

## How it works

The system has three layers: **humans write verified cards**, the cards are **linked into a graph**, and the graph **produces decks and output formats**.

### 1. The card

A card is **multimedia content that conveys one idea in about 1–2 minutes**. It is made of one or more media items (text, image/diagram, audio, video, interactive) arranged however best gets the idea across. There is no fixed template. A card can rely on the cards before it in its deck; the **deck** is what stands on its own.

Reading or watching is the default. The format supports the principles of real learning for learners who want more:

- **Retrieval practice:** optional checks at the end of each deck, in the Finish deck step.
- **Spacing:** checks from earlier decks come back days later, optional and short.
- **Connection:** "builds on" and "leads to" come from the graph.
- **Concrete examples:** each concept comes with a worked example, in the same card or the next one.
- **Application:** an "apply it" scenario at the end of each deck (proposed).

### 2. The graph

The graph is made of **cards** joined by typed **relations**. Cards and decks carry **tags**, from a fixed list (used for tag pages and filtering). Relation types to start with:

| Link | Meaning | What it gives you |
|---|---|---|
| **Requires** | read this first | Reading order inside decks |
| **Follows** | the next development in a story | Continuity, "catch me up" |
| **Updates / contradicts** | later information revises earlier | Trust, corrections |

### 3. Decks and outputs

- **Deck:** an ordered sequence of cards taking about 10–15 minutes (roughly 5–10 cards), finished in one sitting, curated by a human.
- **Track:** an ordered sequence of decks along deck relations, spread over several sittings; learners see their progress through it.
- **News as hooks:** a thin layer of current events links into concepts (e.g. "Postgres 18 shipped async I/O; here are 3 cards to understand why it matters"). This gives learners a reason to come back today, and the graph gives them depth.
- **Outputs:** web pages, RSS per tag, email; later audio (each deck read out as a mini-podcast) and creator tools.

---

## How it differs from existing products

No existing product combines all three: **human-verified cards, a connected prerequisite graph, and incentives tied to learning outcomes.** Each existing product has one or two of these.

| Product | What it does well | Where it falls short | How REL differs |
|---|---|---|---|
| YouTube, blogs | Depth, huge variety | Long; no structure across creators; rewards views | Cards take 1–2 min, decks 10–15 min; tracks span creators |
| Reels, shorts | Instant and easy to start | Half-knowledge, distracting, nothing retained | Optional recall and spacing; cards connect |
| Inshorts, news aggregators | Short and fast | News, not learning; no continuity; engagement creep | Evergreen concepts; news is only a way in |
| Blinkist, Headway | Human-written distillations, audio | Book-shaped; each summary stands alone; no graph | Concept-level cards connected across sources |
| Brilliant, Imprint | Structured, visual, interactive | Closed catalogue; expensive to produce; STEM-heavy | Any medium, chosen per card; open to creators later |
| ByteByteGo and similar | Short visual technical explainers | One creator; no decks or recall | Prerequisite decks, with optional recall checks |
| Duolingo | Habit, completion, spaced repetition | Languages only; leans on streak anxiety | Same learning science, no anxiety-based gamification |
| Coursera, Udemy | Complete courses, certificates | Hours long; most learners drop off | Decks of 10–15 minutes; each deck is a win |
| AI tutors (ChatGPT, NotebookLM, Perplexity) | On demand, free, personal | Answer only what you ask; unverified; no structure | Verified; shows you what you didn't know to ask, in order |
| Substack, Medium | Creator tools and payments | Long posts; payment tied to subscriptions or reads | Creators paid for completion and recall |

**What's distinctive is the combination:**

- **Human verification as a published standard**, not just "written by a person".
- **A shared graph** that grows more valuable with every card, even across creators.
- **Built for completion:** cards finish, decks are short, progress is visible.
- **Rewarding outcomes, not attention:** the platform measures what people remember, not how long they watch.

---

## Where it works and where it doesn't

It fits best as a **structured, verified layer underneath other media**, not as a replacement for them. It struggles where speed, entertainment or conversation is the point.

### Works well with

| Combines with | How | Why it works |
|---|---|---|
| AI (drafting, structuring, suggesting links) | AI drafts, splits long content into cards, proposes edges and recall questions; a human verifies | Speeds up writing without weakening the human-verification moat |
| Long-form (videos, books, talks) | Cards link out to the best long-form source under "Go deeper"; creators turn their long content into decks | Cards make long-form approachable rather than competing with it |
| News | Current events used as hooks into concept decks | Solves the "why come back today" problem learning apps have |
| Audio and podcasts | Audio cards, or decks read out as audio where cards have text | Fits commutes and the gym; free only for text-based cards |
| Spaced-repetition tools (Anki-style) | Recall questions exported or scheduled | Proven learning science; retention you can measure |
| Teams and companies (B2B) | Onboarding decks, internal knowledge graphs | Companies pay for outcomes such as faster ramp-up |
| Communities (cohorts, discussion per card) | Threads attached to cards, not an open feed | Adds discussion without turning into doomscrolling |

### Doesn't work well with

| Area | Why it struggles |
|---|---|
| Ad-supported revenue | Ads reward attention; this product deliberately uses less of it |
| Breaking news and speed | Human verification is slow; aggregators and X will always be first |
| Entertainment-first content | Cards are built to end; reels win on pure entertainment |
| Hands-on skills (cooking, sports, physical crafts) | Video cards make these possible, but they are expensive to produce and verify, and still can't replace practice |
| Very broad catalogue early on | Writing time limits coverage; a thin graph across many subjects has no depth |
| Streak-anxiety gamification | Raises retention but brings back the pressure the product rejects |
| Fully open user-generated content (UGC) without review | Quality and verification collapse; clickbait returns |

---

## Risks

- **Production effort.** The earlier estimate (about 1.5–2 hours per card, about 25 hours per 15-card path) was for 3–5 minute text cards. It needs redoing for 1–2 minute multimedia cards and 10–15 minute decks. Multimedia (video, interactive) will cost more per minute than text.
- **Completion stays hard.** Short cards help, but most learning products lose most learners.
- **The graph's value comes late.** It only shows after about 100–200 cards; before that it looks like a blog.
- **Tag sprawl.** With many tags on cards and decks, and no weekly review, the list drifts (duplicates, synonyms, one-off labels) and the graph decays. The fixed list and review process exist to prevent this.
- **A crowded category.** "Bite-sized learning" is a common pitch, so the distinction has to show within the first 3 cards.
- **Sustainability.** It competes for time with an SVP role and a job search.

---

## Future: a creator platform

A creator platform is the natural endgame, but only if creators are **paid for what people learn, not for attention**. The format gets you in the door; the incentives and the shared graph are what last.

### What the moat actually is

A format alone gets copied within months; Twitter threads, Medium posts and carousels all were. These are harder to copy:

| Moat | Why it's hard to copy |
|---|---|
| Paying for learning outcomes | Incumbents can't switch without breaking their ad business |
| A shared graph across creators | Every new card links into existing decks; a competitor would have to rebuild all of it |
| Completion and recall data | Shows which explanation of a concept actually leads to learning |
| A verification brand | Trust takes years to build |

### Incentives that resist clickbait

Every creator platform drifts toward clickbait because creators optimise for whatever the platform rewards. The design has to reward the right thing from day one:

- **Rank and pay by outcomes:** completion rate, recall scores days later from learners who take the checks, and whether learners continue to the next deck. Views and time spent count for nothing.
- **Learners pay; there are no ads.** Subscription revenue is split by completed, retained learning.
- **A review standard instead of a fixed template** _(open)_. Cards no longer have a fixed structure, so padding, teasers and cliffhangers have to be caught by a written review standard for cards and decks.
- **Openings are factual, not teasers**, enforced through review.
- **Plan for gaming.** Creators will write easy recall questions, so the platform runs its own spaced checks and has review stages (verified creators, reviewed decks).

### Cold start: a tool first, the network later

Expert creators already have long-form content that people don't finish. The way in is a tool: **"Turn your long-form content into a completable deck."** AI splits the content into cards and drafts recall questions and links; the creator edits and verifies. Creators get value on day one without needing an audience, and every deck they publish adds to the shared graph, so the network effect builds up as a side effect. Substack grew the same way: a useful tool first, the network later.

---

## Roadmap and validation

### Phase 0 — what to measure (~6 weeks)

| Signal | Good sign | Warning sign |
|---|---|---|
| Completion | Over 50% finish the deck | Most drop off in the first third |
| Recall | 5-question quiz after 2 weeks mostly correct | Recall close to zero |
| Pull | "What's the next deck?" | Silence |
| Your consistency | Writing every week | Skipping by week 3 |
| Your energy | Writing and linking feel rewarding | It feels like a chore |

---

## Open questions

1. **Which subject for the first deck?** One you're an expert in. Backend and system design is the obvious candidate, but ByteByteGo already covers it; a narrower slice such as *distributed systems through real production incidents* may stand out more.
2. **Who are the first 20 learners**, and at what level (junior engineers, senior engineers, leaders)?
3. **What's the written verification standard?** Primary sources per claim, fact kept apart from opinion, a public corrections log.
4. **Where does gamification stop?** Progress yes, streak-loss anxiety no. Write the line down now.
5. **Text plus diagrams, or text only** for the first deck? Technical subjects likely need diagrams.
6. **How much news?** A thin hook layer from day one, or added after the first deck?
7. **Business model later:** learner subscription, B2B onboarding decks, or creators selling decks?
8. **The personal question:** does building this give you energy? Use the 6 weeks of phase 0 as input to the career self-analysis.

---

## Decision log

_Decisions made during discussion, newest first. Each entry: date — decision — reasoning._

- **2026-11-28** — **Text is not the core medium.** A card uses whatever mix of media best conveys its idea; text is not required. _(Replaces "text is the core medium" from the original vision.)_
- **2026-11-28** — **Checks live at the end of a deck** (the Finish deck step), plus spaced checks days later. Cards contain no checks. A single recall question is called a **check**. This keeps cards lean and measures recall at the deck, the smallest unit that stands alone.
- **2026-11-28** — **A card is multimedia content that conveys one idea in 1–2 minutes, with no fixed sections.** A card does not have to stand alone; the **deck is the smallest standalone unit**. **No single-card decks.** _(Replaces the five-part card: hook, idea, example, check, connects.)_
- **2026-11-28** — **Cards are 1–2 minutes; decks are about 10–15 minutes.** A deck should feel like finishing something meaningful without being so heavy that learners quit. Sizes are starting targets, tuned for everyone from learner data. _(Replaces 3–5 minute cards and 10–20 card, one-hour decks.)_
- **2026-11-28** — Reused cards are **shown again** in each deck. Completion and recall are **separate**: completion drives progress, recall drives outcomes and pay. **Time on card** is recorded but never blocks the learner. The last card completes with **Finish deck**, jumping ahead doesn't complete skipped cards, and going back never undoes completion. **Cards and decks are versioned**; decks are short (one sitting), so expired versions mid-progress are ignored for now. **A deck can be a single card**, and **tracks are made of deck relations**.
- **2026-11-28** — **Card reuse is an authoring idea, not a learner idea.** Progress is tracked **per deck, per learner**, so a card already completed in one deck is shown again in another (the learner may arrive much later). **Card completion** happens when the learner acts to move forward (goes to the next card). Checks are not required for completion.
- **2026-11-28** — **"Topic" dropped; only "tag".** Tags come from a fixed, consistent, reviewed list. A deck inherits its cards' tags as suggestions; the author makes the final choice (as for cards). **A track is an object**: a named, ordered sequence of decks, and a learner has progress on it. **Parked:** subtopics and computed decks. _(Supersedes the topic parts of earlier entries.)_
- **2026-11-28** — **Decks are ordered**, and a card never comes before a card it requires. Moving from deck to deck is called a **track**. Cards and decks both carry many **tags**, which decide which topics they belong to (a card or deck can be in many topics). Tags replace the `about` relation.
- **2026-11-28** — **Deck = path.** A deck is an ordered sequence of cards, and "path" is no longer a separate term. **Topic** is a separate concept from deck (a label from a fixed list), and many decks can share a topic. This keeps room for competing explanations of the same topic, which the outcome data needs.
- **2026-11-28** — **Decks are curated first, computed later.** At launch a human builds every deck. Computed decks come once the graph is dense enough.
- **2026-11-28** — The smallest knowledge unit is called a **card** (replaces "unit"). A card is made of **media items** (text, video, audio, interactive).
- **2026-11-28** — Cards are joined by directed **relations** to form the **knowledge graph**. Cards are grouped into **decks** (a topic or subtopic). Decks have **deck relations** to each other, and cards can be reused across decks.

---

## Changelog

- **2026-11-28** — Removed "text is the core medium" from the vision. Updated the Brilliant comparison, audio row, hands-on skills row and the production-effort risk. Opened a question on text equivalents for non-text media.
- **2026-11-28** — Added Check to the glossary. Moved checks to the end of decks. Updated the learning-principles list and rules. Removed the open question "where do checks live".
- **2026-11-28** — Rewrote the Card definition and "1. The card" section: multimedia, no fixed sections, not standalone. The deck is now the standalone unit, and single-card decks are dropped. Updated incentives (no fixed template) and opened questions on checks, hooks, text and the review standard. Re-applied the card and deck size changes, which had been overwritten on disk.
- **2026-11-28** — Reorganised the glossary into Content, Graph, Learner activity and Parked. Added Version, Recall and Time on card. Deck is now 1+ cards in one sitting. Track is now built from deck relations. Added the agreed rules.
- **2026-11-28** — Added Card reuse, Card completion, Deck progress and Track progress to the glossary. Replaced the open "what counts as complete" questions with narrower edge cases.
- **2026-11-28** — Removed Topic from the glossary and merged it into Tag. Redefined Track as an object with progress. Parked computed decks and subtopics. Replaced remaining "topic" wording in the body.
- **2026-11-28** — Added Tag and Track to the glossary. Removed the `about` relation in favour of tags. Marked the ordering rule as agreed. Renamed the "topic sprawl" risk to "tag sprawl".
- **2026-11-28** — Replaced "path" with "deck" and "entity" with "topic" throughout. Added Topic, Curated deck and Computed deck to the glossary. Added `about` as a card → topic relation, plus proposed ordering and closure rules.
- **2026-11-28** — Added the Ubiquitous language section. Renamed "unit" to "card" throughout the doc. Changed "text is the core unit" to "text is the core medium".
- **2026-11-28** — Converted original vision document into `project.md`. Reconstructed a few truncated passages from the source (e.g. "three layers", "apply it" scenario, "prerequisite paths", "~25 hours", "shared graph across creators", "review stages").
