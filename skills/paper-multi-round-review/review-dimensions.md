# Review Dimension Checklists

Complete dimension-by-dimension checklists for each reviewer role. Use these as systematic guides during Phase 1 (Independent Reviews). Check each item and note findings.

---

## Reviewer A: Security Domain Expert — Full Checklist

### A1. Threat Model & Attack Scope

- [ ] Threat model section exists and is clearly defined
- [ ] Attacker capabilities specified (what attacker can/cannot do)
- [ ] Attacker knowledge specified (white-box, gray-box, black-box)
- [ ] Attack surface clearly delineated
- [ ] Assumptions about monitoring infrastructure stated
- [ ] Scope limitations acknowledged (e.g., "we assume kernel-level audit is available")

### A2. Attack Scenario Realism

- [ ] Evaluated attacks map to MITRE ATT&CK techniques
- [ ] Multi-stage attack chains tested (not just single-step anomalies)
- [ ] Lateral movement scenarios included
- [ ] Data exfiltration scenarios included
- [ ] C2 communication scenarios included
- [ ] Living-off-the-land attacks considered
- [ ] Encrypted traffic scenarios discussed

### A3. Dataset Quality

| Dataset Check | Status |
|---------------|--------|
| Dataset is peer-reviewed/widely-used | |
| Known dataset limitations acknowledged | |
| Attack-to-benign ratio stated | |
| Data collection methodology described | |
| Temporal span of data stated | |
| Number of hosts/flows/events stated | |
| Label quality/methodology described | |

Common dataset concerns:
- **DARPA TC/E3/OpTC**: Synthetic environment, limited number of hosts, specific attack scenarios
- **Enterprise authentication datasets**: Real enterprise data, but often only authentication events; limited attack diversity
- **Labeled IDS benchmark datasets**: Known labeling issues; some datasets have documented errors in ground truth
- **UNSW-NB15**: Synthetic attack generation in controlled lab

### A4. Domain Baseline Coverage

- [ ] Compared with at least 2 provenance-graph methods (if applicable)
- [ ] Compared with at least 1 network-only method (if applicable)
- [ ] Compared with at least 1 rule-based/signature method
- [ ] Compared with at least 1 traditional ML method (RF, XGBoost)
- [ ] Baselines use original implementations or carefully re-implemented

### A5. Operational Viability

- [ ] Detection latency measured and reported
- [ ] Memory footprint during operation discussed
- [ ] Alert volume at realistic base rates estimated
- [ ] Incremental update capability discussed (vs. full retrain)
- [ ] Integration with existing security stack discussed

### A6. Adversarial Considerations

- [ ] Evasion attacks discussed (attacker adapts to detector)
- [ ] Mimicry attacks considered (attacker mimics benign patterns)
- [ ] Data poisoning risks discussed (if online learning)
- [ ] Feature manipulation robustness analyzed

---

## Reviewer B: ML/GNN Methods Expert — Full Checklist

### B1. Architecture Design

- [ ] Architecture overview figure provided
- [ ] Each module's purpose clearly explained
- [ ] Design choices justified (why this over alternatives)
- [ ] Input/output dimensions specified for each module
- [ ] Activation functions and normalization choices explained

### B2. Novelty Decomposition

For each claimed novel component:

| Component | Truly Novel? | Closest Prior Work | Difference |
|-----------|-------------|-------------------|------------|
| | | | |
| | | | |
| | | | |

Questions to answer:
- Is the novelty in problem formulation, algorithm, or application?
- Could this be achieved by combining existing techniques?
- What is the minimal novel contribution (strip everything else away)?

### B3. Theoretical Grounding

- [ ] Key formulas/equations correctly derived
- [ ] Assumptions for theoretical claims stated
- [ ] Proofs (if any) are complete and correct
- [ ] Connection to established theory (e.g., message passing framework)
- [ ] Loss function properties discussed (convexity, convergence)

### B4. Ablation Study Completeness

For N claimed contributions, there should be at least N ablation variants:

| Contribution | Ablation Variant | Tested? | Result Delta |
|-------------|-----------------|---------|--------------|
| | | | |
| | | | |
| | | | |

Additional ablation checks:
- [ ] Each variant removes exactly ONE component
- [ ] All variants use identical hyperparameters
- [ ] All variants use identical data splits
- [ ] Results include standard deviation

### B5. Complexity Analysis

| Operation | Time Complexity | Space Complexity | Reported? |
|-----------|----------------|------------------|-----------|
| Graph construction | | | |
| Feature engineering | | | |
| Model training (per epoch) | | | |
| Inference (per sample) | | | |

### B6. Hyperparameter Sensitivity

Key hyperparameters that should be analyzed:
- [ ] Learning rate sensitivity
- [ ] Hidden dimension sensitivity
- [ ] Number of GNN layers sensitivity
- [ ] Key threshold sensitivity (e.g., confidence threshold)
- [ ] Loss function parameters (alpha, gamma for Focal Loss)
- [ ] At least one plot showing performance vs. hyperparameter

### B7. Convergence Evidence

- [ ] Training loss curve provided
- [ ] Validation loss/metric curve provided
- [ ] Evidence that model converged (not just early-stopped)
- [ ] Overfitting analyzed (train vs. val gap)

---

## Reviewer C: Experimental Methodology Expert — Full Checklist

### C1. ML-Security Pitfalls Audit (Quick Version)

See [ml-security-pitfalls.md](ml-security-pitfalls.md) for the detailed audit. Quick pass:

| # | Pitfall | Status | Notes |
|---|---------|--------|-------|
| P1 | Sampling Bias | | |
| P2 | Label Inaccuracy | | |
| P3 | Data Snooping | | |
| P4 | Spurious Correlations | | |
| P5 | Biased Parameter Selection | | |
| P6 | Inappropriate Baselines | | |
| P7 | Inappropriate Measures | | |
| P8 | Base Rate Fallacy | | |
| P9 | Lab-Only Evaluation | | |
| P10 | Inappropriate Threat Model | | |

### C2. Data Splitting Protocol

- [ ] Split method described (random/stratified/temporal/k-fold)
- [ ] Split ratios stated
- [ ] For temporal split: training period before test period
- [ ] No future information leaks into training features
- [ ] Feature normalization uses training-set-only statistics
- [ ] Feature engineering applied independently on each split
- [ ] Validation set separate from test set
- [ ] Same splits used for all compared methods

### C3. Statistical Rigor

| Check | Status |
|-------|--------|
| Number of runs stated (≥5 recommended) | |
| Mean reported | |
| Standard deviation reported | |
| Statistical significance test applied | |
| Test name specified (t-test, Wilcoxon, etc.) | |
| p-values reported for key comparisons | |
| Confidence intervals provided | |

### C4. Metric Appropriateness

For binary classification under class imbalance:

| Metric | Reported? | Appropriate? |
|--------|-----------|-------------|
| Accuracy | | Misleading under imbalance |
| Precision | | Yes, but alone insufficient |
| Recall | | Yes, but alone insufficient |
| F1-Score | | Good, but threshold-dependent |
| AUROC | | Can be misleading under extreme imbalance |
| AUPRC | | Best for imbalanced; threshold-free |
| FPR at fixed TPR | | Operationally meaningful |
| Detection latency | | Important for real-time systems |

### C5. Baseline Comparison Fairness

For each baseline:

| Baseline | Original Config? | Same Split? | Same HW? | Same Features? | Code Source |
|----------|-----------------|-------------|----------|---------------|-------------|
| | | | | | |
| | | | | | |

### C6. Reproducibility Audit

- [ ] Code available (URL or "will release upon acceptance")
- [ ] Dataset availability stated (public/restricted/private)
- [ ] All hyperparameters listed in paper or appendix
- [ ] Hardware specification provided (GPU model, RAM)
- [ ] Software environment specified (Python version, library versions)
- [ ] Random seed reported
- [ ] Training time reported
- [ ] Preprocessing steps fully described

### C7. Cross-Dataset Evaluation

- [ ] Tested on ≥2 datasets from different sources
- [ ] Datasets have different characteristics (scale, domain, attack types)
- [ ] Performance difference across datasets discussed
- [ ] No cherry-picking of favorable datasets

---

## Reviewer D: Writing & Presentation Expert — Full Checklist

### D1. Abstract (PRKS)

| Element | Present? | Quality |
|---------|----------|---------|
| **P**roblem (research gap) | | ≤20% of abstract |
| **R**esponse (proposed method) | | Clear, concise |
| **K**ey results (specific numbers) | | Match tables |
| **S**ignificance (impact) | | Not overclaimed |
| Word count within venue limit | | |
| No citations | | |
| No undefined acronyms | | |

### D2. Introduction (CARS)

| Move | Present? | Quality |
|------|----------|---------|
| M1: Establish territory | | 1-2 paragraphs, with citations |
| M2: Establish niche (gap) | | Specific, not vague |
| M3: Occupy niche (contribution) | | Numbered list, 3-5 items |
| Gap → Solution transition natural | | |
| Paper outline with \ref{} | | |

### D3. Related Work Structure

- [ ] Organized by theme (not paper-by-paper)
- [ ] Each theme: overview → key works → limitations
- [ ] Comparison table (this paper vs. related work)
- [ ] Final paragraph positions this paper explicitly
- [ ] Fair treatment of competing approaches

### D4. Method Section

- [ ] Method overview figure at beginning
- [ ] All symbols defined at first use
- [ ] Symbol/notation table (if >10 symbols)
- [ ] Core intuition explained in plain language before formalization
- [ ] Algorithm pseudocode provided
- [ ] Design choices explicitly justified ("We use X because Y")
- [ ] Complexity analysis included

### D5. Experiment Section

- [ ] Dataset statistics table
- [ ] Evaluation metrics defined (with formulas)
- [ ] Experimental setup clearly described
- [ ] Implementation details (learning rate, batch size, epochs, etc.)
- [ ] Results interpreted, not just presented
- [ ] Comparison table with bolded best results

### D6. Discussion & Limitations

- [ ] Results interpreted in context (WHY does the method work/fail)
- [ ] Failure cases analyzed
- [ ] At least 3 specific limitations stated
- [ ] Each limitation names a concrete scenario/constraint
- [ ] Broader implications discussed

### D7. Conclusion

- [ ] Multi-paragraph (2-4 paragraphs)
- [ ] Strongest takeaway first
- [ ] NOT a copy of the abstract
- [ ] Answers Introduction's research question
- [ ] Future work is specific and actionable
- [ ] No new information introduced
- [ ] Claims bounded by experimental evidence

### D8. Cross-Section Consistency

| Check | Status |
|-------|--------|
| Abstract numbers = Table numbers | |
| Each contribution → specific section/experiment | |
| Introduction gap → Method solution | |
| Method symbols = Experiment parameters | |
| Conclusion claims ⊆ Results data | |
| Terminology consistent throughout | |

---

## §GNN: Extra Dimensions for GNN Papers

When the paper uses graph neural networks, additionally check:

### GNN-Specific Issues

- [ ] Over-smoothing discussed (for >3 layers)
- [ ] Over-squashing discussed (for long-range dependencies)
- [ ] Heterogeneous message passing correctly implemented per relation type
- [ ] Attention mechanism implementation matches description
- [ ] Edge features properly handled (not just node features)
- [ ] Graph sampling strategy justified and analyzed
- [ ] Inductive vs. transductive setting clearly stated
- [ ] Node feature initialization described
- [ ] Comparison with at least one non-GNN baseline to justify graph structure
