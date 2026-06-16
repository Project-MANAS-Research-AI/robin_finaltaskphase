# Week 2 — Circuits and Causal Tracing

## Task 1 — Rediscover the IOI Circuit in GPT-2 Small

### What is this?

"Indirect Object Identification" (IOI) is a task like:

> *"When Mary and John went to the store, John gave a drink to ___"* → the model should predict **Mary**

Researchers at Anthropic reverse-engineered exactly which attention heads in GPT-2 Small do this job — and named them (S-inhibition heads, name mover heads, etc.). Your job is to reproduce that finding yourself using activation patching.

### What you actually do

1. Load GPT-2 Small into TransformerLens as a `HookedTransformer`.
2. Pick 5–10 IOI sentence templates. There's a standard dataset you can generate yourself — the paper has the template format.
3. Run **activation patching**: corrupt the subject token (replace "Mary" with a random name), then restore one head's output at a time and measure how much the correct logit recovers.
4. Plot a (layer × head) heatmap of "logit difference recovered". The name mover heads will jump out visually.
5. Write down in plain English what you think each important head is doing.

You do not need to fully replicate the paper. The goal is to find *something real* and try to find at least one head that really matters and understand why.

### Resources

- The paper: [Wang et al. 2022 — "Interpretability in the Wild"](https://arxiv.org/abs/2211.00593) — read the abstract and Section 3 (the circuit diagram). Skim the rest.
- TransformerLens patching docs: `transformer_lens.patching` module — look at `get_act_patch_attn_head_out_all_pos`
- Neel Nanda's IOI walkthrough notebook (search "Neel Nanda IOI colab" — it's on his GitHub). Use it as a reference, not a copy-paste source.

### Deliverable

A notebook with:
- The patching loop you wrote
- The heatmap
- 3–4 sentences explaining what you found

---

## Task 2 — Causal Tracing on a Factual Recall Prompt

### What is this?

When you ask GPT-2 something like *"The capital of France is"* and it says *"Paris"* — where in the network does that knowledge actually live? This task answers that question by systematically corrupting and restoring activations.

This is the core experiment from the ROME paper (Meng et al. 2022), and it's one of the cleanest demonstrations of what "locating knowledge" means mechanistically.

### What you actually do

1. Pick 10–15 factual prompts with a single unambiguous completion (capital cities, inventor names, etc.).
2. For each prompt:
   - Run the clean forward pass, record the correct token's logit.
   - Run a corrupted forward pass (replace the subject with a random string like "xkqz"). Record corrupted logit.
   - Now, for every (layer, token position) pair: run the corrupted pass but patch in the clean activation at just that one point. Record how much of the correct logit is recovered.
3. Average the recovery scores across your prompts.
4. Plot a heatmap: x-axis = token position, y-axis = layer. You should see a bright region at the subject token in the early-to-mid MLP layers.

The hook you want is `hook_resid_post` at each layer. Iterate over layers and positions in a double loop.

### Resources

- The paper: [Meng et al. 2022 — ROME](https://arxiv.org/abs/2202.05262) — read Section 3 only ("Locating Factual Associations"). Skip the editing part.
- Rome's causal tracing code is open source on GitHub (`rome` repo by kmeng01) — look at `experiments/causal_trace.py` to understand the structure, but implement your own version from scratch.
- TransformerLens hook reference: `utils.get_act_name("resid_post", layer)` gives you the hook name for `hook_resid_post` at any layer.

### Deliverable

A notebook with:
- Your patching loop (should be ~30–40 lines)
- The (layer × token) heatmap averaged across your prompts
- A one-paragraph answer to: *"Where does GPT-2 Small store the subject→fact association, and how confident are you?"*

---

## Notes

- Both tasks use GPT-2 Small (`gpt2`). Load it once with `HookedTransformer.from_pretrained("gpt2")`.
- Colab is your friend in both of them. Kaggle would be interesting but I have not tried it there.
- If something looks wrong (flat heatmap, no signal), the most common cause is patching at the wrong hook point or averaging across token positions when you shouldn't be. Debug one example manually before running the full loop.
- These two tasks connect directly: Task 1 finds *which heads* matter, Task 2 finds *which layers and positions* matter. Together they give you a two-level picture of how a fact flows through the network.
