TODOs:
- [*] Read ResNet Research paper
- [*] Resnet implementation is remaining
- [*] Transformerlens review
- [*] text2image interp
- [*] blog- https://medium.com/p/c685a9bcdb12?postPublishedType=initial

# Week 1 — Foundations: Vision Representations and Mechanistic Interpretability with TransformerLens

### Task 1 — Read the ResNet Paper

Read [Deep Residual Learning for Image Recognition](https://arxiv.org/abs/1512.03385) (He et al., 2015). Read it fully, focusing heavily on information routing and gradient flow:

- Why does training accuracy degrade on very deep plain networks? Understand the degradation problem.
- What does a residual block actually compute? Write out the equation by hand.
- How does the identity shortcut connection act as a highway for gradient flow, and how does this relate to the concept of the "residual stream" in Transformer architectures?

### Task 2 — Implement ResNet-18 from Scratch

Implement ResNet-18 in PyTorch **without using `torchvision.models`**. You must write every layer yourself: the initial conv stem, the residual blocks, the downsampling shortcuts, and the classification head. 

Train your model on [CIFAR-10](https://www.cs.toronto.edu/~kriz/cifar.html)[cite: 1]. You can use `torchvision.datasets.CIFAR10` for data loading with standard augmentations (random horizontal flip, random crop with padding=4, normalize).

- **Expected Target:** You should achieve above 90% test accuracy. If it is well below that, treat it as an interpretability/debugging exercise to trace where information is dropping.
- **Deliverable:** Plot and save your training loss and test accuracy curves[cite: 1]. Write your own training loop rather than copy-pasting[cite: 1].

### Task 3 — Back to TransformerLens
We have already had some experience with the TransformerLens Library, you will explore it a bit more throughout week 1.

Go through the reference tutorial we have been using: [TransformerLens Sample Tutorial](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwi_4MX75PmUAxXHWHADHXLjJMAQFnoECBoQAQ&url=https%3A%2F%2Fcolab.research.google.com%2Fdrive%2F19mFGzQL_PX-PU_8FOSygeS9CZbuW75Qe&usg=AOvVaw2mLN8mjsYP5xfgZkDMaS3v&opi=89978449).

Just like last time, you are expected to understand how Hooks are used to cache internal activations, intercept tensors during a forward pass, and perform basic ablation interventions.

### Task 4 — Activation Analysis of a Text-to-Image Prompt Generator
Now, apply your TransformerLens skills to a model that bridges textual generation with visual concepts. You will analyze succinctly/text2image-prompt-generator [model](https://huggingface.co/succinctly/text2image-prompt-generator). It is supported by transformerlens [library](https://transformerlensorg.github.io/TransformerLens/generated/transformer_bridge_models.html?q=image). We had handled GPT2 last time, this text to image model actually uses gpt2 as base.

Load this model into a HookedTransformer instance.

The Experiment: Feed it a simple prompt (e.g., "A cinematic shot of a red car") versus a control prompt ("A cinematic shot of a blue car").

Mechanistic Probing: Use activation caching to isolate the specific attention heads or residual stream dimensions responsible for tracking and mutating visual attributes (like color, texture, or style modifiers) across token positions. Locate where this "visual metadata" is structured in the late-stage residual stream before generation.

### Task 5 — Write and Publish a Blog Post on Your TransformerLens Analysis
Write a clear, straightforward technical blog post summarizing your findings from Task 5. It does not need to be fancy or overly dense—aim for a clean, accessible explanation of how the prompt generator handles textual modifiers. Your post must cover:

The Goal: A brief description of what succinctly/text2image-prompt-generator does and why we are looking under the hood.

The Setup: A short code snippet showing how you loaded the model with TransformerLens and added hooks to cache activations.

Your Observations: An explanation of what changed in the internal layers when swapping a key visual token (like changing the color or subject of the prompt). Mention which specific layers or attention patterns seemed most responsive to the change.

Visualizations are encouraged.

Publishing: If you do not have a blog, set one up using the GitHub Pages resources shared earlier[cite: 1]. Share the live link at the Week 1 review[cite: 1].
