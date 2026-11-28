# REL — Read, Evaluate, Learn

> Living document, updated as the idea is discussed. History is in git.

---

## Vision

Build a learning medium in the gap between long-form and reels: **short, human-verified cards**, **connected into a graph** so they add up to real depth. A card takes 1–2 minutes and carries one idea. Cards are grouped into **decks** of about 10–15 minutes: the smallest thing that stands on its own and gives the satisfaction of finishing something meaningful, without being so heavy that people quit. **Tracks** of decks take a learner from not knowing to understanding, one sitting at a time.

The product is not a format or an app. It is a **growing library of verified, connected knowledge**, with incentives designed so that learning, not attention, is what gets rewarded. No single medium is the core: a card uses whatever mix of text, visuals, audio, video or interaction best conveys its idea. Web, email, audio, courses and creator tools are all just ways of presenting the same graph.

**One line:** small effort per card, real depth over time, and a format that ends rather than one that keeps you scrolling.

---

## Ubiquitous language

The shared vocabulary for REL. Use these terms, and only these, in conversation, docs and code. When a term changes, update it here first.

**Not used:** unit, path, topic, entity. Use card, deck, tag instead. A recall question is a **check**, never a "card".

### Content

| Term | Definition | Notes |
|---|---|---|
| **Card** | The smallest reusable unit of knowledge: **multimedia content** (one or more media items) that conveys one idea in about **1–2 minutes**. | No fixed internal structure. Does **not** have to stand alone; it may rely on the cards before it in a deck. Exists on its own in the graph and is not owned by a deck. **Versioned.** |
| **Media item** | One piece of content inside a card: text, image/diagram, video, audio, or interactive. | A card is made of one or more media items. |
| **Tag** | A label from a **fixed, consistent, reviewed list** (e.g. `consistency-models`). Cards and decks can each carry many tags. | Powers tag pages, RSS per tag and filtering. New tags are added only through review. |
| **Deck** | An **ordered** sequence of cards that takes about **10–15 minutes** (roughly 5–10 cards) and is finished in one sitting. | **The smallest unit that stands on its own**: meaningful enough to feel worth it, light enough to finish. Curated by a human. Different decks can explain the same subject (share tags) in different ways. Inherits its cards' tags as suggestions; the author makes the final choice. **Versioned.** |
| **Track** | A saved, named sequence of decks, where each step follows a **deck relation** (e.g. "Distributed systems: Replication → Consensus → Raft in production"). | Spread over several sittings. Has per-learner progress. |
| **Card reuse** | One card included in many decks, and so in many tracks. | An **authoring** idea: write once, use everywhere. No effect on learner progress. |
| **Version** | An immutable snapshot of a card or a deck. Edits create a new version. | Decks are finished in one sitting, so outdated deck versions mid-progress are not handled for now. |

### Graph

| Term | Definition | Notes |
|---|---|---|
| **Relation** | A directed, typed link from one card to another (A → B). | Types below. |
| **Deck relation** | A directed link from one deck to another. | Tracks are built from deck relations. |
| **Knowledge graph** | All cards plus all relations between them. | The core asset. Decks and tracks are views over it. |

**Relation types (draft):**

| Relation | Meaning | What it gives you |
|---|---|---|
| **requires** | A must be understood before B | Reading order inside decks |
| **follows** | B is the next development after A in a story | Continuity, "catch me up" |
| **updates / contradicts** | B revises or corrects A | Trust, corrections |

### Learner activity

| Term | Definition | Notes |
|---|---|---|
| **Card completion** | A learner completes a card in a deck by acting to move forward from it (going to the next card). | Per learner, per deck. On the last card, the forward action is **Finish deck**. Jumping ahead does not complete skipped cards. Going back never undoes completion. |
| **Deck progress** | For one learner and one deck: which cards in that deck they have completed. | A card completed in deck A is shown again, not yet complete, in deck B; the learner may come to B much later. |
| **Track progress** | For one learner and one track: their progress on the track's decks, added up. | |
| **Check** | One recall question about the material in a deck, answer hidden until the learner tries. | Offered at **Finish deck** and again days later as a spaced check. Always optional. Not part of any card. |
| **Recall** | A learner answering a check correctly, usually days after reading the material. | Drives outcome metrics, creator ranking and creator pay. |
| **Time on card** | How long a learner spent on a card before completing it. | Recorded quietly as a quality signal. Never blocks the learner. |

### Rules

- **Order respects `requires`.** Within a deck, a card never comes before a card it requires.
- **Tracks follow deck relations.** Each step from one deck to the next is a deck relation.
- **Progress is per deck, per learner.** Card reuse does not carry progress between decks.
- **Completion ≠ recall.** Moving forward completes a card (drives progress bars); only a passed check counts as recall (drives outcomes and pay). Keeps progress free of quizzes while outcomes stay measurable.
- **Checks live at the end of a deck.** Cards contain no checks; checks are offered at Finish deck and again days later, always optional.
- **No single-card decks.** A deck must be a meaningful, standalone sitting.
- **No core medium.** Text is not required; each card uses whatever media best convey its idea.
- **Sizes are targets, not limits.** Card (1–2 min) and deck (10–15 min) sizes are starting points, tuned for everyone using learner data such as time on card and where learners drop off.
- **Closure** _(proposed)_. If a card in a deck requires a card outside it, either the deck includes that card, or the deck has a `requires` deck relation to a deck that does.

### Parked

- **Computed decks:** decks generated from the graph (a target card plus everything it requires).
- **Subtopics.**

### Under discussion

- **Version pinning.** Does a deck version point to specific card versions (pinned), or always to the latest version of each card?
- **Which version a learner sees.** Does a learner keep the deck version they started, or switch to the newest one on their next visit?
- **Deck relations: authored or derived?** Can they be computed from the card relations that cross deck boundaries?
- **Deck completion.** Is a deck complete when every card is complete, or when Finish deck is pressed?
- **Track progress formula.** Decks completed / decks in track, or weighted by cards?
- **Already-seen cards.** In deck B, should a card completed in deck A be marked "seen before" (and maybe used as a spaced-review moment)?
- **Text equivalent for non-text media.** Should every non-text media item carry a short text equivalent (transcript, caption, alt text)? Without one, verification, search, AI drafting, accessibility and audio output all get harder.
- **Hooks.** Should a deck open with a "why should I care" question, or is that dropped?
- **Review standard.** Cards have no fixed template, so a written review standard for cards and decks has to keep out padding, teasers and cliffhangers.
- **Card ownership.** Who can edit a card that other authors' decks reuse?

---

## The problem

Learning today forces a choice between two bad options: long-form that asks too much before paying back, and short-form that pays back instantly but leaves nothing behind.

| | Long-form (videos, blogs, courses) | Short-form (reels, shorts, threads) | The gap (REL) |
|---|---|---|---|
| **Effort before you get value** | High: 20–60 min | Seconds | Low per card |
| **Depth** | High | Low: half-knowledge | Builds up across cards |
| **Completion** | Poor; most people drop off | High, but nothing is retained | Decks are short and finishable |
| **Structure** | One creator's sequence | None; driven by an algorithm | A graph of prerequisites |
| **Optimised for** | Watch time and views | Attention and scrolling | Learning outcomes |

The real issue isn't attention span. People still binge 3-hour podcasts. Long-form gives nothing back until you've invested a lot of time, with no promise you'll get there; reels give an instant hit that leaves nothing. Both are shaped by platforms that earn more when you watch longer, not when you learn more.

**The trap to avoid is the illusion of learning.** Content that's easy to follow feels like learning and is forgotten within days. Short content falls into the same trap as reels unless the format offers recall, spacing and connection to those who want them.

---

## How it works

Humans make **verified cards**, the cards are **linked into a graph**, and the graph is presented as **decks, tracks and output formats**.

The format supports the principles of real learning for learners who want more:

- **Retrieval practice:** optional checks at the end of each deck.
- **Spacing:** checks from earlier decks come back days later, optional and short.
- **Connection:** "builds on" and "leads to" come from the graph.
- **Concrete examples:** each concept comes with a worked example, in the same card or the next one.
- **Application:** an "apply it" scenario at the end of each deck (proposed).

**News as hooks:** a thin layer of current events links into decks (e.g. "Postgres 18 shipped async I/O; here's a 10-minute deck on why it matters"). This gives learners a reason to come back today, and the graph gives them depth.

**Outputs:** web pages, RSS per tag, email; later audio and creator tools.

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
| ByteByteGo and similar | Short visual technical explainers | One creator; no decks or recall | Prerequisite decks, with optional checks |
| Duolingo | Habit, completion, spaced repetition | Languages only; leans on streak anxiety | Same learning science, no anxiety-based gamification |
| Coursera, Udemy | Complete courses, certificates | Hours long; most learners drop off | Decks of 10–15 minutes; each deck is a win |
| AI tutors (ChatGPT, NotebookLM, Perplexity) | On demand, free, personal | Answer only what you ask; unverified; no structure | Verified; shows you what you didn't know to ask, in order |
| Substack, Medium | Creator tools and payments | Long posts; payment tied to subscriptions or reads | Creators paid for completion and recall |

**What's distinctive is the combination:**

- **Human verification as a published standard**, not just "written by a person".
- **A shared graph** that grows more valuable with every card, even across creators.
- **Built for completion:** decks are short, progress is visible.
- **Rewarding outcomes, not attention:** the platform measures what people remember, not how long they watch.

---

## Where it works and where it doesn't

It fits best as a **structured, verified layer underneath other media**, not as a replacement for them. It struggles where speed, entertainment or conversation is the point.

### Works well with

| Combines with | How | Why it works |
|---|---|---|
| AI (drafting, structuring, suggesting links) | AI drafts, splits long content into cards, proposes relations and checks; a human verifies | Speeds up production without weakening the human-verification moat |
| Long-form (videos, books, talks) | Cards link out to the best long-form source under "Go deeper"; creators turn their long content into decks | Makes long-form approachable rather than competing with it |
| News | Current events used as hooks into decks | Solves the "why come back today" problem learning apps have |
| Audio and podcasts | Audio cards, or decks read aloud where cards have text | Fits commutes and the gym |
| Spaced-repetition tools (Anki-style) | Checks exported or scheduled | Proven learning science; retention you can measure |
| Teams and companies (B2B) | Onboarding decks, internal knowledge graphs | Companies pay for outcomes such as faster ramp-up |
| Communities (cohorts, discussion per card) | Threads attached to cards, not an open feed | Adds discussion without turning into doomscrolling |

### Doesn't work well with

| Area | Why it struggles |
|---|---|
| Ad-supported revenue | Ads reward attention; this product deliberately uses less of it |
| Breaking news and speed | Human verification is slow; aggregators and X will always be first |
| Entertainment-first content | Decks are built to end; reels win on pure entertainment |
| Hands-on skills (cooking, sports, physical crafts) | Video cards make these possible, but they are expensive to produce and verify, and can't replace practice |
| Very broad catalogue early on | Production time limits coverage; a thin graph across many subjects has no depth |
| Streak-anxiety gamification | Raises retention but brings back the pressure the product rejects |
| Fully open user-generated content (UGC) without review | Quality and verification collapse; clickbait returns |

---

## Risks

- **Production effort.** Per-card and per-deck effort needs estimating; multimedia (video, interactive) costs more per minute than text.
- **Completion stays hard.** Short decks help, but most learning products lose most learners.
- **The graph's value comes late.** It only shows after about 100–200 cards; before that it looks like a blog.
- **Tag sprawl.** Without regular review, the tag list drifts (duplicates, synonyms, one-off labels) and the graph decays.
- **A crowded category.** "Bite-sized learning" is a common pitch, so the difference has to show within the first deck.

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

- **Rank and pay by outcomes:** completion rate, recall days later, and whether learners continue to the next deck. Views and time spent count for nothing.
- **Learners pay; there are no ads.** Subscription revenue is split by completed, retained learning.
- **A written review standard** keeps out padding, teasers and cliffhangers; openings are factual, not teasers.
- **Plan for gaming.** Creators will write easy checks, so the platform runs its own spaced checks and has review stages (verified creators, reviewed decks).

### Cold start: a tool first, the network later

Expert creators already have long-form content that people don't finish. The way in is a tool: **"Turn your long-form content into completable decks."** AI splits the content into cards and drafts checks and relations; the creator edits and verifies. Creators get value on day one without needing an audience, and every deck they publish adds to the shared graph, so the network effect builds up as a side effect. Substack grew the same way: a useful tool first, the network later.

---

## Roadmap and validation

### Phase 0 — what to measure (~6 weeks)

| Signal | Good sign | Warning sign |
|---|---|---|
| Completion | Over 50% finish the deck | Most drop off in the first third |
| Recall | Checks after 2 weeks mostly correct | Recall close to zero |
| Pull | "What's the next deck?" | Silence |
| Consistency | Publishing every week | Skipping by week 3 |

---

## Open questions

1. **Which subject for the first deck?** One you're an expert in. Backend and system design is the obvious candidate, but ByteByteGo already covers it; a narrower slice such as *distributed systems through real production incidents* may stand out more.
2. **Who are the first 20 learners**, and at what level (junior engineers, senior engineers, leaders)?
3. **What's the written verification standard?** Primary sources per claim, fact kept apart from opinion, a public corrections log.
4. **Where does gamification stop?** Progress yes, streak-loss anxiety no. Write the line down now.
5. **Which media for the first deck?** Technical subjects likely need diagrams; video costs more to produce and verify.
6. **How much news?** A thin hook layer from day one, or added after the first deck?
7. **Business model later:** learner subscription, B2B onboarding decks, or creators selling decks?
