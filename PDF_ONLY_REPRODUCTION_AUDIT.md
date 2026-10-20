# PDF-Only Reproduction Audit

**PDF used:** `Rebalance_or_Repair__Joint_Workload_Allocation_and_Condition_Based_Maintenance_in_Deteriorating_Assembly_Lines-2.pdf` (22 pp., uploaded file).
**Benchmark data used:** Scholl (1993) SALBP instances via the GitHub mirror `github.com/zixiangliwust/Instances_SALBP1` (files `P7_6_MERTENS`, `P9_6_JAESCHKE`, `P11_7_JACKSON`, `P21_14_MITCHELL`; only task times and precedences used). The official site assembly-line-balancing.de was blocked by the network proxy. Check: T = 29/37/46/105 reproduces Table 3's d_P(2) = 377/481/598/1365 exactly.
**Other files/resources consulted:** none. No repository code, `.tex`, supplement, archive, result files or prior audits were opened. Two web searches were used only to locate a benchmark mirror. The model is an independent Python implementation (policy iteration) written from Eqs. (1)–(24) and Tables 3–6. **Not contaminated.**

## Overall verdict

**PARTIAL.** The principal results reproduce, but the following items require information that is absent from or ambiguous in the PDF.

## Headline checks

| Result | Paper | Reproduced | Verdict |
|---|---|---|---|
| G lit. max (ρ_P=0.1, m3=1.3): Mertens / Jaeschke / Jackson / Mitchell | 1.739 / 2.836 / 3.836 / 3.180 % | 1.7387 / 2.8357 / 3.8356 / 3.1797 % | PASS |
| G stress max: Mertens (0.3, 1.75) | 5.803 % | 5.8032 % at (0.3, 1.75) | PASS |
| G stress max: Jaeschke / Jackson / Mitchell (0.3, 2.0) | 5.925 / 9.826 / 10.125 % | 5.9252 / 9.8264 / 10.1253 % at (0.3, 2.0) | PASS |
| Tied all-new c°-optimal counts (Fixed) | 1, 4, 5, 6 | 1, 4, 5, 6 | PASS |
| Full>CT-min: records / distinct points | 26 / 25 | 26 / 25 † | PARTIAL |
| Largest / median / smallest Δ_CT | 0.077447 / 0.007741 / ≈0.00196 % | 0.077447 / 0.007741 / 0.001959 % † | PARTIAL |
| Mertens state 211 (ρ_P=0.3, m3=1.75): CTs, ν, g·ΔCT, wear saving, net, Bellman margin | 12.375 vs 12.0; 0.0847; 0.2028; 0.2578; 0.0550; 8.12 | 12.375 vs 12.0; 0.08469; 0.20280; 0.25779; 0.05500; 8.1196 | PASS |
| State-211 workloads | Full (9,9,11); CT-min (6,11,12) | (9,10,10) ≡ (9,9,11) exact tie; (6,11,12) | PASS (tie) |
| Exp 3 Q-base column (M0: 0,0,0,0; M1: 0,17,9,10) and M1 clock masses 0.2395 / 0.0010 / 0.0485 | as stated | identical, including the Jaeschke/Jackson workload changes | PASS |
| Exp 3 Q-H1 / Q-I2 / Q-H2 counts (Table 8) | e.g. M0 Mitchell 24/64; M1 Jackson 121/122; M1 Mitchell 43/43 | 21 of 28 cells match under my default tie-break; e.g. Mitchell M0 14–24 / 47–58, Jackson M1 96–108 / 92–104 across tie-breaks | FAIL (those cells) |

† Reproduced only after reading the 60 refinement points (15 per line) off Figure 2. All 60 lie in my independently computed candidate set.

## What reproduced

- The model is fully reconstructible: workloads, the A1 kernel, sojourns (8)–(10), reward (11), the Bellman equation (14), the Eq. (23) normalization, θ = r32/δ_C and the m(2) rule. All Bellman gaps are ≤ 2×10⁻¹⁰.
- All eight Full–Fixed maxima, and their locations, reproduce to the third decimal. No refinement candidate exceeds them, so these maxima don't depend on the refinement selection.
- Experiment 2's 41 points follow uniquely from Section 6.5. They give 10 positive Δ_CT points (Jackson 7, Mitchell 3), consistent with Figure 4b.
- The Full–CT-min summary (25 points, maximum 0.077447%, median 0.007741%) and the state-211 decomposition reproduce exactly.
- The Experiment 3 Q-base results, the M1 clock masses, and the qualitative M0/M1 pattern (Q-I2 changes Mertens, Jackson and Mitchell; Q-H2 changes all four lines at M0; Mertens is unchanged at M1) all reproduce.

## What did not reproduce

- **Refinement selection (Exp 1).** The text caps refinement at 15 points per line, but my classifications yield 23, 31, 34 and 32 eligible midpoints. The selection order is only in Supplement S4, so the PDF text can't regenerate the 57-point grid. Figure 2 shows the points graphically, and that is the only reason the Δ_CT count reproduces. The order matters: an unselected Mitchell candidate at (ρ_P, m3) = (0.1, 1.225) gives Δ_CT = 0.179%, which would change the reported maximum and the "below 0.1%" statement.
- **Experiment 3 stress-profile counts (Q-H1, Q-I2, Q-H2).** The optimal policy is not unique. With δ₀ = 0, joint and sequential PM are exactly tied, and tied workloads also exist, so different optimal policies have different recurrent sets. The Bellman-tie filter removes tied actions in a state but not the change in which states are recurrent. The PDF gives no tie-breaking rule for selecting the reported Full policy, so 7 of 28 Table 8 cells can't be reproduced. The same ambiguity affects the claim that M0 has zero workload changes: Mitchell shows 1 under some tie-breaks.
- **Q-I2 trigger rule.** It is stated only by outcome ("selected Q-I2 at M0 on all four lines"); the rule itself is in S4. This doesn't block anything, because the outcome is given.

## Main numerical discrepancies

There are none among the principal reported numbers. The only numerical mismatch is in the Experiment 3 stress cells above, and it comes from unspecified tie-breaking rather than a model error. Its size is large in some cells: for example, M1 Jackson Q-H2 is reported as 122, and I obtained 92–104.

## Bottom line

1. Yes. The model can be reconstructed completely and unambiguously from the PDF plus the four Scholl instances.
2. Yes for the Full–Fixed maxima, the Experiment 2 results, the mechanism example and the Experiment 3 Q-base results. For the Full–CT-min count and maximum, only by reading the refinement points off Figure 2. Not for the stress-profile policy-change counts in Table 8.
3. For PDF-only reproducibility, the paper needs to state the refinement selection order (or list the 15 added points per line in a table), and the tie-breaking rule used to pick the reported optimal Full policy among exactly tied actions (PM subset and workload), which determines the recurrent states counted in Table 8.
