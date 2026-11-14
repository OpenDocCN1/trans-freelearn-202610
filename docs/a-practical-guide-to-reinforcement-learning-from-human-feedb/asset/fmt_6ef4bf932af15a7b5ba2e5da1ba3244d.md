
## Direct Alignment Techniques

### Stepwise DPO-like Training

Used in settings like chain-of-thought prompting, where correct intermediate steps are rewarded, rather than just final answers. See more here: sDPO: Don't Use Your Data All at Once: https://arxiv.org/abs/2403.19270.

### Token-level Preference Supervision

Still experimental, this approach aligns generation word-by-word, often requiring dense human or model-generated feedback. Token-level Direct Preference Optimization: https://arxiv.org/pdf/2404.11999.

These approaches explore new preference formats, non-contrastive objectives, and more flexible or efficient training frameworks. While inspired by DPO's direct alignment philosophy, they represent conceptual departures that expand what preference-based training can achieve. Together, DPO and its variants—alongside these newer, structurally distinct methods—form a rich and evolving space of direct alignment techniques. Understanding how these methods relate and differ is essential for selecting the right approach depending on available feedback, alignment goals, and resource constraints.

## Limitations and Practical Considerations

Despite its practical advantages, DPO has several important limitations. Its performance relies heavily on the quality and consistency of preference data, since it does not use explicit reward shaping to compensate for noisy or sparse signals. In tasks requiring structured credit assignments, such as multi-step reasoning, long-horizon decision-making, or fine-grained trajectory control, RLHF with explicit reward models may outperform DPO. This is because reward-based methods can provide continuous scalar feedback at each stage, allowing models to learn more nuanced or hierarchical behaviors effectively. Additionally, DPO's reliance on pairwise comparisons can reduce data efficiency, especially when annotations are limited or costly. Recognizing these constraints is essential when selecting an appropriate alignment strategy.

| Criteria | DPO | RLHF with Reward Models |
| --- | --- | --- |
| Data Type | High-quality pairwise preference data | Preference data + scalar reward signals |
| Feedback Structure | Pairwise comparisons, simpler annotations | Detailed scalar or trajectory-level rewards |
| Task Complexity | Single-turn, local, short-horizon tasks | Multi-step, long-horizon, hierarchical tasks |
| Compute Implementation | Simpler pipeline, no explicit reward model | More complex, requires reward model training |
| Noise Sensitivity | Sensitive to preference quality | Can compensate with reward shaping |
| Sample Efficiency | Less efficient if data is scarce | More flexible with dense feedback |
| Alignment Control | Conservative updates via reference model | Fine-grained shaping via custom reward |

## Summary

This chapter presented DPO as a scalable, reinforcement-free method for aligning language models directly from human preference data. We began by motivating DPO in contrast to traditional RLHF pipelines, highlighting its elimination of the reward modeling stage and its use of a contrastive log-likelihood objective over preferred and dispreferred responses. We examined how DPO models the probability of preference under a Boltzmann-rational assumption and incorporates a reference policy to regularize updates via implicit KL divergence. The chapter then detailed the DPO pipeline, covering preference data collection, log-likelihood-based training objectives, and the intuition behind the DPO loss. We walked through a complete implementation of DPO, from preprocessing triplet data to configuring training with a reference model and tuning hyperparameters like temperature and batch size. We then surveyed a set of DPO variants that preserve its core structure while modifying loss functions, regularization strategies, or optimization dynamics to improve generalization, reduce overfitting, or support multi-objective alignment. We also distinguished these from conceptually distinct direct alignment methods—such as listwise imitation, unpaired preference optimization, and fine-grained feedback models—that reflect the field's broader shift toward more flexible, preference-driven alignment strategies. Followed by a brief discussion about limitations of DPO and when RLHF might still be a good alternative. Together, these developments position DPO not only as a practical alternative to RLHF, but as a foundational template for future alignment methods grounded in direct optimization from human preferences. While DPO and its extensions offer powerful tools for training alignment-aware models, the question of how well these models or even RLHF reflect human preferences remains open until evaluated rigorously. In the next chapter, we turn to the topic of model evaluation, exploring both automatic metrics and human-centered approaches for assessing alignment quality, generalization, and behavioral fidelity.

## References

*   [https://github.com/huggingface/peft](https://github.com/huggingface/peft)
*   [https://huggingface.co/docs/peft/main/en/index](https://huggingface.co/docs/peft/main/en/index)
*   [https://huggingface.co/docs/trl/sft_trainer](https://huggingface.co/docs/trl/sft_trainer)
*   ORPO: Monolithic Preference Optimization without Reference Model [https://arxiv.org/pdf/2403.07691](https://arxiv.org/pdf/2403.07691)
*   SimPO: Simple Preference Optimization with a Reference-Free Reward [https://arxiv.org/abs/2405.14734](https://arxiv.org/abs/2405.14734)
*   Contrastive Preference Optimization: Pushing the Boundaries of LLM Performance in Machine Translation: [https://arxiv.org/pdf/2401.08417](https://arxiv.org/pdf/2401.08417)
*   β-DPO: Direct Preference Optimization with Dynamic β [https://arxiv.org/pdf/2407.08639](https://arxiv.org/pdf/2407.08639)
*   A General Theoretical Paradigm to Understand Learning from Human Preferences [https://arxiv.org/pdf/2310.12036](https://arxiv.org/pdf/2310.12036)
*   Disentangling Length from Quality in Direct Preference Optimization [https://aclanthology.org/2024.findings-acl.297.pdf](https://aclanthology.org/2024.findings-acl.297.pdf)
*   Following Length Constraints in Instructions [https://arxiv.org/html/2406.17744v1](https://arxiv.org/html/2406.17744v1)
*   Beyond One-Preference-Fits-All Alignment: Multi-Objective Direct Preference Optimization: [https://arxiv.org/abs/2310.03708](https://arxiv.org/abs/2310.03708)
*   SafeDPO: A Simple Approach to Direct Preference Optimization with Enhanced Safety [https://openreview.net/forum?id=MoJSnVZ59d](https://openreview.net/forum?id=MoJSnVZ59d)
*   UPO: Unpaired Preference Optimization for Large Language Models [https://arxiv.org/pdf/2310.12036](https://arxiv.org/pdf/2310.12036)
*   RRHF: Rank Responses to Align Language Models with Human Feedback without tears [https://arxiv.org/abs/2304.05302](https://arxiv.org/abs/2304.05302)
*   RAFT: Adapting Language Model to Domain Specific RAG: [https://arxiv.org/pdf/2403.10131](https://arxiv.org/pdf/2403.10131)
*   Self-Play Preference Optimization for Language Model Alignment: [https://arxiv.org/abs/2405.00675](https://arxiv.org/abs/2405.00675)

# Chapter 11: RLHF and Model Evaluations

## Join Our Book Community on Discord

![A qr code with a square in the middle Description automatically generated](media/file0.png)

[https://packt.link/EarlyAccessCommunity](https://packt.link/EarlyAccessCommunity)
