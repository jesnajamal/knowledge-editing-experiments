Knowledge-editing experiments

First steps in reproducing knowledge-editing methods for language models and measuring where they succeed and fail. This is preliminary work for a proposed study, Reliable Knowledge Editing: Diagnosing Edit Success and Unintended Behavioural Changes.

Experiment 001: a single ROME edit on GPT-2 XL

Notebook: rome_single_edit_gpt2xl.ipynb (runs on a free Google Colab T4 GPU)

Edit: "The Eiffel Tower is located in the city of" → Rome (originally Paris), applied with ROME through the EasyEdit library. ROME changes one weight matrix (layer 17, mlp.c_proj).

Result. Next-token probabilities before → after the edit:

Test	Prompt	P(Paris)	P(Rome)	Generated text after
Edited prompt	The Eiffel Tower is located in the city of	0.934 → 0.000	0.000 → 1.000	Rome
Paraphrase	The Eiffel Tower can be found in the city of	0.748 → 0.000	0.001 → 1.000	Rome
Nearby fact	The Louvre is located in the city of	0.823 → 0.454	0.003 → 0.259	Paris (unchanged)
Unrelated fact	The Statue of Liberty is located in the city of	0.002 → 0.002	0.000 → 0.045	New York (unchanged)

What this shows. The edit succeeded and transferred to a paraphrase. The generated text for the nearby and unrelated facts did not change, but their probabilities did: the model became much less certain that the Louvre is in Paris. Evaluating by generated text alone would have missed this side effect.

Limitations.

This is one edit, so it illustrates the effect and supports no general claim.
EasyEdit's GPT-2 XL configuration sets mom2_adjustment: false, which skips the covariance statistics used in the original ROME method. Some of the leakage may come from this simplification; testing that is a planned next step.
Planned next steps
Repeat with mom2_adjustment enabled and compare locality.
Run a batch of CounterFact edits and report edit success, paraphrase generalization and locality.
Add a small multi-hop check.
References
Meng et al., Locating and Editing Factual Associations in GPT, NeurIPS 2022.
Meng et al., Mass-Editing Memory in a Transformer, ICLR 2023.
Zhang et al., Locate-then-edit for Multi-hop Factual Recall under Knowledge Editing, ICML 2025.
