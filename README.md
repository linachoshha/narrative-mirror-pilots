# Narrative Mirror: pilot experiments

Exploratory experiments behind Narrative Mirror, a process-level diagnostic for language-model comprehension.
The project is supported by an Israel Innovation Authority Tnufa feasibility grant (2026).

## Question

Can signals from inside a language model show where it failed to hold the information that matters in a text,
especially meaning that is implied rather than stated, before the failure shows up in its output?

## What I tried

- **Models:** GPT-2, DistilBERT and two sentence encoders for method checks; Qwen2.5-1.5B for divergence,
  summaries and question answering.
- **Texts:** two passages from my own memoir (flower pots, 15 sentences; pomelo / Taqwa, 35), a short medical
  record (6) and a loan-contract excerpt (6).
- **Human reference:** sentence-level salience scores that I assigned as the author. There are no reader data yet.
- **Divergence:** the gap between a model trajectory built from hidden states and the author salience, per sentence.
  I then checked the model's summaries and its answers to questions that each depend on one sentence.

## What I found

- In the two narrative texts, the model's wrong answers fell on the sentences with the highest divergence. Asked
  why the flowers were neglected, it answered "dry conditions", missing that the sisters had just lost their
  mother. Claude and GPT-4, acting as judges, rated its summary acceptable.
- In the medical record, the summary invented a causal link ("…hopelessness and insomnia, *despite* being on
  lisinopril").
- In the explicit legal text, all answers were correct and divergence showed no pattern.
- The correlation between divergence and summary coverage was unstable across texts (0.65, 0.19, −0.45).
- In GPT-2, raw similarity between sentence states was about 0.99 for everything: geometry, not meaning. After
  correcting for it, the effect was below my pre-set threshold, so inconclusive.

Full numbers are in [`results.md`](results.md).

## Limitations

Author-only salience; very small n; one run per text with greedy decoding. The Qwen trajectory encodes sentences
without context and largely reflects position; the divergence measure does not separate under- from over-weighting;
the coverage metric mislabeled at least one preserved sentence as dropped.

## Next

A contextual measure (the model's state before and after each sentence, keeping the sign of the gap); controlled
texts where meaning is implied or becomes significant only later, with explicit versions as a contrast;
hand-checked failure labels; reader annotations; larger open models.

## Files

| File | What it is |
|---|---|
| `01_gpt2_pomelo_method_checks.ipynb` | Method checks on the pomelo story: geometry, centering, baselines, lexical control, control story. Original outputs. |
| `02_qwen_summary_qa_pilot.ipynb` | The Qwen pipeline as originally run. Saved outputs are from the last text run (medical record). |
| `03_qwen_all_texts_clean.ipynb` | Clean re-implementation of notebook 02 that runs all four texts in one pass and saves a results table. Same method and settings. |
| `results.md` | Results for all texts, with limitations. |

Small open models were used because the method needs access to hidden states. Scaling to larger models is a next step.
