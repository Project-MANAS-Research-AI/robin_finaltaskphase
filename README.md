## TODOS:
- [ ] Read the foundational & cutting-edge Vision Mech Int literature
- [ ] Implement a clean, modular Vision Transformer (ViT) stem and Block
- [ ] Train on CIFAR-10 with a hook-ready, completely transparent architecture
- [ ] Map out a visual circuit or perform activation patching on your own weights

# Week 1 — Foundations: Vision Transformers, Superposition, and Visual Circuit Discovery

### Task 1 — Read the ViT Paper (Through a Mech Int Lens)

Read [An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale](https://arxiv.org/abs/2010.11929) (Dosovitskiy et al., 2020)[cite: 1]. Since you already understand standard text Transformers, read this strictly to analyze the structural changes enforced by spatial patching. Focus on:

- **The Patch Stem:** How is the linear projection of flattened patches mathematically distinct from text token embeddings? How does the lack of inductive bias (compared to CNNs) affect early-layer representations?
- **Positional Embeddings:** Look closely at the learned 1D position embeddings. How do they preserve 2D spatial coordinates? 
- **The [CLS] Token:** How does the routing of information from patch tokens into the `[CLS]` token differ from how causal models route information to the final token?

### Task 2 — Implement a "Hook-Ready" ViT from Scratch

Implement a minimal, working Vision Transformer (ViT-Toy) in PyTorch **without using any external modeling libraries**. 
* **Design Rule:** Your implementation must be built for easy interpretability. Every module (Patch Embedding, Attention Head, MLP block) must allow easy extraction of internal activations. Explicitly store or design hooks for the input and output of the residual stream at every block.
* **Training:** Train your model from scratch on [CIFAR-10](https://www.cs.toronto.edu/~kriz/cifar.html) using a simple classification loop[cite: 1]. 
* **Deliverable:** Get your implementation to converge smoothly, plot your training/loss curves[cite: 1], and verify that your internal dictionary of activation shapes is well-indexed for patching.

### Task 3 — Read Recent Frontiers in Vision Mech Int

To upgrade from language-based Mech Int to the unique structural phenomena of vision, read these breakthrough concepts:
1. **Visual Circuit Discovery:** Read *Seeing Through Circuits: Faithful Mechanistic Interpretability for Vision Transformers (2026)*. Understand how edge-based circuit discovery works in ViTs to locate task-specific computational subgraphs.
2. **Concept Geometry & Superposition:** Review recent 2025/2026 work on *Minkowski Representation Geometry* in vision foundation models (e.g., DINO). Focus on how spatial tokens form convex mixtures of archetypes rather than purely sparse text-like representations.

**Focus Questions to Answer:**
* What makes polysemanticity and superposition in vision channels fundamentally different from language models?
* How do "induction heads" translate to vision? Do spatial attention heads behave more like translation invariant operators or semantic feature aggregators?

### Task 4 — Write and Publish a Technical Post: "The Spatial Transformer vs. The Causal LLM"

Write an advanced technical blog post mapping out how Mech Int primitives shift when moving from text sequence models to vision models. Your post must cover:
* **Attention Layouts:** Bidirectional/Global attention in ViTs vs. Causal masking in LLMs, and why this complicates direct circuit tracing.
* **The Token Space:** The mathematical implications of treating spatial pixel coordinates as independent semantic vectors.
* **Visual Representation:** Include an original architectural schema or mathematical formulation comparing how features are packed into the residual stream in vision vs. language.

**Publishing:** Host this on your GitHub Pages blog and share the link for the Week 1 review[cite: 1].

### Task 5 — Activation Patching & Toy Circuit Discovery

Instead of using passive visualization tools like Attention Rollout[cite: 1], you will perform an active causal intervention on the weights of the ViT model you trained in Task 2.

* Select two highly distinct classes from CIFAR-10 (e.g., *Automobile* vs. *Bird*).
* **Intervention:** Run activation patching (causal swapping) across individual attention heads or MLP layers to determine where the "background features" separate from the "object features."
* **Log the Evidence:** Find at least one structural component (a specific attention head or MLP channel) that, when ablated, completely flips the classification metric without destroying the rest of the representation. Save these intervention graphs.

---

## Week 1 review — what to bring

- **Codebase Link:** Your custom, hook-ready ViT implementation on GitHub.
- **Convergence Metrics:** Loss and training curves proving your scratch ViT successfully extracts visual features from CIFAR-10[cite: 1].
- **Blog Post:** A link to your published deep-dive on Vision Mechanistic Interpretability[cite: 1].
- **Causal Intervention Report:** Your activation patching diagrams detailing the exact blocks or heads responsible for separating conflicting visual features.
- **Two Advanced Questions:** Open conceptual challenges regarding scaling visual circuit discovery or training Vision Sparse Autoencoders (SAEs).
