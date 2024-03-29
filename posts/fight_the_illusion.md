---
title: '6 Ways to Fight the Interpretability Illusion'
date: 11/30/2023
author:
  - name: 
      given: Michael
      family: Sklar
    email: michaelbsklar@gmail.com
bibliography: biblio.bib
description: Notes on using optimization and causal models for interpretability.
format:
  html: default
draft: false
---

<!-----

Conversion time: 0.504 seconds.

Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β35
* Tue Nov 28 2023 15:52:28 GMT-0800 (PST)
* Source doc: 6 ways to fight the Interpretability illusion
----->


Recommended pre-reading: 

- Geiger et al.'s [DAS](https://arxiv.org/abs/2303.02536) and [Boundless DAS](https://arxiv.org/pdf/2305.08809.pdf). 
- [An Interpretability Illusion for Activation Patching of Arbitrary Subspaces](https://www.lesswrong.com/posts/RFtkRXHebkwxygDe2/an-interpretability-illusion-for-activation-patching-of). 
- The corresponding [ICLR paper, “Is This the Subspace You Are Looking For?](https://openreview.net/forum?id=Ebt7JgMHv1)”

------

This post is motivated by Lange, Makelov, and Nanda's LessWrong post [Interpretability Illusion for Activation Patching](https://www.lesswrong.com/posts/RFtkRXHebkwxygDe2/an-interpretability-illusion-for-activation-patching-of) and [ICLR paper](https://openreview.net/forum?id=Ebt7JgMHv1). They study [Geiger et al's DAS](https://arxiv.org/abs/2303.02536) method, which uses optimization to identify an abstracted causal model with a small subset of dimensions in a neural network's residual stream or internal MLP layer. Their results show that DAS can, depending on the situation, turn up both "correct" and "spurious" findings on the train-set. From the investigations in the [ICLR paper](https://openreview.net/forum?id=Ebt7JgMHv1) and conversations with a few researchers, my understanding is these "spurious" directions have not performed well on held-out generalization sets, so in practice it is easy to distinguish the "illusions" from "real effects". But, I am interested in developing even stronger optimize-to-interpret methods. With more powerful optimizers, illusion effects should be even stronger, and competition from spurious signals may make true signals harder to locate in training. So, here are 6 possible ways to fight against the interpretability illusion. Most of them can be tried in combination.

1. **The causal model still holds, and may still be what we want.**: We call it an interpretability _illusion_ because we are failing to describe the model’s normal functioning. But unusual functioning is fine for some goals! Applications include: 
    - Finding latent circuits which might be targetable by optimized non-routine inputs. For example, these circuits might be used in an adversarial attack.
    - "Pinning" a false belief into the model, for testing or alignment training. For example, forcing the model to believe it is not being watched, in order to test deception or escape behavior.

	The key point is that the interpretability illusion is a failure to _describe typical model operation_, but a success for _enacting the causal model_.
2. **Study more detailed causal models with multiple output streams, multiple options for the input variables, or more compositions.** To start, notice that it is obviously good to have more outputs/consequences of the causal mode in the optimization. Why? First, if we have multiple output-measurements at the end of the causal graph, it is harder for a spurious direction to perform well on all of them by chance. Additionally, if an abstract causal model has modular pieces, then there should be exponentially many combinatorial-swap options that we can test. To score well on the optimization's training-loss across all swaps (in the language of [DAS](https://arxiv.org/abs/2303.02536) this is a high "IIA"), the spurious structure would have to be very sophisticated. While Lange et al. show that spurious solutions may arise for searches in 1 direction, it should be less likely to occur for _pairs_ of directions, and less likely yet for full spurious circuits. So, illusion problems may be reduced by scaling up the complexity of the causal model. Some possible issues remain, though:
    - In some cases we may struggle to identify specific directions within a multi-part model; i.e., we might find convincing overall performance for a circuit, but an individual dimension or two could be spurious, and we might be unable to determine exactly which.
    - This approach relies on big, deep, abstract causal models existing inside the networks, with sufficient robustness in their functioning across variable changes. There is some suggestive work on predictable / standardized structures in LLM’s, from investigations like [Feng and Steinhardt (2023](https://arxiv.org/pdf/2310.17191.pdf))’s entity binding case study, the [indirect object identification (IOI)](https://github.com/redwoodresearch/Easy-Transformer/blob/main/README.md) paper, and studies of [recursive tasks](https://arxiv.org/pdf/2305.14699.pdf). However, the consistency/robustness and DAS-discoverability of larger structures in scaled-up models is not yet clear. More case studies in larger models would be valuable.
3. **Measure generalizability, and use it to filter out spurious findings after-the-fact.** This is just common-sense, and researchers are already doing this in several ways. We can construct train/test splits with random sampling, and conclude a found direction is spurious if it does not generalize on the test data; or we could ask how the patched model generalizes out-of-training-distribution following a small perturbation, such as adding extra preceding tokens. Spurious solutions are likely to be sensitive to minor changes, and for many purposes we are primarily interested in causal models that generalize well. As mentioned earlier, the [ICLR ](https://openreviewnet/forum?id=Ebt7JgMHv1) paper’s `spurious’ findings performed sufficiently poorly on generalization sets that they could easily be distinguished from real effects.
4. **Quantify a null distribution.** In the [“Illusion” post](https://www.lesswrong.com/posts/RFtkRXHebkwxygDe2/an-interpretability-illusion-for-activation-patching-of), Lange et al. show that the strength of the spurious signal depends on how many neurons it is allowed to optimize over. So, a very strong signal, taken over a small optimization set, should be more convincing. Thinking as statisticians, we could attempt to construct a null distribution for the spurious signals; this approach could offer evidence that a causal map element is being represented at all. We can do this inference for individual _pieces_ of a larger causal model, with each component having its own uncertainty.
5. **Use unsupervised feature extraction as a first step.** Recent interpretability work with auto-encoders ([Bricken et al. 2023](https://transformer-circuits.pub/2023/monosemantic-features), [Cunningham et al. 2023](https://arxiv.org/abs/2309.08600)) suggests that many of a small transformer’s most important features can be identified. If this technique scales well, it could **_vastly_** reduce the amount of optimization pressure needed to identify the right directions, shrinking the search space and reducing optimistic bias and spurious findings.
6. **Incorporate additional information as a prior / penalty for optimization.** As Lange et al. note in the  [“Illusion” post](https://www.lesswrong.com/posts/RFtkRXHebkwxygDe2/an-interpretability-illusion-for-activation-patching-of), and as described in Section 5 of the [ICLR paper](https://openreview.net/forum?id=Ebt7JgMHv1), it is possible to supply additional evidence that a found direction is faithful or not. In the case study with the IOI task, they argued the direction found by DAS on a residual layer fell within the query subspace of human-identified name mover heads. More generally, if intuitions about faithfulness can be scored with a quantitative metric, then tacking that metric onto the optimization as a penalty should help the optimizer favor correct directions over spurious solutions. Still, using this approach requires answering two difficult questions: what additional evidence to choose, and then how to quantify it? Some rough possibilities:
    - If we know of structures that should be related to the task, such as entity bindings ([Feng and Steinhardt (2023)](https://arxiv.org/pdf/2310.17191.pdf)), we can try to build outwards from them; or if we have a reliable feature dictionary from sparse auto-encoders or "[belief graph](https://arxiv.org/pdf/2111.13654.pdf)" per Hase et al. 2021 which offers advance predictions for how subsequent layers' features may react to a change, we can penalize lack of correlation or causal effects on downstream features.
    - Somehow use the basic structure of the network to quantify which directions are 'dormant.' Although this sounds simple, I am unsure how to do it, given the indeterminacy of what a 'dormant' direction even means (this issue is described in Lange et al.'s lesswrong [post](https://www.lesswrong.com/posts/RFtkRXHebkwxygDe2/an-interpretability-illusion-for-activation-patching-of), Appendix section: the importance of correct model units.)
    - Perhaps next-gen AI will offer accurate "auto-grading", giving a general yet quantitative evaluation of plausibility of found solutions

    Using extra information in this way unfortunately spends its usability for validation. But preventing the optimization from getting stuck on spurious signals may be the higher priority.


------

Thanks to Atticus Geiger, Jing Huang, Zhengxuan Wu, Ben Thompson, Zygimantas Straznickas and others for conversations and feedback on earlier drafts.
