# Pilot results

Exploratory pilots, run before the IIA Tnufa grant period. Small texts, one run each, greedy decoding.
Human salience scores (0–1 per sentence) were assigned by the author. They are not reader data.

## Qwen2.5-1.5B: divergence, summary and Q&A (4 texts)

Divergence = |model trajectory − human salience| per sentence. The model trajectory projects each sentence's
mean-pooled last-layer state, encoded on its own, onto the first-to-last sentence direction (normalized 0–1).

| Text | Sentences | Highest divergence | Lowest divergence | Q&A | Summary | Coverage corr.* |
|---|---|---|---|---|---|---|
| **Flower pots** (memoir) | 15 | "…two young sisters who had just lost their mum" 0.756; "It was replaced, quietly, by something else" 0.734 | "I felt like I was intruding" 0.031; "Compassion" 0.050 | **Wrong** on the highest-divergence sentence: "Why were the flowers neglected?" → "left unwatered and exposed to dry conditions". The 4 questions on lower-divergence sentences were answered correctly ("ashes": right word, symbolic meaning missed). | Kept the conclusion (compassion) and dropped the cause (the mother's death). Claude and GPT-4, acting as judges, rated the summary acceptable. | 0.191 (1-sentence summary: 13/15 sentences scored "dropped", little variance) |
| **Pomelo / Taqwa** (memoir) | 35 | "In the unseen moral lines…" 0.895; "…first real lesson in accountability" 0.822; "…treated like an adult, by an adult" 0.759 | "As a child, I wanted money for one thing only: sweets" 0.031 | **Wrong:** "Why didn't she yell?" → "the child had done nothing wrong". **Shallow:** "Why take the coins?" → "because she saw the child touch the fruit". | "Caught stealing a fruit, learning responsibility": misframes "just to touch", and the transformation ("lost coins but gained compass") is absent. | not recorded |
| **Medical record** | 6 | "…feeling increasingly hopeless and having difficulty sleeping" 0.690; "…blood pressure of 138/88…" 0.700 | "Current medications include lisinopril…" 0.116 | All 3 correct (penicillin/amoxicillin allergy; mental-health concern; BP value). | Kept all content except the BP value, but **invented a causal link**: "…hopelessness and insomnia, *despite* being on lisinopril for hypertension". | 0.650 (n=6; the hopelessness sentence was scored "dropped" although the summary contains it) |
| **Legal contract** | 6 | no meaningful pattern | no meaningful pattern | All correct. | Captured all key clauses. | −0.446 |

\* Pearson r between sentence divergence and a "drop score" from sentence-embedding similarity to the summary
(all-MiniLM-L6-v2; ≥0.55 preserved, 0.35–0.55 partial, <0.35 dropped).

**Observed pattern (exploratory):** in the two narrative texts, failures fell on the highest-divergence sentences,
where meaning is implied rather than stated. In the explicit legal text there were no failures and no pattern.
The coverage correlation was unstable across texts (0.65, 0.19, −0.45).

**Failure types seen:** dropped connection (flower pots), flattened meaning (pomelo), invented connection (medical).

## GPT-2 and sentence encoders: method checks (pomelo story, 11 sentences)

| Check | Result |
|---|---|
| Raw cosine similarity between sentence states (GPT-2, last layer) | about 0.99–1.00 for all pairs, which is geometry (anisotropy) rather than meaning |
| Centered cosine, S1 vs S11, against a baseline of unrelated sentences | z = 1.61 (sentence vectors), z = 2.55 (prefix-conditioned states). Below the pre-set z > 3 threshold, so **inconclusive** |
| Lexical control: S11 with Islamic terms vs neutral wording | Δ = 0.001, no effect |
| Phase separation, memoir (internal moral turn) vs control story (external events), sentence encoders | memoir 0.103 / 0.086 vs control 0.165 / 0.137 (all-mpnet-base-v2 / all-MiniLM-L6-v2). Phases of the internal turn are less separated: a lead to test, one story each |
| Human arc vs model arc (all-mpnet-base-v2, notebook 01 section 10): the author's cumulative "how much meaning has landed" arc vs the model's projection onto the S1→S11 direction | Pearson r = 0.646 (p = 0.032), Spearman ρ = 0.752 (p = 0.008), n = 11. Both arcs run from about 0 to 1 by construction, which inflates the correlation |

Earlier GPT-2 pilot (three memoir scenes, about 60 sentences, single annotator): Pearson r = 0.744 between the
author's salience annotations and the model trajectory. That run is not in these notebooks.

## Limitations

- Salience was annotated by the author alone; there are no reader data yet.
- The Qwen trajectory encodes sentences without context, and the first-to-last projection largely reflects position.
- |trajectory − salience| does not separate under-weighting from over-weighting.
- One run per text, greedy decoding, very small n; no significance testing.
- The coverage metric mislabeled at least one preserved sentence as dropped.

## Next

A contextual measure (the model's state before vs after each sentence, keeping the direction of the gap); more
texts, with implied meaning as the test condition and explicit text as the contrast; reader annotations;
failure labels checked by hand; larger open models.
