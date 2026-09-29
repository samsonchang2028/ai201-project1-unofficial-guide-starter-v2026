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

**Question:**

**Answer:**

```text
```

**My relevance cutoff:**

| Question | In corpus? | Best distance |
|---|---|---|
|  |  |  |

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

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. Chunk size preserves complete thoughts | 4 of 5 |  |  |  |  |
| 5. Source citation matches the evidence | 4 of 5 |  |  |  |  |

## Verdicts

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 |  |  |  |
| 2 |  |  |  |
| 3 |  |  |  |
| 4 |  |  |  |
| 5 |  |  |  |

## Diagnoses

## The Improvement

**What I changed:**

**Why I picked it:**

### Run Log - After

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. Chunk size preserves complete thoughts | 4 of 5 |  |  |  |  |
| 5. Source citation matches the evidence | 4 of 5 |  |  |  |  |

**Did it help?**

## What's Still Broken

## What I'd Do Differently
