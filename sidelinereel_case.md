# SidelineReel Product Case

## Executive summary

My roadmap recommendation is to fix player-identification reliability before adding major new surface area.

The product already demonstrates strong value when parents trust and open the reel: 92% of opened reels are shared. The larger problem is that roughly 30% of generated reels are never opened, and the available evidence points to late notifications and, more importantly, incorrect player identification in thumbnails and clips. Three support tickets across three different sports describe the same failure mode: visually similar jersey numbers are being confused.

I would therefore prioritize a **Player Identity Confidence Gate** that prevents low-confidence clips from automatically entering a player's reel and routes only ambiguous cases to a quick human review.

I would not build full livestreaming next. Ridgeline is commercially important and the $180k renewal risk is real, so I would treat it as an urgent commercial discovery track with a concrete answer for Carla within days. But the materials do not show broad demand for livestreaming, while they do show a repeated trust defect in the core product. Committing the roadmap to a large new capability before fixing that core reliability problem would increase product and delivery risk.

---

# Task 1 — Roadmap call

## Decision principles

I used four criteria:

1. **Customer impact:** how many users are likely affected and how severe the problem is.
2. **Evidence strength:** behavioral data and repeated observed failures weigh more than stated preference alone.
3. **Business risk:** retention and revenue concentration matter, especially for Ridgeline.
4. **Effort and reversibility:** smaller, testable fixes should beat large bets when evidence is incomplete.

## Ranked roadmap

### 1. Player Identity Confidence Gate

**Why it is first**

- Three independent support tickets across soccer, lacrosse, and baseball describe the same issue: the wrong player appears because similar jersey numbers are confused.
- The product-health note explicitly says incorrect thumbnails are one reason generated reels are never opened.
- Roughly 30% of generated reels are never opened.
- When a reel is opened, the product is already strong: 92% of opened reels are shared.
- This is therefore a trust and activation problem in the core experience, not a marginal feature request.

**Hypothesis**

If SidelineReel stops publishing clips when player identity is uncertain, parents will see fewer wrong-player reels and thumbnails, increasing trust and the percentage of generated reels that are opened.

**Primary metrics**

- Wrong-player support tickets per 1,000 generated reels.
- Percentage of generated reels opened.
- Percentage of reels requiring human review.
- False-accept rate for reviewed clips.

### 2. Notification timing optimization

The health dashboard says late-night notifications are another reason reels remain unopened.

This is likely lower effort than a new major feature and directly targets the same 30% unopened segment.

I would test send-time changes after or in parallel with the identity fix, because notification timing should not be optimized using reels whose thumbnails may still show the wrong child.

**Metrics**

- Reel open rate by send-time cohort.
- Time from notification to first open.
- Notification opt-out rate.

### 3. Ridgeline livestreaming — urgent discovery and constrained pilot, not full build commitment

Ridgeline is the largest club account at **$180k per season**, and Sales says it is the highest-risk renewal this quarter. Carla needs a concrete answer before signups open in six weeks.

I would immediately run a short discovery and feasibility track with Carla, Sales, and Engineering to define the minimum valuable version.

Examples to investigate:

- A limited one-field / one-game pilot.
- A third-party streaming integration instead of building the entire streaming stack.
- A scheduled live-link experience inside SidelineReel.
- A paid beta with explicit technical constraints.

Within one week, I would give Carla a specific answer: what can be piloted, with what scope, and by what date.

**Why this is not #1**

The evidence shows one very important account requesting it, but no evidence of broad customer demand. Full livestreaming is also described internally as a big lift. It should not displace a repeated trust defect affecting the core product until we know whether a smaller commercial solution can protect the renewal.

### 4. Reel Editor improvements

This is the clearest example of why I would not prioritize survey results in isolation.

- 58% of survey respondents selected more editing controls as their biggest request.
- Only 6% of parents opened the Reel Editor in the last 90 days.
- Fewer than 1% of those who opened it completed an edit.

The stated demand and observed behavior conflict.

Before building more editing functionality, I would investigate why users abandon the existing editor. The problem may be usability, discoverability, performance, or simply that parents like the idea of editing more than they actually want to do it.

I would not add more editor controls next.

### 5. Watermark reliability

The missing team-logo watermark is a real defect, but the ticket itself describes it as non-urgent and there is no evidence that it materially affects retention, activation, or revenue.

I would fix it opportunistically if the engineering effort is small.

### 6. Multi-sport app icon

This is a valid brand/usability observation but has the weakest evidence and lowest demonstrated business impact.

It should not compete with trust, activation, or renewal risk.

## What I would NOT build next

### Full livestreaming

I would **not commit to building full livestreaming as the next roadmap item**.

Reasons:

1. The request is strongly validated for one large customer, but broad demand is not established.
2. The internal material describes full livestreaming as a large lift.
3. There is an active reliability defect in the existing core product.
4. A rushed six-week build could create a second unreliable core experience.
5. We can protect the Ridgeline relationship more intelligently by committing to a concrete discovery/pilot decision immediately rather than promising a full platform we may not deliver well.

### More Reel Editor controls

I would also not build more editing controls next because usage data contradicts the survey request. I would diagnose abandonment before adding functionality.

---

# Task 2 — Product spec

## Feature name

**Player Identity Confidence Gate**

## Problem

SidelineReel sometimes assigns clips to the wrong player when jersey numbers look similar, including:

- 14 vs 4.
- Swapped digits.
- 1 vs 11.

This creates incorrect highlight reels and thumbnails.

Because the reel represents a child, a wrong-player clip is not a minor classification error. It damages trust in the entire product.

## Desired outcome

Reduce incorrect player clips and thumbnails while preserving the speed and automation that make weekly reels valuable.

### Business outcome

Increase parent trust and improve the percentage of generated reels that are opened.

### User outcome

A parent should be able to trust that every clip in their child's reel actually belongs to their child.

## Success metrics

### Primary

- Reduce wrong-player support tickets per 1,000 generated reels by at least 80% from baseline.
- Improve generated-reel open rate relative to the pre-launch baseline.

### Guardrails

- No more than 10% of clips require manual review after the first tuning period.
- Median review time under 10 seconds per flagged clip.
- Reel delivery SLA should not materially degrade.

The exact thresholds should be adjusted after measuring the current baseline.

## Core flow

1. SidelineReel detects candidate clips for a player.
2. Each candidate receives an identity-confidence score.
3. High-confidence clips continue automatically.
4. Low-confidence clips are blocked from publication.
5. A coach, team admin, or internal reviewer receives a small review queue.
6. The reviewer can:
   - Confirm the suggested player.
   - Assign the clip to another player.
   - Exclude the clip.
7. Only confirmed or high-confidence clips can appear in the final reel.
8. The selected reel thumbnail must come from a confirmed/high-confidence clip.

## In scope

- Confidence scoring on player identity before a clip enters a reel.
- A configurable confidence threshold.
- Blocking uncertain clips from automatic publication.
- A lightweight review queue for ambiguous clips.
- Reviewer actions: Confirm, Reassign, Exclude.
- Audit log containing:
  - Original prediction.
  - Confidence.
  - Reviewer action.
  - Final player assignment.
- Thumbnail selection restricted to trusted clips.
- Basic analytics for confidence distribution and reviewer corrections.

## Out of scope

- Rebuilding the underlying computer-vision model from scratch.
- Livestreaming.
- Additional Reel Editor controls.
- Notification scheduling changes.
- Manual review of every clip.
- Parent-facing identity correction inside the first release.
- Automatic face recognition.
- Changes to the app icon or watermark system.

## Human in the loop

A human is involved **only when confidence is below the release threshold**.

The reviewer should normally be a coach or club admin because they can recognize players quickly and already have a relationship with the roster.

For clubs that do not want to review clips, SidelineReel can default to excluding uncertain clips instead of risking a wrong-player publication.

The system should never force a reviewer to choose a player when they are unsure. **Exclude clip** must remain a first-class option.

## Functional requirements

### FR1 — Confidence state

Every candidate clip must have:

- Suggested player.
- Suggested jersey number.
- Confidence score.
- Status: Auto-approved, Needs review, Confirmed, Reassigned, or Excluded.

### FR2 — Threshold behavior

If confidence is at or above the configured threshold:

- Clip may enter the reel automatically.

If confidence is below the threshold:

- Clip must not enter the published reel until reviewed.

### FR3 — Review queue

Reviewer sees:

- Player whose reel is being generated.
- Clip preview.
- Suggested jersey number.
- Confidence.
- Likely alternative players when available.

### FR4 — Reviewer actions

Reviewer can:

- Confirm.
- Reassign.
- Exclude.

Each action is recorded.

### FR5 — Safe thumbnail

A reel thumbnail can only use a frame from:

- An auto-approved clip above threshold.
- A human-confirmed clip.

### FR6 — Empty-reel behavior

If all candidate clips are excluded:

- Do not fabricate or borrow another player's content.
- Deliver a graceful "No verified highlights this week" state or hold the reel according to existing product rules.

## Acceptance criteria

### AC1 — High-confidence clip

**Given** a clip assigned to player #14 with confidence above threshold  
**When** the reel is generated  
**Then** the clip may be included without human review.

### AC2 — Ambiguous number

**Given** a clip where the model is uncertain between #14 and #4  
**When** confidence falls below threshold  
**Then** the clip is absent from the publishable reel and appears in the review queue.

### AC3 — Confirm

**Given** a flagged clip in the review queue  
**When** the reviewer confirms #14  
**Then** the clip becomes eligible for #14's reel and the decision is logged.

### AC4 — Reassign

**Given** a clip predicted as #14  
**When** the reviewer selects player #4  
**Then** the clip is removed from #14's reel candidate set and assigned to #4.

### AC5 — Exclude

**Given** a clip where the reviewer cannot identify the player  
**When** they select Exclude  
**Then** the clip appears in no player's published reel.

### AC6 — Thumbnail protection

**Given** a reel with both verified and unresolved clips  
**When** a thumbnail is generated  
**Then** the thumbnail can only come from a verified clip.

### AC7 — No unresolved publication

**Given** any clip still in Needs review  
**When** the reel publication job runs  
**Then** that clip must not be included.

### AC8 — Auditability

**Given** a reviewer changes an assignment  
**When** an admin views the audit record  
**Then** the original prediction, confidence, final decision, and timestamp are visible.

## Rollout

### Phase 1 — Shadow mode

Run confidence scoring without changing output.

Measure:

- Confidence distribution.
- Likely review volume.
- Error clusters such as 14/4 and 1/11.

### Phase 2 — Small club pilot

Enable gating for a small group of clubs.

Include at least one club that has already reported the problem if they agree to participate.

### Phase 3 — Expand

Roll out if:

- Wrong-player errors decline.
- Review volume stays manageable.
- Reel delivery time remains healthy.

## Top failure mode 1

### Failure

The confidence model is overconfident and wrong clips still auto-publish.

### Why it matters

This defeats the purpose of the feature because the most damaging errors never reach the reviewer.

### Detection

- Sample a percentage of high-confidence auto-approved clips for QA.
- Track support tickets by confidence bucket.
- Compare human correction rate in shadow-mode samples.

### Response

Raise the threshold for risky number patterns and use category-specific rules for known confusions such as 14/4 or 1/11.

## Top failure mode 2

### Failure

Too many clips are flagged, creating reviewer fatigue and delaying reels.

### Why it matters

A safety mechanism that requires constant manual work destroys the automation advantage.

### Detection

Track:

- Percent of clips flagged.
- Median review time.
- Reels delayed by review.
- Queue abandonment.

### Response

Tune thresholds, prioritize only the highest-risk ambiguity classes, improve the model using reviewed examples, and default to excluding low-value uncertain clips rather than requiring every ambiguity to be resolved.

---

# Task 3 — Prototype

The prototype shows the identity-confidence feature inside a more complete SidelineReel product flow:

1. The club dashboard displays multiple AI-generated weekly reels with realistic states: Ready, Published, and Needs review.
2. Ready and published reels can be opened in a working reel player that cycles through generated highlights.
3. Maya's reel is already generated, but two uncertain clips are blocked by the Player Identity Confidence Gate.
4. The reviewer opens the queue and confirms, reassigns, or excludes each ambiguous clip.
5. Once all flagged clips are resolved, the reel becomes safe to publish.
6. The final verified reel can be opened in the same player, demonstrating that the safety gate protects publication without removing the automated reel experience.

The prototype is intentionally focused on the core trust and review flow rather than simulating livestreaming, editing, or the entire production application.

See the accompanying `sidelinereel_prototype.html`.

---

# Task 4 — AI Workflow Note

I used AI to help structure the evidence, pressure-test the roadmap ranking, draft acceptance criteria, and accelerate the clickable prototype.

I deliberately kept the actual prioritization decision, trade-offs, scope boundaries, and final product judgment human. In particular, I did not let the loudest request automatically become the top roadmap item.

One thing I chose not to accept from AI was the temptation to treat the 58% survey result for editing controls as decisive. The behavioral data shows only 6% of parents opened the editor and fewer than 1% of those users completed an edit, so I weighted observed behavior more heavily than stated preference.

---

# Video walkthrough outline

## 0:00–0:30 — Intro

Hi, I'm Federico San Lorenzo. I'll walk through my roadmap decision, the spec for the top item, and a working prototype of the core flow.

## 0:30–2:30 — Roadmap

The key pattern I saw is that SidelineReel already creates strong value when the reel is trusted and opened.

Ninety-two percent of opened reels are shared, so I would not start by adding more creation functionality.

The bigger issue is that about 30% of generated reels are never opened. The materials point to two causes: late notifications and wrong-player thumbnails.

The strongest repeated qualitative signal is player misidentification. There are three tickets across three sports, all describing similar jersey-number confusion.

So my number-one roadmap item is a Player Identity Confidence Gate.

Second is notification timing optimization.

Third is an urgent Ridgeline livestreaming discovery and pilot track because the $180,000 renewal is clearly important.

I would not commit to full livestreaming as the next major build yet. It is a large lift, the evidence currently comes from one account, and the product has a core trust defect that should be fixed first.

I also would not expand the Reel Editor yet. Fifty-eight percent of surveyed parents asked for more controls, but only six percent opened the current editor and fewer than one percent of those completed an edit. I would diagnose that mismatch before adding functionality.

## 2:30–5:00 — Spec

The goal of the Player Identity Confidence Gate is simple: a parent should be able to trust that every clip in their child's reel is actually their child.

Every candidate clip gets an identity confidence score.

High-confidence clips continue automatically.

Low-confidence clips are blocked and routed to a lightweight human review.

The reviewer can confirm the suggested player, reassign the clip, or exclude it.

That last option is important. The system should never force a human to guess.

Only high-confidence or human-confirmed clips can enter a reel or become its thumbnail.

The human stays in the loop only for ambiguous clips, because reviewing every clip would destroy the value of automation.

The primary success metric is wrong-player incidents per thousand generated reels, with reel open rate as the key downstream product metric.

I would also watch the percentage of clips flagged and review time as guardrails.

## 5:00–7:00 — Prototype demo

Here I have a weekly reel for Maya, number 14.

The system has generated five clips, but two have low identity confidence.

Instead of silently publishing them, SidelineReel shows a review-needed state.

When I open the queue, I can see the first ambiguous clip. The model thinks it is number 14, but number 4 is also a likely match.

If I recognize Maya, I confirm number 14.

On another clip, I can reassign it to the correct player or exclude it if I cannot identify the player confidently.

Once every uncertain clip is resolved, the reel becomes safe to publish.

The key point is that we preserve automation for the majority of clips while placing a human exactly where the model is least trustworthy.

## 7:00–8:00 — Failure modes

The first major failure mode is an overconfident model. If wrong clips still receive high confidence, they bypass the gate.

I would catch that with QA sampling of high-confidence clips and by tracking support incidents by confidence bucket.

The second risk is flagging too much and creating review fatigue.

I would monitor review volume, time per clip, and delayed reels, then tune the threshold and improve the model using review decisions.

## 8:00–9:00 — Close

A little about me: I'm Federico San Lorenzo, a software developer and technical project professional based in Argentina. My background combines software development, APIs, automation, AI workflows, and client-facing project ownership.

That is why this kind of role is especially interesting to me. I like working at the point where user problems, business constraints, and technical execution meet. I also enjoy using AI as a tool to move faster without outsourcing the actual product judgment.

What attracted me to Ajaia is the opportunity to work on practical AI products where quality, trust, and real user outcomes matter, rather than building AI just for the sake of it.

For compensation, my expectation is USD 45,000 gross annually, depending on the final scope and total package.

Thank you for your time.
