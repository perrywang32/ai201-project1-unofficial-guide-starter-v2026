# The Unofficial Guide

> Perry Wang — campus_life corpus

> **This file is your submission.** Fill it in as you go — most sections get
> written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`.
> Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets
> full credit; a picture of the same table gets none.
>
> Delete these instruction blocks as you replace them. The `<!-- -->` comments
> are notes to you and don't show up when the page renders — you can leave them
> or remove them.

---

# Unit 1

## What This Does

<!-- Three or four sentences. Which corpus you picked, and the kinds of
     questions your system answers. Write it for someone who has never seen
     this repo.

     Milestone 5. -->
This project answers questions about the campus-life corpus using document
retrieval and a language model. It loads the campus documents, splits them into
paragraph-aware chunks, retrieves the chunks most related to a question, and
generates an answer using only those retrieved documents. It also names its
sources and refuses questions when the documents do not contain enough
information.

## Chunking Strategy

**Chunk size:**
**Overlap:**

<!-- What about YOUR documents made you pick these numbers? Short posts and
     long sectioned guides don't want the same chunking, and "800 seemed
     reasonable" earns nothing. Point at something you noticed when you read
     the documents in Milestone 1.

     If you changed your mind partway through, say so and say why. That's worth
     more than pretending you got it right first time.

     Milestone 3. -->

## Sample Chunks

<!-- Five chunks, pasted as text. Label each one and name the file it came from
     AND the function that produced it — the grader checks your code against
     what you claim here.

     `python app.py chunks -n 5` prints all three for you. Copy them straight
     across.

     Milestone 3. -->

**Chunk 1** — source: `admin_add_drop_deadline.txt#0` — produced by: `chunker.py::split_documents`

```
On the add/drop deadline

You can add a course through the end of the second week. Dropping is a longer window — through the end of week six — but a drop after week two shows as a W on your transcript. Nothing anywhere on the registrar's site says this plainly, and students find out from each other.
```

**Chunk 2** — source: `course_biol_160.txt#0` — produced by: `chunker.py::split_documents`

```
BIOL 160 Cell Biology

I lived here my sophomore year. Format is lecture three times a week with a weekly lab. Assessment: four unit tests and a cumulative final. Not curved.

Expect 9 to 11 hours a week, the heaviest first-year course by reputation.

The one piece of advice: the unit tests come fast, roughly every three weeks; falling behind once is very hard to recover from.
```

**Chunk 3** — source: `course_hist_118_workload.txt#0` — produced by: `chunker.py::split_documents`

```
Workload for HIST 118 Modern World History

People keep asking so: a lot of reading, about 120 pages a week, but no problem sets. That's real time, not optimistic time.

It's front-loaded — the first month is heavier than the rest, partly because you're learning the format.
```

**Chunk 4** — source: `dining_pellew_dining_hall_followup.txt#0` — produced by: `chunker.py::split_documents`

```
Re: Pellew Dining Hall

Adding to what people have said about Pellew Dining Hall. The wait figure of 12 to 18 minutes at peak matches what I've seen. If you're trying to eat between classes, go before 11:45 and it's a different building entirely.

Also worth saying: the furthest hall from anywhere, next to the athletics centre. Nobody tells you this at orientation.
```

**Chunk 5** — source: `housing_innisfree_hall.txt#0` — produced by: `chunker.py::split_documents`

```
Innisfree Hall — what it's actually like

Transferred in last year, so take this with a grain of salt. Built 1991, renovated 2022. Rooms are doubles arranged as pairs sharing one bathroom between two rooms.

The good: the shared-bathroom-between-two-rooms arrangement is the best compromise on campus.

The bad: no air conditioning, which matters for the first three weeks of September.

Laundry costs $1.75 wash, $1.75 dry, app-based. On noise: moderate; the building is L-shaped and the short wing is much quieter.

For each one, ask: could someone answer a question using only this,
without reading what came before or after?
```

## Sample Answer

<!-- One complete question and answer, pasted as text, with the source line
     visible. Milestone 4. -->

**Question:** Is the housing lottery completely random?

**Answer:** No, the housing lottery is not completely random. Rising sophomores receive a randomly drawn number, while juniors and seniors are ordered by accumulated credit hours first, with random selection used only for tie-breaks.

**Source:** `admin_housing_lottery.txt`

```
```

**My relevance cutoff:** `0.6`

<!-- The number you set in config.py, and how you got there.

     You ran five questions your corpus covers and the five in OUT_OF_SCOPE
     that it clearly doesn't, and wrote down the best distance for each. What
     did those two groups look like? Where was the gap? Put the actual numbers
     here — the table below wants all ten rows.

     Milestone 4. -->

| Question | In corpus? | Best distance |
|---|---|---|
|  |  |  |
| Is the housing lottery completely random? | Yes | 0.251 |
| What determines priority in the housing lottery? | Yes | 0.361 |
| What is the deadline or rule for obtaining a campus parking permit? | Yes | 0.521 |
| What specific requirement does the advising registration guide give students? | Yes | 0.618 |
| What policy or feature of Tamsin Court is described in the housing document? | Yes | 0.478 |
| What is the capital of Mongolia? | No | 0.825 |
| How do I change the oil in a diesel engine? | No | 0.934 |
| Who won the 1994 World Cup? | No | 0.886 |
| What is the recommended dosage of ibuprofen for a headache? | No | 0.844 |
| How do I write a for loop in Rust? | No | 0.896 |

The in-corpus questions were mostly below the `0.6` cutoff, while all five
out-of-corpus questions were above it. The gate refused all five out-of-corpus
questions and returned "I don't have enough information about that," so the
cutoff cleanly separated the two groups for this test.
## How I Used AI

<!-- Two specific moments. For each: what you asked for, what came back, and
     what you changed about it.

     "I asked Claude to write the chunking function from my notes. It ignored
     the overlap, so I added that myself" is the level of detail we're after.
     "I used AI to help me code" is not.

     Milestone 5. -->
**1.** I asked AI for help replacing the starter fixed-size chunker. It suggested
a paragraph-aware strategy with a maximum chunk size of about 600 characters.
I kept the existing `Chunk` structure and changed the implementation so
paragraphs stayed together and the function reported
`chunker.py::split_documents`.

**2.** I asked AI to help interpret the retrieval results and choose a relevance
cutoff. It helped me compare the in-corpus and out-of-corpus distances. I kept
the cutoff at `0.6` because all five out-of-corpus questions were refused while
four of five in-corpus questions were answered.

<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

<!-- Your five criteria, three runs each. `python run_eval.py --label before`
     runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
     writes it all into results/ for you. Targets come from criteria.md; the
     verdict column is your call.

     Criterion 3 is measured in one deterministic pass rather than three, so
     the same number goes in all three run columns. That's correct, not lazy.

     Milestone 1. -->
| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 4/5 | 4/5 | 4/5 | MET |
| 2. Every answer names a source | 5 of 5 | check results | check results | check results | decide |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. Your chunk criterion | your target | decide | decide | decide | decide |
| 5. Your choice | your target | decide | decide | decide | decide |
<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->
## Relevance gate output

| Out-of-scope question | Best distance | Gate |
|---|---:|---|
| What is the capital of Mongolia? | 0.825 | refused |
| How do I change the oil in a diesel engine? | 0.934 | refused |
| Who won the 1994 World Cup? | 0.886 | refused |
| What is the recommended dosage of ibuprofen for a headache? | 0.844 | refused |
| How do I write a for loop in Rust? | 0.896 | refused |

### Is the housing lottery completely random? — run 1

- Best distance: 0.2514 (passed the gate)
- Sources retrieved: admin_housing_lottery.txt, admin_parking_permits.txt, advising_registration.txt, housing_morrow_house.txt, housing_tamsin_court.txt

```
No, the housing lottery is not completely random. Rising sophomores get a number drawn at random, but juniors and seniors are ordered by accumulated credit hours first, and only tie-break randomly (*admin_housing_lottery.txt*).
```

### Is the housing lottery completely random? — run 2

- Best distance: 0.2514 (passed the gate)
- Sources retrieved: admin_housing_lottery.txt, admin_parking_permits.txt, advising_registration.txt, housing_morrow_house.txt, housing_tamsin_court.txt

```
No, the housing lottery is not completely random. Rising sophomores get a random number drawn, but juniors and seniors are ordered by accumulated credit hours first, with random selection used only as a tie-breaker (*admin_housing_lottery.txt*).
```

### Is the housing lottery completely random? — run 3

- Best distance: 0.2514 (passed the gate)
- Sources retrieved: admin_housing_lottery.txt, admin_parking_permits.txt, advising_registration.txt, housing_morrow_house.txt, housing_tamsin_court.txt

```
No, the housing lottery is not completely random. Rising sophomores have a number drawn at random, but juniors and seniors are ordered by accumulated credit hours first, with random tie-breaking used only as a tie-breaker (*admin_housing_lottery.txt*).
```

### What determines priority in the housing lottery? — run 1

- Best distance: 0.3615 (passed the gate)
- Sources retrieved: admin_housing_lottery.txt, advising_registration.txt, course_stat_150_exams.txt, dining_the_atrium_followup.txt, housing_tamsin_court.txt

```
Based on the provided documents, rising sophomores get a number drawn at random, while juniors and seniors are ordered by accumulated credit hours first, with a random tie-break used if needed. (Source: admin_housing_lottery.txt)
```

### What determines priority in the housing lottery? — run 2

- Best distance: 0.3615 (passed the gate)
- Sources retrieved: admin_housing_lottery.txt, advising_registration.txt, course_stat_150_exams.txt, dining_the_atrium_followup.txt, housing_tamsin_court.txt

```
For the housing lottery, rising sophomores get a number drawn at random, while juniors and seniors are ordered first by accumulated credit hours, with tie-breaks handled randomly (*admin_housing_lottery.txt*).
```

### What determines priority in the housing lottery? — run 3

- Best distance: 0.3615 (passed the gate)
- Sources retrieved: admin_housing_lottery.txt, advising_registration.txt, course_stat_150_exams.txt, dining_the_atrium_followup.txt, housing_tamsin_court.txt

```
In the housing lottery, rising sophomores get a number drawn at random, while juniors and seniors are ordered first by accumulated credit hours, with random tie-breaks used only when necessary. 

Source: `admin_housing_lottery.txt`
```

### What is the deadline or rule for obtaining a campus parking permit? — run 1

- Best distance: 0.5208 (passed the gate)
- Sources retrieved: admin_add_drop_deadline.txt, admin_parking_permits.txt, advising_registration.txt, transit_shuttle.txt, transit_walking.txt

```
Student permits for the west lots go on sale in August and typically sell out in about three days, while the east lot never sells out. There is no waitlist for permits. 

Source: `admin_parking_permits.txt`
```

### What is the deadline or rule for obtaining a campus parking permit? — run 2

- Best distance: 0.5208 (passed the gate)
- Sources retrieved: admin_add_drop_deadline.txt, admin_parking_permits.txt, advising_registration.txt, transit_shuttle.txt, transit_walking.txt

```
Student permits for the west lots go on sale in August and sell out in about three days, while the east lot never sells out because it is a 12-minute walk. There is no waitlist for permits. 

Source: admin_parking_permits.txt
```

### What is the deadline or rule for obtaining a campus parking permit? — run 3

- Best distance: 0.5208 (passed the gate)
- Sources retrieved: admin_add_drop_deadline.txt, admin_parking_permits.txt, advising_registration.txt, transit_shuttle.txt, transit_walking.txt

```
Student permits for the west lots go on sale in August and sell out in about three days, while the east lot never sells out. There is no waitlist available for permits. 

Source: `admin_parking_permits.txt`
```

### What specific requirement does the advising registration guide give students? — run 1

- Best distance: 0.6177 (refused by the gate)
- Sources retrieved: admin_add_drop_deadline.txt, admin_graduation_requirements.txt, admin_pass_fail_option.txt, advising_registration.txt, course_engl_205_exams.txt

```
I don't have enough information about that.
```

### What specific requirement does the advising registration guide give students? — run 2

- Best distance: 0.6177 (refused by the gate)
- Sources retrieved: admin_add_drop_deadline.txt, admin_graduation_requirements.txt, admin_pass_fail_option.txt, advising_registration.txt, course_engl_205_exams.txt

```
I don't have enough information about that.
```

### What specific requirement does the advising registration guide give students? — run 3

- Best distance: 0.6177 (refused by the gate)
- Sources retrieved: admin_add_drop_deadline.txt, admin_graduation_requirements.txt, admin_pass_fail_option.txt, advising_registration.txt, course_engl_205_exams.txt

```
I don't have enough information about that.
```

### What policy or feature of Tamsin Court is described in the housing document? — run 1

- Best distance: 0.4780 (passed the gate)
- Sources retrieved: housing_fenwick_court.txt, housing_fenwick_court_noise.txt, housing_tamsin_court.txt, housing_tamsin_court_laundry.txt, housing_tamsin_court_noise.txt

```
Based on the provided documents, Tamsin Court features studio apartments with private kitchens and bathrooms, concrete floors between units for structural quietness, and in-unit washer-dryer laundry machines. 

(Source: `housing_tamsin_court.txt`, `housing_tamsin_court_noise.txt`, and `housing_tamsin_court_laundry.txt`)
```

### What policy or feature of Tamsin Court is described in the housing document? — run 2

- Best distance: 0.4780 (passed the gate)
- Sources retrieved: housing_fenwick_court.txt, housing_fenwick_court_noise.txt, housing_tamsin_court.txt, housing_tamsin_court_laundry.txt, housing_tamsin_court_noise.txt

```
Tamsin Court features studio apartments with private kitchens and bathrooms, concrete floors between units for structural quietness, and in-unit washer-dryers. It is also the most expensive housing tier and the most independent option on campus with full kitchens. 

(Source: housing_tamsin_court.txt)
```

### What policy or feature of Tamsin Court is described in the housing document? — run 3

- Best distance: 0.4780 (passed the gate)
- Sources retrieved: housing_fenwick_court.txt, housing_fenwick_court_noise.txt, housing_tamsin_court.txt, housing_tamsin_court_laundry.txt, housing_tamsin_court_noise.txt

```
Based on the provided documents, Tamsin Court features studio apartments with private kitchens and bathrooms, concrete floors between units for structural quietness, and in-unit washer-dryers. 

Sources:
- `housing_tamsin_court.txt`
- `housing_tamsin_court_noise.txt`
- `housing_tamsin_court_laundry.txt`
```


## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 |  |  |  |
| 2 |  |  |  |
| 3 |  |  |  |
| 4 |  |  |  |
| 5 |  |  |  |

## Diagnoses

<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough — you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading → chunking → embedding → retrieval → generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->

## The Improvement

**What I changed:**

**Why I picked it:**

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

**Did it help?**

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
