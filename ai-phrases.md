# AI-Generated Writing Style: Complete Phrase Database

This reference catalogs phrases and patterns strongly associated with AI-generated academic text. Organized by confidence level and category.

---

## Tier 1: Almost Certain AI Markers

These phrases rarely appear in human-written academic papers. Flag and replace immediately.

### Flowery Intensifiers

| AI Phrase | Replacement |
|-----------|-------------|
| delve into / delve deeper into | examine / analyze / investigate |
| in the realm of | in |
| in the landscape of | in / within |
| in the ever-evolving landscape | in the field of / in current |
| a myriad of | many / various / multiple |
| a plethora of | many / several / numerous |
| a multitude of | many / multiple |
| holds immense promise | shows potential / is promising |
| pave(s) the way for | enable(s) / facilitate(s) |
| stands as a testament to | demonstrates / confirms |
| is poised to revolutionize | may improve / could advance |
| game-changer / game-changing | significant improvement |
| cutting-edge (overuse) | recent / advanced / current |
| groundbreaking (overuse) | important / significant |
| unparalleled | superior / best among tested |
| unprecedented | new / first reported |

### Formulaic Transitions

| AI Phrase | Replacement |
|-----------|-------------|
| it is worth noting that | (delete, state the fact directly) |
| it should be noted that | (delete, state the fact directly) |
| it is important to note that | (delete, state the fact directly) |
| importantly | (often deletable; use "Notably," if needed) |
| interestingly | (often deletable; state why it's interesting instead) |
| in this context | (often deletable) |
| in this regard | (often deletable) |
| to this end | (delete or replace with "Therefore,") |
| in light of this | "Given this," / "Based on this," |
| by the same token | similarly / likewise |
| it is well-established that | (delete, state the fact directly) |
| as is well known | (delete, state the fact directly) |
| needless to say | (delete entirely) |

### Verbose Constructions

| AI Phrase | Replacement |
|-----------|-------------|
| plays a crucial/pivotal/vital role in | is important for / contributes to |
| has garnered significant attention | has been widely studied / is an active research area |
| offers a promising avenue for | enables / supports |
| the intricate nature of | the complexity of |
| the nuanced interplay between | the interaction between |
| sheds (new) light on | explains / clarifies / reveals |
| provides valuable insights into | explains / shows |
| opens up new possibilities | enables |
| has attracted widespread attention | has been studied |
| the critical challenges inherent in | the challenges of |
| effectively addressing the challenges | (delete, or state the specific challenge) |

---

## Tier 2: Suspicious When Overused

These phrases are acceptable in moderation (1-2 per paper). Flag when used excessively.

### Adjective/Adverb Overuse Thresholds

| Word | Max Acceptable Count | Alternative |
|------|---------------------|-------------|
| novel | 2 | new / proposed / introduced |
| comprehensive | 3 | thorough / detailed / extensive |
| robust | 3 | reliable / consistent / stable |
| remarkable | 2 | significant / notable |
| exceptional | 2 | strong / high / improved |
| compelling | 2 | strong / convincing |
| notably | 3 | in particular / specifically |
| remarkably | 2 | (delete, use numbers instead) |
| significantly | 5 | by X% / substantially |
| effectively | 3 | (often deletable) |
| innovative | 2 | new / proposed |
| sophisticated | 2 | complex / advanced |
| substantial | 3 | large / considerable |
| paramount | 1 | critical / essential |
| superior | 3 | better / higher / improved |
| pivotal | 1 | important / key |
| holistic | 1 | complete / overall |
| seamlessly | 1 | smoothly / without overhead |

### Pattern Overuse Thresholds

| Pattern | Max Acceptable | Reason |
|---------|---------------|--------|
| not only ... but also | 2 per paper | AI signature construction |
| furthermore ... moreover (same paragraph) | 0 | Pick one |
| on the other hand | 2 per paper | Often unnecessary |
| it is well-established that | 1 per paper | Verbose; just state the fact |
| demonstrates remarkable/exceptional | 1 per paper | Use specific numbers |
| underscores the/their/its | 1 per paper | Vague; be specific |
| this is particularly significant | 1 per paper | Show why with evidence |
| effectively addressing the challenges | 0 | Too vague to be informative |
| validates the (algorithmic) innovation | 0 | Self-congratulatory |
| superior performance and adaptability | 0 | Vague; use numbers |

---

## Tier 3: Structural AI Patterns

Beyond individual phrases, AI text exhibits structural patterns:

### Paragraph-Level Patterns

**Pattern 1: The "Triple Superlative"**

AI version:
> "The results demonstrate remarkable performance, achieving exceptional 
> accuracy and outstanding efficiency across all datasets."

Human version:
> "Method X achieves 0.998 AUC on Dataset Y and processes 1M edges in 12 seconds."

**Pattern 2: The "Challenge-Solution Sandwich"**

AI version:
> "The ever-increasing complexity of X poses significant challenges. 
> To address this critical issue, we propose a novel approach that 
> effectively tackles these challenges."

Human version:
> "Existing methods require O(n^2) storage for n nodes. The proposed method uses 
> sketch data structures to reduce this to O(d * b)."

**Pattern 3: The "Validation Crescendo"**

AI version:
> "These findings not only validate the algorithmic innovation of our 
> approach but also underscore its potential for generalized applications, 
> effectively addressing the critical challenges inherent in contemporary 
> cybersecurity research."

Human version:
> "Both methods outperform baselines on all four datasets. SP-SBEAD 
> achieves the highest AUC on three of four datasets."

**Pattern 4: The "Significance Amplifier"**

AI version:
> "This consistent high-performance pattern across diverse datasets not only 
> validates ... but also underscores their potential for generalized anomaly 
> detection applications."

Human version:
> "The consistent improvement across DARPA, CICIDS2017, CICIDS2019, and 
> TON-IoT (Table 3) suggests the approach generalizes across dataset types."

**Pattern 5: The "Redundant Re-validation"**

AI version:
> "The results validate the effectiveness and robustness of the proposed approach.
> These findings further confirm the superior performance and adaptability of our
> method across diverse scenarios."

Human version:
> "Table 3 shows the proposed method achieves the best AUC on three of four datasets,
> with an average improvement of 5.2% over the strongest baseline."

---

## Tier 4: Section-Specific AI Patterns

### Abstract AI Markers

| Red Flag Opening | Better Opening |
|------------------|---------------|
| "With the exponential growth of..." | Start with the specific problem |
| "In recent years, X has attracted..." | Start with what your paper does |
| "The rapid advancement of..." | State your contribution first |
| "In the era of big data..." | Be specific about the domain |
| "As ... has become increasingly important" | State the concrete gap being addressed |
| "With the rapid development of..." | Start with the specific technical problem |

**Rule**: If the first sentence could apply to any paper in the field, it's too generic.

### Introduction AI Markers

| Red Flag | Better |
|----------|--------|
| "has been extensively studied in the literature" | "Prior work includes [specific methods]" |
| "there remain opportunities to improve" | "Method X has limitation Y" |
| "has garnered significant attention from researchers in the field" | "[X] has been studied [specific refs]" |
| "With the development of X, Y has become increasingly important" | State what's specifically missing |
| "poses significant challenges" | "requires O(n^2) memory" (be specific) |
| "remains a challenging task" | State WHY it remains challenging with specifics |

### Results AI Markers

| Red Flag | Better |
|----------|--------|
| "demonstrates the superior performance" | "achieves 0.998 AUC, 5% higher than baseline" |
| "remarkable resilience and superior predictive capabilities" | "maintains >0.88 AUC across all test ratios" |
| "particularly compelling is the performance on" | "On DARPA, SP-SBEAD improves AUC by 0.01" |
| "consistently outperforming existing approaches" | "outperforms all baselines on 3/4 datasets (Table X)" |
| "effectively addressing the critical challenges" | (delete, or state specific challenge addressed) |
| "remarkable Precision/Recall/F1" | "high Precision (0.998)" — use specific number |
| "superior performance and adaptability" | "X% improvement on Y datasets" |
| "demonstrates remarkable computational efficiency" | "processes N edges in T seconds" |

### Conclusion AI Markers

| Red Flag | Better |
|----------|--------|
| "This paper proposes two innovative methods" | "This paper proposes [Method A] and [Method B]" |
| "These findings strongly indicate significant potential" | "Results on four datasets confirm..." |
| "paving the way for future advancements" | "Future work will address [specific limitation]" |
| "superior performance and adaptability" | "[Method] achieves [metric] on [dataset]" |
| "demonstrate the effectiveness of the proposed approach" | "the proposed approach outperforms X by Y% on Z" |

---

## Tier 5: Pronoun + AI Style Compound Errors

These combinations of pronoun overuse and AI style are especially conspicuous:

| Compound Error | Replacement |
|----------------|-------------|
| "Our innovative approach effectively addresses" | "The proposed approach reduces X by Y%" |
| "We have successfully demonstrated" | "Results show" / "Experiments confirm" |
| "Our comprehensive experiments validate" | "Experiments on N datasets show" |
| "We achieve remarkable performance" | "[Method] achieves [metric] on [dataset]" |
| "Our novel method significantly outperforms" | "[Method] outperforms [baseline] by [%]" |
| "We believe our approach holds great promise" | "The approach shows potential for [specific use]" |
| "Our experimental results convincingly demonstrate" | "Table X shows..." |

---

## Quick Decision Guide

When you encounter a suspicious phrase, ask:

1. **Can I replace it with a specific number?** → Do so
2. **Can I delete it without losing information?** → Delete it
3. **Is it one of the Tier 1 markers?** → Must replace
4. **Is it used more than the threshold allows?** → Replace excess instances
5. **Does the sentence make a claim without evidence?** → Add evidence or remove claim

**Golden Rule**: Academic writing should be precise, not impressive. Every adjective should be backed by data.
