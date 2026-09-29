# The Unofficial Guide

# Unit 1

## What This Does

This project builds a retrieval system for the `campus_life` corpus, a set of
short student posts about campus dining, housing, classes, transportation,
administrative deadlines, and study resources. The system should answer
specific questions by retrieving the short post that contains the relevant fact
and then naming the source document it used. It is meant for factual campus-life
questions such as dining wait times, parking permit rules, housing lottery
details, and library room policies.

## Chunking Strategy

**Chunk size:** up to 650 characters, split only at paragraph or sentence
boundaries when a document is longer than that.
**Overlap:** 0 characters.

The baseline index for `campus_life` made 88 chunks from 88 documents, with an
average length of 317 characters, shortest 178, and longest 549. That showed me
the starter was not actually cutting these posts apart, because the campus-life
documents are already short student posts. I kept that basic shape but replaced
the fixed-window chunker with `chunker.py::split_documents`, which preserves a
whole short post as one chunk and only splits longer documents at natural
paragraph or sentence breaks. I chose no overlap because the chunks are complete
posts rather than sliding windows.

## Sample Chunks

**Chunk 1** - source: `admin_add_drop_deadline.txt#0` - produced by:
`chunker.py::split_documents`

```text
On the add/drop deadline

You can add a course through the end of the second week. Dropping is a longer window through the end of week six but a drop after week two shows as a W on your transcript. Nothing anywhere on the registrar's site says this plainly, and students find out from each other.
```

**Chunk 2** - source: `course_biol_160.txt#0` - produced by:
`chunker.py::split_documents`

```text
BIOL 160 Cell Biology

I lived here my sophomore year. Format is lecture three times a week with a weekly lab. Assessment: four unit tests and a cumulative final. Not curved.

Expect 9 to 11 hours a week, the heaviest first-year course by reputation.

The one piece of advice: the unit tests come fast, roughly every three weeks; falling behind once is very hard to recover from.
```

**Chunk 3** - source: `course_hist_118_workload.txt#0` - produced by:
`chunker.py::split_documents`

```text
Workload for HIST 118 Modern World History

People keep asking so: a lot of reading, about 120 pages a week, but no problem sets. That's real time, not optimistic time.

It's front-loaded: the first month is heavier than the rest, partly because you're learning the format.
```

**Chunk 4** - source: `dining_pellew_dining_hall_followup.txt#0` - produced by:
`chunker.py::split_documents`

```text
Re: Pellew Dining Hall

Adding to what people have said about Pellew Dining Hall. The wait figure of 12 to 18 minutes at peak matches what I've seen. If you're trying to eat between classes, go before 11:45 and it's a different building entirely.

Also worth saying: the furthest hall from anywhere, next to the athletics centre. Nobody tells you this at orientation.
```

**Chunk 5** - source: `housing_innisfree_hall.txt#0` - produced by:
`chunker.py::split_documents`

```text
Innisfree Hall - what it's actually like

Transferred in last year, so take this with a grain of salt. Built 1991, renovated 2022. Rooms are doubles arranged as pairs sharing one bathroom between two rooms.

The good: the shared-bathroom-between-two-rooms arrangement is the best compromise on campus.

The bad: no air conditioning, which matters for the first three weeks of September.

Laundry costs $1.75 wash, $1.75 dry, app-based. On noise: moderate; the building is L-shaped and the short wing is much quieter.
```

## Sample Answer

**Question:** How long is the lunch wait at Kestrel Commons between 12:15 and 1:00?

**Answer:**

```text
The wait time at Kestrel Commons between 12:15 and 1:00 is 20 to 25 minutes.

Source: `dining_kestrel_commons.txt` (also mentioned in `dining_kestrel_commons_followup.txt`).
```

Sources retrieved: `dining_halden_hall_followup.txt`,
`dining_kestrel_commons.txt`, `dining_kestrel_commons_followup.txt`,
`dining_north_kitchen_followup.txt`, `dining_the_ridgeway_cafe_followup.txt`

**My relevance cutoff:** 0.6

My five in-corpus questions had best distances from 0.1797 to 0.3723. The five
out-of-scope questions had best distances from 0.8246 to 0.9340. I kept the
cutoff at 0.6 because it sits in the clear gap between those two groups: all of
my real questions pass, and all of the unrelated questions are refused.

| Question | In corpus? | Best distance |
|---|---|---|
| Which student parking permit sells out quickly, and when does it go on sale? | Yes | 0.3723 |
| How long is the lunch wait at Kestrel Commons between 12:15 and 1:00? | Yes | 0.1797 |
| For juniors and seniors, what decides housing lottery order before the random tie-break? | Yes | 0.2467 |
| How far ahead can students book group study rooms, and how long is each block? | Yes | 0.1893 |
| How many times can students change their meal plan tier, and when must they do it? | Yes | 0.2351 |
| What is the capital of Mongolia? | No | 0.8246 |
| How do I change the oil in a diesel engine? | No | 0.9340 |
| Who won the 1994 World Cup? | No | 0.8859 |
| What is the recommended dosage of ibuprofen for a headache? | No | 0.8442 |
| How do I write a for loop in Rust? | No | 0.8960 |

## How I Used AI

**1.** I asked AI to help turn the assignment instructions into testable
questions and criteria, then I checked the corpus files so the final questions
used facts that actually appear in the documents.

**2.** I asked AI to help compare the starter chunking behavior to the
`campus_life` corpus. I kept the finding that one short post usually works as
one chunk, but changed the code so future splits happen on paragraph or sentence
boundaries instead of fixed character windows.

---

# Unit 2

## Run Log - Before

Raw evidence file: `results/run_2026-09-28_2336_before.md`, produced by
`run_eval.py::main`. Retrieval came from `store.py::search`, chunks came from
`chunker.py::split_documents`, and answers came from
`generate.py::answer_from_chunks`.

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 2. Every answer names a source | 5 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. Chunk size preserves complete thoughts | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 5. Source citation matches the evidence | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |

Evidence from the before run:

```text
Which student parking permit sells out quickly, and when does it go on sale? - run 1
Best distance: 0.3723
Sources retrieved: admin_library_holds.txt, admin_parking_permits.txt,
advising_registration.txt, money_textbooks.txt, transit_shuttle.txt

Student permits for the west lots go on sale in August and sell out in about three days.

Source: admin_parking_permits.txt
```

```text
How long is the lunch wait at Kestrel Commons between 12:15 and 1:00? - run 1
Best distance: 0.1797
Sources retrieved: dining_halden_hall_followup.txt, dining_kestrel_commons.txt,
dining_kestrel_commons_followup.txt, dining_north_kitchen_followup.txt,
dining_the_ridgeway_cafe_followup.txt

The lunch wait at Kestrel Commons between 12:15 and 1:00 is 20 to 25 minutes.

Source: dining_kestrel_commons.txt
```

```text
Out-of-scope gate check, cutoff 0.6:
What is the capital of Mongolia? - refused, best distance 0.825
How do I change the oil in a diesel engine? - refused, best distance 0.934
Who won the 1994 World Cup? - refused, best distance 0.886
What is the recommended dosage of ibuprofen for a headache? - refused, best distance 0.844
How do I write a for loop in Rust? - refused, best distance 0.896
Gate refused 5 of 5.
```

## Verdicts

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 | Retrieved chunks contain the answer | MET | For each of the five questions, the retrieved sources included a document with the expected answer phrase in all three runs. |
| 2 | Every answer names a source | MET | Every generated answer named at least one source document in all three runs. |
| 3 | The relevance gate stops out-of-corpus questions | MET | The gate refused all five out-of-scope questions, so the same 5/5 result applies to all three deterministic gate columns. |
| 4 | Chunk size preserves complete thoughts | MET | The sampled retrieved chunks were whole short campus-life posts or complete paragraphs; none of the five sampled chunks had sentences cut off at the start or end. |
| 5 | Source citation matches the evidence | MET | For all five questions, the named source was one of the retrieved documents and contained the `expects` phrase from `questions.py`. |

## Diagnoses

I did not miss any of my five criteria in the before run. That means there is
no failure to trace to loading, chunking, embedding, retrieval, or generation.

The honest diagnosis is that my criteria were useful but safe for this corpus.
The documents are short, the answer phrases are exact, and the distance gap
between in-corpus and out-of-scope questions is large. If I were tightening a
criterion, I would make criterion 5 stricter: every answer must end with a
separate `Source: filename.txt` line, not just mention a source somewhere.

## The Improvement

**What I changed:** I tightened the grounding prompt in
`generate.py::GROUNDING_INSTRUCTION` and `generate.py::build_prompt` so every
answer must end with a separate line in this exact format:
`Source: filename.txt`.

**Why I picked it:** Before the change, the answers all named sources, but the
format varied: some used a separate `Source:` line, some used parentheses, and
some added extra prose such as "also mentioned in." Since source checking is one
of my criteria, a stricter and more consistent citation format is the most
useful small generation-stage improvement.

### Run Log - After

Raw evidence file: `results/run_2026-09-28_2337_after.md`, produced by
`run_eval.py::main`.

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 2. Every answer names a source | 5 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. Chunk size preserves complete thoughts | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 5. Source citation matches the evidence | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |

Evidence from the after run:

```text
Which student parking permit sells out quickly, and when does it go on sale? - run 1

Student permits for the west lots go on sale in August and sell out in about three days.

Source: admin_parking_permits.txt
```

```text
For juniors and seniors, what decides housing lottery order before the random tie-break? - run 1

For juniors and seniors, accumulated credit hours decide the housing lottery order before any random tie-break is applied.

Source: admin_housing_lottery.txt
```

```text
How many times can students change their meal plan tier, and when must they do it? - run 1

Students can change their meal plan tier once, and they must do it within the first ten days of the semester.

Source: admin_meal_plan_changes.txt
```

**Did it help?**

Yes, but only on formatting. The before run already met all five criteria, so
the improvement did not raise any criterion score. It did make the answer format
more consistent: after the change, the sample answers use a clean separate
`Source: filename.txt` line, which makes criterion 2 and criterion 5 easier to
check.

## What's Still Broken

Nothing is broken against my current five criteria after the improvement.
However, the test set is probably too friendly: each question asks for a fact
that appears almost exactly in one short document. If I had more time, I would
add harder questions where the answer is split across two related documents or
where several dining or housing documents share similar wording.

## What I'd Do Differently

I would rewrite criterion 5 to be stricter. Instead of "the answer's named
source document is one of the retrieved source documents that contains the
`expects` phrase," I would write: "For all 5 test questions, the final line of
the answer is exactly `Source: filename.txt`, and that file is one of the
retrieved documents containing the `expects` phrase." That would have made the
before run reveal the inconsistent citation formatting more clearly.

I also used AI in this unit to help organize the run logs into criterion-level
counts and to pressure-test whether the improvement matched the diagnosis. I
kept the actual verdicts tied to the saved `results/` files rather than to the
AI's opinion.
