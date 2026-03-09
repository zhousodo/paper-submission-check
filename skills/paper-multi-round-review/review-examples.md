# Review Examples

Calibration examples showing what good reviews look like at different verdict levels. Based on real ICLR/NeurIPS review patterns (anonymized and adapted).

---

## Example 1: Borderline Accept Review (Reviewer B — Methods Expert)

Context: A paper proposing a heterogeneous GNN for network anomaly detection with uncertainty-aware alignment.

```markdown
# Review — Reviewer B
Expertise: Graph neural networks, heterogeneous graph learning, uncertainty quantification

## 1. Summary
This paper proposes a cross-modal fusion method for network anomaly detection that bridges
multi-source heterogeneous data. The core contribution is a multi-factor alignment scoring
mechanism that produces continuous quality scores for cross-modal alignment, replacing binary
hard matching. The method constructs a role-aware heterogeneous graph with directed
heterogeneous edges encoding source/target roles, and uses quality-weighted message passing
in a heterogeneous GNN. Experiments on Dataset-A and Dataset-B show strong F1 scores
(95.3% and 96.5%).

## 2. Strengths
- **S1** [§4.1]: The multi-factor alignment scoring is well-designed. Fusing spatial,
  temporal, semantic, behavioral, and contextual signals provides a more robust alignment
  than any single dimension. The formulation is clean and the weight decomposition is
  intuitive.
- **S2** [§4.2]: The role-differentiated edge distinction is a meaningful modeling choice
  that captures directional semantics. The asymmetric weight splitting (source: 1.0/0.4,
  target: 0.5/1.0) is well-motivated from the application perspective.
- **S3** [§4.3]: Post-softmax quality scaling is an interesting design choice that
  preserves the attention competition structure while modulating message amplitudes.
  This is technically cleaner than pre-softmax weighting.
- **S4** [§5]: The ablation study covers the main contributions (cross-modal fusion,
  uncertainty modulation, temporal encoding, role distinction).

## 3. Weaknesses
- **W1** [§4.1] 🟧 Major: The alignment dimensions and their weights (0.30, 0.15, 0.35,
  0.15, 0.05) appear to be manually set. There is no sensitivity analysis showing
  how performance changes with different weight configurations. Are these weights
  optimal? Were they tuned, and if so, on what data?
  *Suggestion*: Add a weight sensitivity analysis (e.g., vary each weight ±50% and
  report F1 change), or discuss learning these weights end-to-end.

- **W2** [§5.3] 🟧 Major: The ablation study removes components but doesn't isolate them
  sufficiently. For example, removing "cross-modal fusion" simultaneously removes the
  alignment scoring, the role-differentiated edges, and the GNN message passing. It would be
  more informative to have finer-grained ablations:
  (a) Replace continuous quality scores with binary (hard alignment, score=1.0)
  (b) Replace directed role-differentiated edges with undirected edges
  (c) Remove temporal encoding only
  These would directly validate each claimed contribution independently.

- **W3** [§4.3] 🟨 Minor: The uncertainty modulation coefficient β=0.1 means the
  uncertainty adjustment range is only [1.0, 1.1] — a maximum 10% increase in loss
  weight. It is unclear whether such a small modulation has meaningful impact.
  *Suggestion*: Report the distribution of uncertainty values across samples and show
  that the modulation actually changes gradient magnitudes in practice.

- **W4** [§5] 🟨 Minor: No computational cost comparison with baselines. How much
  additional time does the multi-factor scoring and role-aware graph construction add?

## 4. Questions for Authors
- **Q1**: How sensitive are the results to the alignment dimension weights? What happens
  if you set all weights to 0.20 (uniform)?
- **Q2**: The semantic similarity dimension (f_sem, weight 0.35) dominates. If you remove
  it entirely, how much does performance drop? This would clarify whether the method's
  strength comes from multi-dimensional scoring or primarily from semantic matching.
- **Q3**: How many edges are created in the role-aware graph compared to a hard-alignment
  baseline? What fraction of edges have quality score < 0.5?

## 5. Missing References
- ConfGCN (Vashishth et al., 2019) is cited but not compared against as a baseline.
  Since it also introduces quality weighting into GNN message passing, a direct comparison
  would strengthen the novelty claim.

## 6. Scores
| Dimension     | Score | Scale |
|---------------|-------|-------|
| Soundness     | 3/4   | Claims mostly supported; weight selection needs justification |
| Presentation  | 3/4   | Clear writing; good figures; notation consistent |
| Contribution  | 3/4   | Good problem formulation; novelty in alignment scoring + role edges |
| Overall       | 5/10  | Borderline accept — technically solid, needs stronger ablation |
| Confidence    | 4/5   | Confident in GNN methodology assessment |

## 7. Verdict
Borderline Accept — The problem formulation and overall approach are sound and
well-motivated. The main concerns are the manually-set alignment weights without
sensitivity analysis, and the coarse-grained ablation study. If these are addressed
in revision, the paper would be a clear accept.
```

---

## Example 2: Reject Review (Reviewer C — Methodology Expert)

Context: Same paper, but this reviewer focuses on experimental methodology pitfalls.

```markdown
# Review — Reviewer C
Expertise: ML evaluation methodology, statistical rigor, reproducibility

## 1. Summary
The paper proposes ProposedMethod for network anomaly detection, combining multi-source
heterogeneous data through a multi-factor alignment scoring mechanism. The method is
evaluated on Dataset-A and Dataset-B, reporting F1 scores of 95.3% and 96.5%.

## 2. Strengths
- **S1** [§4]: The problem formulation is well-motivated. The limitations of hard alignment
  in cross-modal fusion are clearly articulated with concrete examples (dynamic
  addressing scenarios (e.g., DHCP, NAT)).
- **S2** [§5.1]: Using both temporal and random splits for evaluation is a good practice
  that many papers in this area neglect.

## 3. Weaknesses
- **W1** [§5] ⬛ Critical: No standard deviations or confidence intervals are reported for
  any results. All comparisons are based on single-point estimates. Without variance
  information, it is impossible to determine whether the reported improvements are
  statistically significant or within random fluctuation. For example, the claimed 3.2%
  F1 improvement from cross-modal fusion could be entirely within the noise of a single
  random seed.
  *Required*: Report mean ± std over at least 5 runs with different random seeds. Apply
  a paired statistical test for key comparisons.

- **W2** [§5.1] ⬛ Critical: The paper does not describe how the detection threshold was
  selected. F1 is threshold-dependent — at what threshold was 95.3% F1 achieved? Was
  this threshold selected on the validation set or the test set? If on the test set, this
  constitutes parameter selection bias (Pitfall P5).
  *Required*: Specify threshold selection procedure. Report AUPRC (threshold-free)
  as the primary metric.

- **W3** [§5.1] 🟧 Major: The base rate fallacy is not addressed. At what anomaly prevalence
  were these results achieved? If the test set has 10% anomalies but real deployment has
  0.01% anomalies, a 1% FPR would generate ~100 false alarms per true detection.
  *Suggestion*: Compute precision at realistic base rates (e.g., 0.01%, 0.1%, 1%)
  and report expected daily false alarm counts for a network of realistic scale.

- **W4** [§5.2] 🟧 Major: Feature normalization procedure not described. Are features
  normalized using training-set-only statistics? For graph-based features (centrality,
  PageRank), are these computed on the training subgraph only, or on the full graph
  including test nodes? The latter would constitute data snooping (Pitfall P3).
  *Required*: Explicitly state the normalization procedure and confirm no test-time
  information leaks into training.

- **W5** [§5] 🟧 Major: Only two datasets are used. While they represent different
  application domains, both show strong performance (>95% F1). This raises concerns
  about whether the method truly generalizes or whether these specific datasets are
  particularly easy. A third established benchmark with different characteristics would
  strengthen the claims.

- **W6** [§5.2] 🟨 Minor: Baseline implementation details are insufficient. Are baselines
  re-implemented or using original code? Are they using their published optimal
  hyperparameters? Without this information, the comparison may be unfair.

## 4. Questions for Authors
- **Q1**: What is the detection threshold used for reporting F1? How was it selected?
- **Q2**: What is the anomaly prevalence in each dataset's test split?
- **Q3**: Are graph structural features (centrality, etc.) computed on the full graph or
  training subgraph only?
- **Q4**: How many random seeds were used? What is the variance across runs?

## 5. Missing References
- Arp et al. "Pitfalls in Machine Learning for Computer Security" (CACM 2024) —
  directly relevant to the methodological concerns raised here.

## 6. Scores
| Dimension     | Score | Scale |
|---------------|-------|-------|
| Soundness     | 2/4   | No variance, threshold bias risk, potential data snooping |
| Presentation  | 3/4   | Well-written but methodology section lacks critical details |
| Contribution  | 3/4   | Good idea, but execution has methodological gaps |
| Overall       | 4/10  | Borderline reject — methodology concerns undermine results |
| Confidence    | 5/5   | Very confident in methodology assessment |

## 7. Verdict
Borderline Reject — The conceptual contribution is interesting, but the experimental
methodology has critical gaps: no statistical significance testing, unclear threshold
selection, and potential data snooping through graph features. These issues must be
resolved before the results can be trusted. A major revision addressing all five
critical/major weaknesses would make the paper suitable for re-review.
```

---

## Example 3: Meta-Review Synthesis

```markdown
# Meta-Review (Area Chair Synthesis)

## Consensus Strengths
1. Problem formulation is well-motivated (all 4 reviewers agree)
2. Multi-factor alignment scoring is a meaningful contribution (Rev A, B, D)
3. Role-differentiated edge distinction captures real application semantics (Rev A, B)

## Consensus Weaknesses
1. No statistical significance / variance reporting (Rev B, C — both flag as critical)
2. Ablation study is too coarse-grained (Rev B, C — both flag as major)
3. No base rate analysis for realistic deployment (Rev A, C)

## Divergent Points
- Rev B rates contribution 3/4; Rev C rates 3/4 but with methodology concerns
  that may lower effective contribution. Need discussion on whether methodology
  issues are fixable or fundamental.
- Rev A wants adversarial robustness discussion; Rev D considers it optional
  for a detection paper at this stage.

## Critical Path to Acceptance
Priority-ordered must-fix list:
1. **Add variance reporting**: Mean ± std over ≥5 runs for all results (Rev B, C)
2. **Clarify threshold selection**: Specify procedure; add AUPRC metric (Rev C)
3. **Finer-grained ablation**: Isolate each contribution independently (Rev B)
4. **Alignment weight sensitivity**: Analyze impact of weight choices (Rev B)
5. **Base rate analysis**: Report precision at realistic prevalence (Rev A, C)
6. **Normalization procedure**: Confirm no data leakage (Rev C)

## Score Summary
| Reviewer | Soundness | Presentation | Contribution | Overall | Confidence |
|----------|-----------|--------------|--------------|---------|------------|
| A        | 3/4       | 3/4          | 3/4          | 6/10    | 4/5        |
| B        | 3/4       | 3/4          | 3/4          | 6/10    | 4/5        |
| C        | 2/4       | 3/4          | 3/4          | 4/10    | 5/5        |
| D        | 3/4       | 3/4          | 3/4          | 6/10    | 3/5        |
| **Mean** | **2.75**  | **3.00**     | **3.00**     | **5.50**| **4.00**   |

## Preliminary Decision
Revise & Resubmit — The paper presents a good conceptual contribution but needs
methodological strengthening. The critical path items (especially #1-3) must be
addressed before acceptance. If all items are satisfactorily resolved, the paper
would likely receive 6-7/10 in re-review.
```

---

## Example 4: Rebuttal Response Template

```markdown
# Author Response to Reviewer C

We thank Reviewer C for the thorough methodology review. We address each concern:

## Re: W1 (No variance reporting) — ⬛ Critical

We agree this is a significant gap. We have re-run all experiments with 10 random
seeds and report mean ± std in revised Tables 3-5. Key results:
- Dataset-A F1: 95.3 ± 0.42% (original: 95.3%)
- Dataset-B F1: 96.5 ± 0.25% (original: 96.5%)

Paired t-tests confirm our method significantly outperforms all baselines (p < 0.01).
See revised §5.3 and new Table 6.

## Re: W2 (Threshold selection) — ⬛ Critical

The detection threshold was selected on the validation set using F1 maximization.
We apologize for omitting this detail. We have:
1. Added explicit threshold selection description in revised §5.1
2. Added AUPRC as a primary metric (Table 3): Dataset-A 97.8%, Dataset-B 98.5%
3. Added full Precision-Recall curves in new Figure 7

## Re: W3 (Base rate fallacy) — 🟧 Major

We have added a base-rate analysis in new §5.5:
| Base Rate | Precision | Daily FP (10K flows/day) |
|-----------|-----------|--------------------------|
| 10%       | 99.2%     | ~8                       |
| 1%        | 92.1%     | ~79                      |
| 0.1%      | 56.3%     | ~437                     |
| 0.01%     | 10.2%     | ~879                     |

We acknowledge that at very low base rates, precision degrades significantly.
This is a fundamental challenge shared by all detection methods, and we discuss
mitigation strategies (cascaded filtering, alert prioritization) in revised §6.

[continue for each weakness...]
```

---

## Key Lessons from Examples

1. **Specific references**: Good reviews cite section numbers and line numbers, not vague complaints.
2. **Severity calibration**: Not everything is "critical." Reserve that for issues that invalidate core claims.
3. **Constructive suggestions**: Each weakness should suggest a concrete fix.
4. **Questions ≠ weaknesses**: Questions seek clarification; weaknesses identify problems.
5. **Score justification**: Each score should be traceable to specific strengths/weaknesses.
6. **Rebuttal structure**: Address concerns in order of severity, with concrete evidence of fixes.
