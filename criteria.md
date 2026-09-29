# Acceptance Criteria - The Unofficial Guide

Five criteria that say what "working" means for this system, written in unit 1
before any results existed.

---

## 1. Retrieved chunks contain the answer

For at least 4 of my 5 test questions, the retrieved chunks include one that
contains the answer.

**Why this target:**
I picked 4 of 5 because my campus life documents are short and usually keep the
answer in one sentence, so most answers should be retrievable. I did not pick 5
of 5 because a few questions could match more than one similar campus topic,
especially dining or housing.

---

## 2. Every answer names a source

Every answer the system produces names at least one source document.

**Why this target:**
I picked every answer because this system is only useful if a reader can check
where the information came from. The corpus has clear source document names, so
missing a source should mean the answer format or generation step needs fixing.

---

## 3. The relevance gate stops out-of-corpus questions

When I ask a question my documents clearly don't cover, the relevance gate
stops it and the system returns "I don't have enough information about that" in
at least 4 of 5 tries.

**Why this target:**
I picked 4 of 5 because the out-of-scope questions are from totally different
subjects, so the gate should usually reject them. I did not pick 5 of 5 because
one unrelated question might still share common school-like words with the
campus documents and get through.

---

## 4. Chunk size preserves complete thoughts

For at least 4 of 5 sampled retrieved chunks, the chunk includes a complete
campus-life post or a complete paragraph with no sentence cut off at the start
or end.

**Why this target:**
I picked 4 of 5 because the campus life corpus is made of short posts, so a good
chunk should usually preserve the full thought. I did not pick 5 of 5 because a
follow-up post or longer housing note may still split awkwardly while remaining
usable.

---

## 5. Source citation matches the evidence

For at least 4 of my 5 test questions, the answer's named source document is one
of the retrieved source documents that contains the `expects` phrase from
`questions.py`.

**Why this target:**
I care that the citation is not just present but connected to the actual answer.
I picked 4 of 5 because most of my expected phrases are exact facts in one
document, but one answer could combine similar documents and cite the less direct
one.
