# Week 1 — Foundations: Deep CNNs, Attention, and Vision Transformers

### Task 1 — Read the ResNet paper

Read [Deep Residual Learning for Image Recognition](https://arxiv.org/abs/1512.03385) (He et al., 2015). Read it fully, not just the abstract. Pay attention to:

- Why does training accuracy degrade on very deep plain networks? This is called the degradation problem. Make sure you understand it before moving on.
- What does a residual block actually compute? Write out the equation by hand.
- How does identity shortcut connection help with gradient flow?

Do not skip Section 4 (experiments). Understanding the ablation studies is part of reading a paper properly.

### Task 2 — Implement ResNet-18 from scratch

Implement ResNet-18 in PyTorch **without using `torchvision.models` i.e you cannot just directly import the resnet18 model**. You should write every layer yourself: the initial conv stem, the residual blocks, the downsampling shortcuts, and the classification head. You are absolutely free to refer to online resources, but be mindful of the design decisions and try to understand them rather than just copy-pasting.



Train on [CIFAR-10](https://www.cs.toronto.edu/~kriz/cifar.html). You can use `torchvision.datasets.CIFAR10` for data loading — that is fine. Use standard augmentation (random horizontal flip, random crop with padding=4, normalize).

Expected result: you should be able to get above 90% test accuracy with this setup. If you are well below that, something is wrong with your implementation — debug it.

Plot your training loss and test accuracy curves and save them. You will need these for the Week 1 review.

Useful reference for the training setup: [PyTorch CIFAR-10 tutorial](https://pytorch.org/tutorials/beginner/blitz/cifar10_tutorial.html), but write your own training loop rather than copy-pasting.

### Task 3 — Read "Attention Is All You Need"

Read [Attention Is All You Need](https://arxiv.org/abs/1706.03762) (Vaswani et al., 2017). This paper introduces the Transformer architecture for NLP. You are not implementing it — you are understanding it.

Focus areas:
- Section 3.2: Scaled dot-product attention. Understand the matrix operations. What are Q, K, V and where do they come from?
- Section 3.3: Multi-head attention. Why multiple heads rather than one big one?
- Section 3.5: Positional encoding. The model has no recurrence — so how does it know token order?

You do not need to deeply understand the encoder-decoder structure for NLP. What you need is a solid mental model of self-attention as a mechanism, because that is what gets imported into vision.

### Task 4 — Write and publish a blog post on attention

Write a blog post explaining the attention mechanism to someone who has taken a basic ML course but has not read the paper. Your post should cover:

- The intuition behind Q, K, V (not just the math)
- The scaled dot-product attention formula with explanation of why you scale by sqrt(d_k)
- Multi-head attention — what does it let the model do that single-head does not?
- At least one diagram or figure you made yourself (hand-drawn and photographed is fine)

**Publishing:** If you do not have a blog, set one up using the github pages resources shared earlier.

This post is a deliverable. Share the link at the Week 1 review. It should be readable and well-structured, not a dump of bullet points.

### Task 5 — Read the ViT paper

Read [An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale](https://arxiv.org/abs/2010.11929) (Dosovitskiy et al., 2020). This is the paper that brought the Transformer directly to image classification.

Focus areas:
- Section 3: How is an image turned into a sequence of patches? How does this relate to word tokens in NLP?
- Positional embeddings for patches — how are they learned and what do they look like?
- Section 3.1: The [CLS] token — what does it do and why does this design choice come from BERT?
- Section 4: The main finding — ViT underperforms CNNs on small datasets but beats them at scale. Why?

You do not need to implement ViT this week. Understanding the architecture and its trade-offs is the goal.

### Task 6 — Visualize attention maps with timm

Install [timm](https://github.com/huggingface/pytorch-image-models) and use a pretrained ViT-B/16 to visualize attention maps on a few images of your choice.

```bash
pip install timm
```

The key thing to extract is the attention weights from the last transformer block, specifically the attention from the [CLS] token to all patch tokens — this tells you which parts of the image the model is focusing on when making a classification decision. This technique is called [Attention Rollout](https://arxiv.org/abs/2005.00928) and there are reference implementations available.

A good starting point for the code: [jeonsworld/ViT-pytorch](https://github.com/jeonsworld/ViT-pytorch) has attention visualization utilities.

Run this on at least 5 images — pick images where the subject is clear (a dog, a car, a person) and check whether the attention concentrates on the object or scatters across the background. Save your visualizations.

Bring these visualizations to the Week 1 review and be ready to explain what you see.

---

## Week 1 review — what to bring

- Your ResNet-18 implementation on GitHub (link in your repo's README)
- Training and accuracy curves from CIFAR-10
- Your published blog post on attention (link)
- Attention map visualizations for at least 5 images
- At least two questions you could not answer by reading alone

