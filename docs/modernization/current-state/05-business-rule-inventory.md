# LIFE400 Business Rule Inventory

This document is the rule census for the LIFE400 assessment. It answers three questions and nothing else: how many business rules are documented for this system, how many of them can be traced to a line of source, and by what repeatable procedure the remainder are traced. It exists because migration parity has to be asserted against something, and in an estate with no test members that something can only be a rule inventory anchored to source.

Scope. This document owns the authoritative rule count, the inline rule-identifier bands, the mapping from each identifier band to the [paragraph](../reference/glossary-ibm-i.md#paragraph) that contains it, the anchored-versus-unanchored split, the extraction procedure, and the record format the parity harness consumes. No other document in this set publishes a competing count; where another document needs a rule figure it cites this one.

Out of scope, deliberately. This document does not design the characterization suite — that is [the characterization test strategy](../migration/05-characterization-test-strategy.md) — and does not design the comparison itself, which is [parallel run and output parity](../migration/06-parallel-run-and-output-parity.md). It contains no test code and no runnable code of any kind. It does not disposition the defects it happens to surface; every irregularity recorded below is stated as a measurement and handed to [the known defects and stubs register](07-known-defects-and-stubs.md). It does not restate all 333 documented rules in prose, because the prior corpus already holds that prose and duplicating it would create a second copy to drift.

## How this census was measured

Every figure in this document was produced by running a pattern over the working tree. None is quoted from an earlier description of the system, and where an earlier description disagrees the disagreement is named and reconciled rather than passed over. Four counting rules are used, and they count four different things — most of the confusion in earlier figures comes from treating them as one.

| Counting rule | What it counts | Why it is needed |
|---|---|---|
| Corpus rule row | One row of a rule table in the prior documentation corpus, that is a line matching `^\| BR-nnn \|` | This is the unit the corpus itself counts, so it is the only rule that reproduces the published total |
| Unique identifier | Distinct strings matching `(NB\|SV\|CL)-` followed by three or four digits, anywhere in `QCBLLESRC/*.cbl` | This is the set of rule numbers the source actually names, and therefore the set an anchored inventory can start from |
| Raw occurrence | Every occurrence of the same pattern, counting repeats | Identifiers recur legitimately, so the occurrence count is what a naive grep reports and must be published alongside the unique set to prevent the two being confused |
| Definition site | An identifier immediately followed by a colon, the form `* NB-202: ISSUE AGE LIMITS` | This is the subset that carries a description of the rule at the line the rule is implemented, and it is the strictest reading of what "anchored" means |

The patterns, stated so a reader can reproduce every number without guessing at the intent:

```text
corpus rule row   ^\| *BR-[0-9]{3} *\|          over .swm/*.sw.md
unique / raw      \b(NB|SV|CL)-[0-9]{3,4}\b     over QCBLLESRC/*.cbl
definition site   \b(NB|SV|CL)-[0-9]{3,4}:      over QCBLLESRC/*.cbl
```

Three sources are reconciled, and each contributes something the other two cannot. The prior corpus contributes the rule population and the descriptive prose. The source members contribute the anchors, the literals and the observable effects. The repository overview contributes an independent domain summary that was written by hand rather than generated, so it serves as a check that no whole domain has been missed.

The corpus under `.swm/` is machine-generated and is consumed strictly as read-only prior art. It is cited by walkthrough and line, never edited, and none of its markup is reproduced here: its generated path and token tags, its click-to-open directives, its duplicated diagram comment blocks and its layout-engine directives are tool-emitted and cannot be hand-authored correctly. Only its prose and its table cells are used, and every prose claim taken from it is verified against the member it describes before it is carried into this inventory. One case below shows why that verification is not a formality.

## The documented rule corpus

### Rules by walkthrough

The corpus publishes a rule count per document [.swm/business-rules-statistics.md:L4-L14] and a total [.swm/business-rules-statistics.md:L29]. Both are reproduced here, and the total is reconciled arithmetically rather than quoted, because it is the one figure in this document a reader can verify without opening a source member.

| Walkthrough under `.swm/` | Program covered | Documented rules |
|---|---|---|
| `mainmenu-menu-interaction-and-dispatch.xdjwx2vk.sw.md` | `QCBLLESRC/MAINMENU.cbl` | 67 |
| `nbuwmnt-new-business-maintenance.jngxvgpo.sw.md` | `QCBLLESRC/NBUWMNT.cbl` | 64 |
| `svcbilb-batch-policy-servicing-and-amendments.hnvq1sdj.sw.md` | `QCBLLESRC/SVCBILB.cbl` | 57 |
| `nbuwb-new-business-and-underwriting-batch-processing.yatiwmqj.sw.md` | `QCBLLESRC/NBUWB.cbl` | 48 |
| `svcmnt-policy-servicing-maintenance.um1zpijd.sw.md` | `QCBLLESRC/SVCMNT.cbl` | 43 |
| `clmmnt-interactive-claims-maintenance.fqoabtf9.sw.md` | `QCBLLESRC/CLMMNT.cbl` | 25 |
| `clmadjb-claim-adjudication.2q95z11f.sw.md` | `QCBLLESRC/CLMADJB.cbl` | 21 |
| `polmstinq-policy-master-inquiry.4dyvkwri.sw.md` | `QCBLLESRC/POLMSTINQ.cbl` | 8 |
| `changing-a-policyholders-insurance-plan.1ogi2aoz.sw.md` | scenario, no single program | 0 |

67 + 64 + 57 + 48 + 43 + 25 + 21 + 8 + 0 = **333**, which agrees with the published total [.swm/business-rules-statistics.md:L29].

All 333 come from the eight program walkthroughs. The ninth document is a scenario narrative and contributes **0** [.swm/business-rules-statistics.md:L6], so the rule population is exactly co-extensive with the eight ILE COBOL programs and nothing else. Nothing in the corpus describes a rule in the five ILE CL members, and nothing describes a rule expressed only in DDS.

### The same total, derived a second way

The published counts were taken on trust by every earlier description of this system. They are independently reproducible, and reproducing them establishes what the corpus means by "a rule" — which the extraction procedure then depends on.

A rule in the corpus is one **row of a rule table**. The tables carry five columns, headed Rule ID, Category, Rule Name, Description and Implementation Details, for example [.swm/nbuwb-new-business-and-underwriting-batch-processing.yatiwmqj.sw.md:L386]. Counting rows across the corpus yields **333**, and the per-document breakdown agrees with the published table for **all nine** documents, not merely in total. The rows are spread over **73** rule tables: 14 in the online new-business walkthrough, 13 in the batch servicing walkthrough, 11 in the batch new-business walkthrough, 10 each in the menu and online servicing walkthroughs, 7 in the online claims walkthrough, 6 in the batch claims walkthrough, 2 in the inquiry walkthrough and none in the scenario document.

One measurement caveat matters for anyone re-running this count. A naive count of identifier occurrences rather than table rows returns 71 for the menu walkthrough against its 67 rows, because one rule table was emitted flattened onto a single physical line together with adjoining prose [.swm/mainmenu-menu-interaction-and-dispatch.xdjwx2vk.sw.md:L2301]. Row counting is correct; occurrence counting is not. This is the first of several places where a plausible pattern gives a wrong answer, and it is the reason every counting rule in this document is stated before it is used.

### What kind of rules they are

The Category column classifies all 333 rows. The distribution is worth publishing because it predicts how each rule can be asserted on: a calculation rule yields a number to compare, a decision rule yields a branch outcome, and an output rule yields a written record.

| Category | Rows |
|---|---|
| Decision Making | 123 |
| Calculation | 85 |
| Data validation | 78 |
| Writing Output | 25 |
| Invoking a Service or a Process | 13 |
| Reading Input | 9 |

123 + 85 + 78 + 25 + 13 + 9 = **333**, a third independent reconciliation of the same total.

Two thirds of the population — the 208 rows classified as decision or calculation — turn on a value the code computes rather than on a field the user supplied, which is precisely the class of rule that cannot be verified by reading a screen and must be captured from the record the program writes.

### The corpus supplies no stable per-rule identifier

The corpus does assign identifiers, in the form `BR-nnn`. They cannot be used as inventory keys, and the measurement showing why is unambiguous: numbering **restarts at `BR-001` in every table**, so the identifiers are table-local rather than document-local, let alone corpus-wide. Only **13** distinct identifiers exist across all 333 rules.

| Identifier | Times reused across the corpus |
|---|---|
| `BR-001` | 73 |
| `BR-002` | 71 |
| `BR-003` | 67 |
| `BR-004` | 48 |
| `BR-005` | 29 |
| `BR-006` | 15 |
| `BR-007` | 7 |
| `BR-008` | 7 |
| `BR-009` | 6 |
| `BR-010` | 4 |
| `BR-011` | 3 |
| `BR-012` | 2 |
| `BR-013` | 1 |

73 + 71 + 67 + 48 + 29 + 15 + 7 + 7 + 6 + 4 + 3 + 2 + 1 = **333**. `BR-001` occurring exactly 73 times is the same 73 as the table count, which is what "one per table" means.

The consequence is structural rather than cosmetic. A parity report that cited `BR-006` would be ambiguous across 15 different rules, and a defect raised against `BR-001` would be ambiguous across 73. The inventory therefore cannot adopt the corpus identifiers; it needs its own, and it needs them stable, which is what the extraction procedure below is for.

## Where the rules are anchored in source

### The measured identifier census

Three of the eight programs carry inline rule identifiers in their comments. The counts below are of the source as it stands in this repository.

| Program | Band | Unique identifiers | Raw occurrences | Definition sites |
|---|---|---|---|---|
| `QCBLLESRC/NBUWB.cbl` | `NB-` | 28 | 42 | 25 |
| `QCBLLESRC/SVCBILB.cbl` | `SV-` | 16 | 24 | 8 |
| `QCBLLESRC/CLMADJB.cbl` | `CL-` | 15 | 23 | 13 |
| `QCBLLESRC/NBUWMNT.cbl` | none | 0 | 0 | 0 |
| `QCBLLESRC/SVCMNT.cbl` | none | 0 | 0 | 0 |
| `QCBLLESRC/CLMMNT.cbl` | none | 0 | 0 | 0 |
| `QCBLLESRC/POLMSTINQ.cbl` | none | 0 | 0 | 0 |
| `QCBLLESRC/MAINMENU.cbl` | none | 0 | 0 | 0 |
| **Total** | | **59** | **89** | **46** |

Every identifier in the estate lives in a comment. Not one is a data name, a paragraph label or a literal, so no identifier is visible to the compiler and nothing in the build would notice if one were deleted, duplicated or misnumbered.

### Reconciling the published figure of 46

Earlier descriptions of this estate state **46** anchored identifiers, broken down as 25 in the batch new-business program, 13 in the batch claims program and 8 in the batch servicing program. This document publishes **59** unique identifiers and **89** raw occurrences. Both figures are correct, and the difference is fully explained rather than merely noted.

**46 is the definition-site count.** Measured per band, definition sites number 25 for `NB-`, 8 for `SV-` and 13 for `CL-` — matching the earlier breakdown band for band, exactly. So the earlier figure counts identifiers that carry a description at the line where the rule is implemented, and it counts them correctly. It simply counts a narrower thing than "identifiers the source names".

The 89 raw occurrences decompose without remainder:

| Occurrence kind | Count | Where it appears |
|---|---|---|
| Definition site | 46 | A comment line naming one rule and describing it, immediately above that rule's code |
| Paragraph-banner occurrence | 43 | The paragraph's comment banner, which names either a single rule or a range |
| **Total raw occurrences** | **89** | |

The 43 banner occurrences come from **26** banner lines: 17 banners state a range and therefore name two identifiers each, and 9 banners name a single identifier, giving 17 × 2 + 9 = 43. Adding the 46 definition sites gives 89.

The unique set follows from the same decomposition: 46 identifiers have a definition site, and a further **13** are named only on a banner and nowhere else, giving 59. Those 13 are `NB-101`, `NB-601`, `NB-1001`, `SV-101`, `SV-401`, `SV-403`, `SV-501`, `SV-601`, `SV-701`, `SV-801`, `SV-1001`, `CL-101` and `CL-601`. A banner-only identifier names a rule without describing it, so it locates the rule to a paragraph but not to a line.

The counting rule this document adopts, and the reason for it: **the unique identifier set, 59.** An inventory key has to exist for every rule the source names, including the thirteen the source names without describing. Adopting 46 would leave those thirteen rules with no key at all, and they include the whole of plan-parameter loading in all three domains and the whole of the plan-change and repricing logic in servicing — not marginal rules. The two narrower figures are published alongside so that neither can be mistaken for the other again.

### What that leaves unanchored

Anchoring is now arithmetic. 333 documented rules, 59 of them named by an identifier in source:

| Quantity | Value | Share of 333 |
|---|---|---|
| Rules named by an inline source identifier | 59 | 18% |
| Rules with no inline source identifier | 274 | 82% |

333 − 59 = **274**. Earlier derived figures of 287 unanchored and 86% rest on the 46 count and are superseded by this recomputation; they are named here only so a reader meeting them elsewhere knows which figure replaced them and why.

The 82% is the size of the extraction job, and it is not distributed evenly. It is concentrated in the five programs that carry no identifier at all, which between them account for 207 of the 333 documented rules.

### Documentation volume is not a proxy for anchoring

The two quantities are inversely related in this estate, which is worth stating plainly because it inverts the intuition that a well-documented program is an easy program to convert.

- `MAINMENU` carries the **largest** documented rule count of any program, 67 — and is the least structurally anchored member in the estate. It has **zero** inline rule identifiers, and [the current-state architecture](02-architecture-current-state.md), which owns the paragraph inventory, records **zero** numbered paragraphs in it: its whole procedure is one driver paragraph containing a single inline loop [QCBLLESRC/MAINMENU.cbl:L60]. There is no identifier to anchor a rule to, and no paragraph to anchor it to either.
- `CLMADJB` carries the **smallest** rule count of the three batch programs, 21 — and 15 identifiers, the highest ratio of identifiers to documented rules in the estate at 15 of 21. Its rules are the most precisely located of any program's. The batch servicing program is at the other end of the three, with 16 identifiers against 57 documented rules, and the batch new-business program sits between them at 28 against 48.
- `POLMSTINQ` carries 8 documented rules and no identifiers, yet its rules are the easiest of the unanchored set to locate, because [the current-state architecture](02-architecture-current-state.md) describes it as a three-paragraph read model: with so few paragraphs, paragraph-level binding is nearly as precise as an identifier would be.

A rule count is therefore not a proxy for extraction difficulty. The ratio of identifiers to documented rules is closer, and the number of paragraphs available to bind an unanchored rule to is closest of all — which is why the procedure below binds every rule to a paragraph first and to an identifier second.

## The three identifier bands

The three bands partition by business domain, one band per batch program: `NB-` for new business and underwriting, `SV-` for servicing and billing, `CL-` for claims. The tables below enumerate every identifier that exists, its group paragraph, its definition site if it has one, and what it governs. Paragraph labels and line numbers are cited from the members directly; the per-program paragraph inventory itself is owned by [the current-state architecture](02-architecture-current-state.md).

### The `NB-` band: new business and underwriting, 28 identifiers

| Identifier | Group paragraph in `QCBLLESRC/NBUWB.cbl` | Definition site | What it governs |
|---|---|---|---|
| `NB-101` | `1100-LOAD-PLAN-PARAMETERS` [QCBLLESRC/NBUWB.cbl:L144] | banner only [QCBLLESRC/NBUWB.cbl:L142] | Loading the per-plan parameter set |
| `NB-201` | `1200-VALIDATE-APPLICATION` [QCBLLESRC/NBUWB.cbl:L197] | [QCBLLESRC/NBUWB.cbl:L198] | Mandatory fields |
| `NB-202` | `1200-VALIDATE-APPLICATION` | [QCBLLESRC/NBUWB.cbl:L230] | Issue-age limits |
| `NB-203` | `1200-VALIDATE-APPLICATION` | [QCBLLESRC/NBUWB.cbl:L238] | Sum-assured limits |
| `NB-204` | `1200-VALIDATE-APPLICATION` | [QCBLLESRC/NBUWB.cbl:L246] | Maturity-age rule |
| `NB-205` | `1200-VALIDATE-APPLICATION` | [QCBLLESRC/NBUWB.cbl:L253] | Term-to-65 plan, hazardous occupation not permitted |
| `NB-206` | `1200-VALIDATE-APPLICATION` | [QCBLLESRC/NBUWB.cbl:L261] | Severe occupation, automatic decline |
| `NB-301` | `1300-DETERMINE-UW-CLASS` [QCBLLESRC/NBUWB.cbl:L273] | [QCBLLESRC/NBUWB.cbl:L274] | Default to the preferred class when all criteria are met |
| `NB-302` | `1300-DETERMINE-UW-CLASS` | [QCBLLESRC/NBUWB.cbl:L282] | Table-B conditions |
| `NB-303` | `1300-DETERMINE-UW-CLASS` | [QCBLLESRC/NBUWB.cbl:L288] | Decline for a smoker over 60 with a high sum assured |
| `NB-401` | `1400-LOAD-RATE-FACTORS` [QCBLLESRC/NBUWB.cbl:L301] | [QCBLLESRC/NBUWB.cbl:L302] | Base mortality rate by age band |
| `NB-402` | `1400-LOAD-RATE-FACTORS` | [QCBLLESRC/NBUWB.cbl:L315] | Gender factor |
| `NB-403` | `1400-LOAD-RATE-FACTORS` | [QCBLLESRC/NBUWB.cbl:L321] | Smoker factor |
| `NB-404` | `1400-LOAD-RATE-FACTORS` | [QCBLLESRC/NBUWB.cbl:L327] | Occupation factor |
| `NB-405` | `1400-LOAD-RATE-FACTORS` | [QCBLLESRC/NBUWB.cbl:L334] | Underwriting-class factor, and outside its own paragraph's declared range |
| `NB-501` | `1500-VALIDATE-RIDERS` [QCBLLESRC/NBUWB.cbl:L345] | [QCBLLESRC/NBUWB.cbl:L351] | At most five riders |
| `NB-502` | `1500-VALIDATE-RIDERS` | [QCBLLESRC/NBUWB.cbl:L358] | Accidental-death rider, age cap |
| `NB-503` | `1500-VALIDATE-RIDERS` | [QCBLLESRC/NBUWB.cbl:L366] | Waiver-of-premium rider, permitted age range |
| `NB-504` | `1500-VALIDATE-RIDERS` | [QCBLLESRC/NBUWB.cbl:L374] | Critical-illness rider, sum-assured cap |
| `NB-601` | `1600-CALCULATE-BASE-PREMIUM` [QCBLLESRC/NBUWB.cbl:L388] | banner only [QCBLLESRC/NBUWB.cbl:L386] | Base annual premium computation |
| `NB-701` | `1700-CALCULATE-RIDER-PREMIUM` [QCBLLESRC/NBUWB.cbl:L405] | [QCBLLESRC/NBUWB.cbl:L411] | Accidental-death rider rate per thousand |
| `NB-702` | `1700-CALCULATE-RIDER-PREMIUM` | [QCBLLESRC/NBUWB.cbl:L417] | Waiver-of-premium rider as a percentage of base annual premium |
| `NB-703` | `1700-CALCULATE-RIDER-PREMIUM` | [QCBLLESRC/NBUWB.cbl:L422] | Critical-illness rider rate per thousand |
| `NB-801` | `1800-CALCULATE-TOTAL-PREMIUM` [QCBLLESRC/NBUWB.cbl:L436] | [QCBLLESRC/NBUWB.cbl:L437] | Gross annual premium as base plus riders plus policy fee |
| `NB-802` | `1800-CALCULATE-TOTAL-PREMIUM` | [QCBLLESRC/NBUWB.cbl:L447] | Modal premium and its loading factors |
| `NB-901` | `1900-EVALUATE-REFERRALS` [QCBLLESRC/NBUWB.cbl:L469] | [QCBLLESRC/NBUWB.cbl:L470] | Reinsurance referral on a sum-assured threshold |
| `NB-902` | `1900-EVALUATE-REFERRALS` | [QCBLLESRC/NBUWB.cbl:L474] | Manual underwriting triggers |
| `NB-1001` | `2000-ISSUE-POLICY` [QCBLLESRC/NBUWB.cbl:L484] | banner only [QCBLLESRC/NBUWB.cbl:L482] | Issuing the policy and setting the contract status |

### The `SV-` band: servicing and billing, 16 identifiers

| Identifier | Group paragraph in `QCBLLESRC/SVCBILB.cbl` | Definition site | What it governs |
|---|---|---|---|
| `SV-101` | `1100-LOAD-PLAN-PARAMETERS` [QCBLLESRC/SVCBILB.cbl:L146] | banner only [QCBLLESRC/SVCBILB.cbl:L144] | Loading the per-plan parameter set |
| `SV-201` | `1300-EVALUATE-PAYMENT-STATUS` [QCBLLESRC/SVCBILB.cbl:L196] | [QCBLLESRC/SVCBILB.cbl:L200] | Grace-period transition |
| `SV-202` | `1300-EVALUATE-PAYMENT-STATUS` | [QCBLLESRC/SVCBILB.cbl:L211] | Outstanding premium when a policy is overdue |
| `SV-301` | `1400-VALIDATE-SERVICING-REQUEST` [QCBLLESRC/SVCBILB.cbl:L219] | [QCBLLESRC/SVCBILB.cbl:L220] | A claimed or terminated contract cannot be serviced |
| `SV-302` | `1400-VALIDATE-SERVICING-REQUEST` | [QCBLLESRC/SVCBILB.cbl:L227] | An amendment type is required |
| `SV-401` | `2100-CHANGE-PLAN` [QCBLLESRC/SVCBILB.cbl:L237] | banner only [QCBLLESRC/SVCBILB.cbl:L235] | Plan change, named as the lower bound of its paragraph's range |
| `SV-403` | `2100-CHANGE-PLAN` | banner only [QCBLLESRC/SVCBILB.cbl:L235] | Plan change, named as the upper bound of the same range |
| `SV-501` | `2200-CHANGE-SUM-ASSURED` [QCBLLESRC/SVCBILB.cbl:L275] | banner only [QCBLLESRC/SVCBILB.cbl:L273] | Sum-assured change |
| `SV-502` | `2200-CHANGE-SUM-ASSURED` | [QCBLLESRC/SVCBILB.cbl:L284] | An increase beyond a proportional or absolute threshold requires underwriting |
| `SV-601` | `2300-CHANGE-BILLING-MODE` [QCBLLESRC/SVCBILB.cbl:L308] | banner only [QCBLLESRC/SVCBILB.cbl:L306] | Billing-mode change |
| `SV-701` | `2400-ADD-RIDER` [QCBLLESRC/SVCBILB.cbl:L329] | banner only [QCBLLESRC/SVCBILB.cbl:L327] | Adding a rider |
| `SV-702` | `2400-ADD-RIDER` | [QCBLLESRC/SVCBILB.cbl:L344] | Accidental-death rider not permitted above the age cap |
| `SV-801` | `2500-REMOVE-RIDER` [QCBLLESRC/SVCBILB.cbl:L371] | banner only [QCBLLESRC/SVCBILB.cbl:L369] | Removing a rider |
| `SV-901` | `2600-PROCESS-REINSTATEMENT` [QCBLLESRC/SVCBILB.cbl:L393] | [QCBLLESRC/SVCBILB.cbl:L394] | Only lapsed policies may be reinstated |
| `SV-902` | `2600-PROCESS-REINSTATEMENT` | [QCBLLESRC/SVCBILB.cbl:L410] | Outstanding premium plus the reinstatement fee |
| `SV-1001` | `3100-REPRICE-POLICY` [QCBLLESRC/SVCBILB.cbl:L422] | banner only [QCBLLESRC/SVCBILB.cbl:L420] | Repricing the policy after an amendment |

### The `CL-` band: claims, 15 identifiers

| Identifier | Group paragraph in `QCBLLESRC/CLMADJB.cbl` | Definition site | What it governs |
|---|---|---|---|
| `CL-101` | `1100-LOAD-PLAN-PARAMETERS` [QCBLLESRC/CLMADJB.cbl:L142] | banner only [QCBLLESRC/CLMADJB.cbl:L140] | Loading the per-plan parameter set |
| `CL-201` | `1200-VALIDATE-CLAIM-INTAKE` [QCBLLESRC/CLMADJB.cbl:L164] | [QCBLLESRC/CLMADJB.cbl:L165] | Death claims only |
| `CL-202` | `1200-VALIDATE-CLAIM-INTAKE` | [QCBLLESRC/CLMADJB.cbl:L172] | The policy must be active or in grace |
| `CL-203` | `1200-VALIDATE-CLAIM-INTAKE` | [QCBLLESRC/CLMADJB.cbl:L179] | Core claim data required |
| `CL-204` | `1200-VALIDATE-CLAIM-INTAKE` | [QCBLLESRC/CLMADJB.cbl:L188] | Required documents |
| `CL-301` | `1300-DETERMINE-INVESTIGATION` [QCBLLESRC/CLMADJB.cbl:L201] | [QCBLLESRC/CLMADJB.cbl:L208] | Death within the contestability period |
| `CL-302` | `1300-DETERMINE-INVESTIGATION` | [QCBLLESRC/CLMADJB.cbl:L214] | Suspicious cause of death |
| `CL-303` | `1300-DETERMINE-INVESTIGATION` | [QCBLLESRC/CLMADJB.cbl:L220] | Accidental, homicide or unknown cause without medical records |
| `CL-401` | `1400-ADJUDICATE-COVERAGE` [QCBLLESRC/CLMADJB.cbl:L231] | [QCBLLESRC/CLMADJB.cbl:L232] | Suicide within the suicide window |
| `CL-402` | `1400-ADJUDICATE-COVERAGE` | [QCBLLESRC/CLMADJB.cbl:L245] | Death after the expiry date |
| `CL-501` | `1500-CALCULATE-SETTLEMENT` [QCBLLESRC/CLMADJB.cbl:L256] | [QCBLLESRC/CLMADJB.cbl:L257] | Base payout equals the sum assured |
| `CL-502` | `1500-CALCULATE-SETTLEMENT` | [QCBLLESRC/CLMADJB.cbl:L259] | Add the accidental-death rider sum assured for an accidental death |
| `CL-503` | `1500-CALCULATE-SETTLEMENT` | [QCBLLESRC/CLMADJB.cbl:L271] | Deduct the modal premium when the policy is in grace |
| `CL-504` | `1500-CALCULATE-SETTLEMENT` | [QCBLLESRC/CLMADJB.cbl:L275] | Deduct the outstanding loan balance |
| `CL-601` | `1600-SETTLE-CLAIM` [QCBLLESRC/CLMADJB.cbl:L288] | banner only [QCBLLESRC/CLMADJB.cbl:L286] | Settling the claim and writing the outcome |

### The numbering convention, derived rather than assumed

The identifiers look like `<hundreds group><sequence>`, and the meaning of the hundreds group is not documented anywhere. It is recoverable by measurement, and the measurement settles both of the band irregularities below in one step, so it is worth stating as a tested proposition rather than an impression.

Each hundreds group maps to **exactly one** paragraph — in all three programs, with no group spanning two paragraphs and no paragraph carrying two groups. The group number is the **ordinal position of that paragraph among the paragraphs the author chose to annotate**, counted in source order. The two-digit sequence then numbers the rules inside that paragraph.

| Program | Annotated paragraphs, in source order | Groups assigned |
|---|---|---|
| `QCBLLESRC/NBUWB.cbl` | `1100`, `1200`, `1300`, `1400`, `1500`, `1600`, `1700`, `1800`, `1900`, `2000` — spanning [QCBLLESRC/NBUWB.cbl:L144-L484] | 1 to 10 |
| `QCBLLESRC/SVCBILB.cbl` | `1100`, `1300`, `1400`, `2100`, `2200`, `2300`, `2400`, `2500`, `2600`, `3100` — spanning [QCBLLESRC/SVCBILB.cbl:L146-L422] | 1 to 10 |
| `QCBLLESRC/CLMADJB.cbl` | `1100`, `1200`, `1300`, `1400`, `1500`, `1600` — spanning [QCBLLESRC/CLMADJB.cbl:L142-L288] | 1 to 6 |

The proposition holds for all 26 annotated paragraphs. Note that the group tracks the annotation ordinal and not the paragraph number: in the batch servicing program the groups run 1, 2, 3 over paragraphs `1100`, `1300`, `1400` — skipping `1200` entirely — and then continue over the `2100` to `2600` amendment handlers and on to `3100`. A reader who assumed the group was the paragraph's second digit would be right in the new-business program and wrong in the other two.

Paragraphs carrying no rule identifier at all are as consequential as the annotated ones, because a rule implemented there has no band member to be anchored to:

| Program | Numbered paragraphs with no rule identifier |
|---|---|
| `QCBLLESRC/NBUWB.cbl` | `1000-INITIALIZE` [QCBLLESRC/NBUWB.cbl:L122], `9000-RETURN-ERROR` [QCBLLESRC/NBUWB.cbl:L504] |
| `QCBLLESRC/SVCBILB.cbl` | `1000-INITIALIZE` [QCBLLESRC/SVCBILB.cbl:L130], `1200-CALCULATE-ATTAINED-AGE` [QCBLLESRC/SVCBILB.cbl:L187], and the five repricing paragraphs `3110` to `3200` [QCBLLESRC/SVCBILB.cbl:L434-L543] |
| `QCBLLESRC/CLMADJB.cbl` | `1000-INITIALIZE` [QCBLLESRC/CLMADJB.cbl:L127], `9000-RETURN-ERROR` [QCBLLESRC/CLMADJB.cbl:L303], `9000-RETURN-PENDING` [QCBLLESRC/CLMADJB.cbl:L311] |

The servicing row is the significant one. The whole repricing calculation — the rating-factor ladder, the base annual premium, the rider premiums, the totals and the modal recalculation, five paragraphs across [QCBLLESRC/SVCBILB.cbl:L434-L543] — is covered by the single banner-only identifier `SV-1001` on the coordinator paragraph above it. One identifier that describes nothing stands in for the entire monetary computation on the servicing path, which is exactly the code a parity comparison cares most about.

### Two structural irregularities

Both are recorded as measurements. Neither is dispositioned here; disposition belongs to [the known defects and stubs register](07-known-defects-and-stubs.md). They are reported because an inventory keyed on these identifiers has to tolerate them, and because each one defeats a plausible extraction shortcut.

- **`SV-402` does not exist anywhere in the estate.** The servicing band runs `SV-401` then `SV-403`. Both are named only on the banner of `2100-CHANGE-PLAN`, which states them as the bounds of a range [QCBLLESRC/SVCBILB.cbl:L235], and that paragraph carries no definition site at all across its span [QCBLLESRC/SVCBILB.cbl:L237-L272]. So `SV-402` is implied by a range whose interior is never written down, and a search of every member in the estate finds no occurrence of it. It is the only unwritten range interior in the estate: every other range — for instance the six-rule range in new-business validation [QCBLLESRC/NBUWB.cbl:L195] — has a definition site for each of its interior members. The inventory records the three rules of `2100-CHANGE-PLAN` against their code spans and does not pretend that `SV-402` is a locatable identifier.
- **A paragraph banner that understates its own paragraph.** `1400-LOAD-RATE-FACTORS` declares a four-rule range on its banner [QCBLLESRC/NBUWB.cbl:L299], but the paragraph contains a fifth definition site, `NB-405`, at [QCBLLESRC/NBUWB.cbl:L334] — inside the paragraph, which runs to [QCBLLESRC/NBUWB.cbl:L340] before the next label [QCBLLESRC/NBUWB.cbl:L345]. An extraction that trusted banner ranges and expanded them would silently drop the underwriting-class rating factor, one of the five multipliers in the premium calculation. This is the reason the counting rule above scans for identifiers everywhere in the member rather than reading banners.

The two four-digit identifiers, `NB-1001` [QCBLLESRC/NBUWB.cbl:L482] and `SV-1001` [QCBLLESRC/SVCBILB.cbl:L420], are not anomalies once the convention is known: they are group **10** written in a scheme with no separator between group and sequence. Both are the tenth annotated paragraph of their program — policy issue in new business, repricing in servicing. The claims band has no four-digit member for the same reason: `CLMADJB` annotates only six paragraphs, so its groups stop at 6. The inventory keeps both identifiers verbatim rather than normalising them to `NB-10-01`, because the string in the source is the anchor and rewriting it would break the one link that makes it useful.

### Five of the eight programs carry no identifier at all

This is the qualitative fact that shapes the extraction problem, and unlike every count above it needs no threshold or convention to be exact. Inline rule identifiers exist **only** in the three batch programs. `NBUWMNT`, `SVCMNT`, `CLMMNT`, `POLMSTINQ` and `MAINMENU` carry **none** — not one occurrence between them.

That would be a tolerable gap if the five unanchored programs implemented different logic from the three anchored ones. They do not. [The current-state architecture](02-architecture-current-state.md) establishes that each business domain is implemented twice, and that in new business the online program repeats **nine** paragraph labels from the batch program with 221 of the online engine's 250 significant lines shared. The consequence for this inventory is direct and is the single largest extraction hazard in the estate:

- **The same rule is anchored on one path and unanchored on the other.** The rider-pricing rules are the clearest case. The batch program carries an identifier above each of the three rider codes [QCBLLESRC/NBUWB.cbl:L411-L427]; the online program prices the same three codes with the same literals and carries no comment at all [QCBLLESRC/NBUWMNT.cbl:L434-L445]. An inventory built by walking the 59 identifiers would record three rules and cite only the batch anchors, leaving the online implementation of those rules invisible to it.
- **A whole domain can be missed on one path.** The claims paths share no paragraph label at all, and the batch program carries a referral rule, `CL-303`, that the online path does not implement anywhere [QCBLLESRC/CLMADJB.cbl:L220-L226] against [QCBLLESRC/CLMMNT.cbl:L210-L226]. Anchoring by identifier alone would attribute that rule to the domain rather than to one of its two paths, and a comparison built on it would expect an answer the online path never produces.

The procedure below therefore treats the identifier set as a starting point and the paragraph structure as the frame, never the reverse. Every rule is bound to a paragraph in every program that implements it, whether or not that program names it.

### The outcome channel is anchored on the same three programs

One further measurement determines what an assertion can be made against, and it lines up with the identifier census exactly.

The shared outcome pair — the return code and return message declared in the shared data contract [QCPYSRC/POLDATA.cpy:L36-L37], whose role in the record layout is owned by [the current data model](04-data-model-current-state.md) — is written **only** by the three batch programs: [QCBLLESRC/NBUWB.cbl:L487-L488], [QCBLLESRC/NBUWB.cbl:L497-L498], [QCBLLESRC/NBUWB.cbl:L505-L506], [QCBLLESRC/SVCBILB.cbl:L107-L108], [QCBLLESRC/SVCBILB.cbl:L121-L122], [QCBLLESRC/CLMADJB.cbl:L298], [QCBLLESRC/CLMADJB.cbl:L304-L305] and [QCBLLESRC/CLMADJB.cbl:L312-L313]. The five interactive programs reference neither field even once. The online new-business program, for instance, moves its result code to a [display file](../reference/glossary-ibm-i.md#display-file) field instead [QCBLLESRC/NBUWMNT.cbl:L210].

So the programs whose rules are anchored are the same programs whose outcomes are observable in a record. For the five interactive programs an outcome is a screen field, which no record comparison can see. The inventory records the observable effect per anchor rather than per rule for exactly this reason, and the effect it records for an online anchor is a field of the record the program rewrites, or a screen field, whichever the code actually sets.

## Extracting the remaining 274

The procedure below is dependency-ordered: each step consumes the output of the one above it, and no step may be started on a rule whose predecessor step is incomplete. It reconciles the three sources named earlier, and it produces one inventory entry per rule per implementing path.

- **Enumerate the population.** Take every rule-table row in the corpus, keyed by walkthrough file and line, giving 333 rows. The row's Rule Name, Description, Implementation Details and Category carry over as the rule's descriptive fields, and its table-local `BR-nnn` value is retained only as provenance, never as a key.
- **Bind each row to a paragraph.** Rule tables follow the walkthrough's workflow sections, and each section corresponds to a paragraph or to a contiguous span of one. Use the paragraph inventory in [the current-state architecture](02-architecture-current-state.md) as the frame and confirm the binding against the member. A row that cannot be bound to a paragraph is carried forward as unlocated rather than forced.
- **Bind to an in-source identifier where one exists.** For each of the 59 identifiers, attach it to the rule its definition site describes, or to the rules of its paragraph where the identifier is banner-only. This is what raises the 59 from comments to keys.
- **Assign an identifier where none exists.** See the convention below.
- **Locate the code span and verify the prose against it.** Record the line range that implements the rule, and check the corpus description against the code before accepting it. This step is not optional and is not a formality: in the issue-age rule below, the corpus Implementation Details cell states that plans other than the two it names use an age range of 18 to 50 [.swm/nbuwb-new-business-and-underwriting-batch-processing.yatiwmqj.sw.md:L393], whereas the plan-parameter paragraph assigns that range to the third named plan [QCBLLESRC/NBUWB.cbl:L175-L176] and its catch-all branch assigns no ages at all, setting an invalid-plan error instead [QCBLLESRC/NBUWB.cbl:L189-L191]. The prose conflates a plan with the fallback. A test written from the prose would encode a rule the system does not have.
- **Record the observable effect.** A field mutation, a status transition, or a value in the shared outcome pair. Where the effect is a screen field rather than a record field, say so, because that determines whether the rule can be asserted on by comparing records at all.
- **Classify the path relationship.** Duplicated, divergent, single-path, or unlocated, per the four cases below.
- **Reconcile and close.** The entry count must equal 333 plus one additional entry for each divergent rule's second path, every one of the 59 in-source identifiers must appear on at least one entry, and every entry must carry either a source anchor or an explicit unlocated marker. A mismatch in any of the three is a defect in the inventory, not a tolerance.

### What every entry carries

| Field | Holds | Notes |
|---|---|---|
| `rule_id` | The inventory key | Either an in-source identifier verbatim, or an assigned one; never a corpus `BR-nnn` |
| `origin` | `in-source` or `assigned` | Makes the distinction explicit even in a flat export |
| `domain` | New business, servicing, claims, inquiry or session | Derived from the implementing member, not from the identifier band, so unanchored rules are classified the same way as anchored ones |
| `paragraph` | The paragraph label containing the rule | The unit a characterization assertion can address |
| `anchor` | Member path and line range, one per implementing path | Two anchors for a duplicated rule; `unlocated` where no code can be found |
| `provenance` | Walkthrough path, line, and table-local `BR-nnn` | Lets any entry be traced back to the prose it came from |
| `precondition` | The condition under which the rule fires | Taken from the code, corroborated against the prose |
| `effect` | The observable effect, and whether it lands in a record or on a screen | Recorded **per anchor**, because two paths implementing one rule can report the same decision through different channels |
| `divergence` | `none`, `latent` or `active` | Whether the paths' **decisions** differ, in the vocabulary of [the current-state architecture](02-architecture-current-state.md): `latent` means they agree on current data and would part company on a parameter change, `active` means they already differ |

### Assigning identifiers to the unanchored rules

Assigned identifiers must be stable, reproducible and impossible to mistake for an identifier that exists in source.

```text
in-source key    NB-202        three or four digits, band prefix, exists in a comment
assigned key     INV-0417      four digits, no band prefix, exists only in the inventory
```

- **Shape.** `INV-` followed by four digits. No band prefix, so no assigned key can ever be read as a `NB-`, `SV-` or `CL-` identifier, and the four-digit width keeps it distinguishable from the corpus `BR-nnn` form as well. The `origin` field states the same thing independently, so the distinction survives even if the shapes were ever confused.
- **Deterministic assignment.** Sequence numbers are allocated by a fixed traversal — corpus walkthrough filename ascending, then rule-table row order within the walkthrough — so re-running the extraction on the same inputs allocates the same key to the same rule. Nothing about the allocation depends on the order the work happens to be done in.
- **Immutability.** An allocated key is never reused and never renumbered. A rule that turns out to be a duplicate of another has its entry superseded with a pointer, not deleted, and a retired rule's key retires with it. Parity findings and defect reports cite these keys, so a renumbering would silently invalidate every reference made to them.
- **No key without a record.** A key is allocated only to a rule that exists in the 333-row population. The inventory does not invent rules, and it does not allocate keys to behaviour it merely suspects.

### The four cases that lose coverage if handled loosely

- **Duplicated logic — one key, two anchors.** Where a rule is implemented on both the online and the batch path with the same precondition and the same decision, it gets **one** key carrying **two** anchors. Two keys would double-count the rule and would report a parity comparison as covering two rules when it covers one. A difference in how the two paths then *report* that decision does not split the key, because the effect is recorded per anchor; only a difference in the decision itself does. The rider-pricing rules are the reference case: `NB-701` through `NB-703` anchored at [QCBLLESRC/NBUWB.cbl:L411-L427] and the same three rules anchored, unnamed, at [QCBLLESRC/NBUWMNT.cbl:L434-L445].
- **Divergent logic — two keys, one family.** Where the two paths genuinely differ, one key cannot carry one expected answer, so the rule gets **two** entries sharing a family reference, each with its own anchor, its own expected effect and a `divergence` value. The claims domain supplies both flavours: the batch-only medical-records referral is `active` divergence, since the paths already answer differently [QCBLLESRC/CLMADJB.cbl:L220-L226]; the contestability window is `latent`, since the online path compiles the window in [QCBLLESRC/CLMMNT.cbl:L214] while the batch path reads it from plan parameters [QCBLLESRC/CLMADJB.cbl:L206-L207] and every plan currently sets the same value. Collapsing a latent divergence into one entry is the more dangerous error of the two, because a comparison of current outputs would show no difference and would certify a rule that is not in fact single-valued.
- **Unanchored rules — assigned key, real anchor.** The 274 rules with no in-source identifier get an assigned key and a **measured** anchor: the line range of the code that implements them. Unanchored means unnamed, not unlocatable, and for four of the five silent programs the paragraph structure makes location straightforward. `MAINMENU` is the exception and is called out as such: with no numbered paragraph to bind to, its 67 rules bind to spans of the single driver paragraph, and the span boundaries are part of the inventory entry rather than derivable from a label.
- **Rules with no locatable code — recorded as unlocated.** Where corpus prose describes behaviour that cannot be found in any member, the entry is marked `unlocated` and carries no anchor. Fabricating an anchor to satisfy a completeness target would be the worst available outcome: it would pass the citation check, since the path and line range would resolve, and it would put a rule into the acceptance oracle that no code implements. Unlocated entries are counted and reported as a coverage gap, and they are the one class of entry that is allowed to remain open.

### Independent corroboration of domain coverage

The repository overview summarises the rules of each domain in hand-written prose [README.md:L248-L252], and because it was authored independently of the generated corpus it is a useful check that no whole area of behaviour has been missed. Its new-business paragraph names issue-age and sum-assured limits, the maturity-age cap, the occupation restrictions, underwriting-class determination, the rating factors, rider validation, modal loading and reinsurance referral [README.md:L248]; its servicing paragraph names the grace and lapse transitions, the reinstatement window, plan change, the underwriting threshold on a sum-assured increase, billing-mode change and rider addition or removal [README.md:L250]; its claims paragraph names death-only intake, the eligibility check, the contestability and suicide windows, the accidental-death payout, the grace and loan deductions, the settlement floor and the payment modes [README.md:L252].

Every item in those three sentences maps onto a band member enumerated above, and the mapping is exhaustive in that direction — the overview names nothing that has no identifier or paragraph to bind to. It is not exhaustive in the other direction, and cannot be: it is a summary, and it characterises new business as carrying "50+ rules" [README.md:L248], which is a narrative figure rather than a count. The authoritative counts are the ones in this document.

The plan parameters the rules test against are tabulated in the same overview [README.md:L62-L66], and the three plan codes and their issue-age ranges there agree with the parameter branches in the source [QCBLLESRC/NBUWB.cbl:L147-L148], [QCBLLESRC/NBUWB.cbl:L161-L162], [QCBLLESRC/NBUWB.cbl:L175-L176]. That agreement is what makes the overview table usable as fixture data.

## One rule carried end to end

The issue-age rule is traced in full below, from the prose that describes it to the assertion that would verify it. It is chosen because it exercises every part of the procedure at once: it is anchored in one program and unanchored in its twin, its corpus description is imprecise in a way only source verification catches, and its outcome reaches a record on one path and a screen on the other.

| Step | What it establishes | Evidence |
|---|---|---|
| Corpus prose | The rule exists and what it is said to do | A narrative sentence [.swm/nbuwb-new-business-and-underwriting-batch-processing.yatiwmqj.sw.md:L501] and a rule-table row giving its category as data validation, its result code as 12 and its message text [.swm/nbuwb-new-business-and-underwriting-batch-processing.yatiwmqj.sw.md:L393] |
| Provenance key | The corpus identifier, retained but unusable as a key | Table-local `BR-006`, one of the 15 rules corpus-wide sharing that string |
| In-source identifier | The rule is named in source | Definition site `NB-202` [QCBLLESRC/NBUWB.cbl:L230] |
| Paragraph and group | Where it sits, and which group it belongs to | `1200-VALIDATE-APPLICATION` [QCBLLESRC/NBUWB.cbl:L197], group 2, the second annotated paragraph of the program |
| Batch anchor | Precondition and immediate effect | [QCBLLESRC/NBUWB.cbl:L231-L237] |
| Online anchor | The same rule, unnamed | The same predicate and the same literals inside the online program's validation paragraph [QCBLLESRC/NBUWMNT.cbl:L306-L312], within `1200-VALIDATE-APPLICATION` [QCBLLESRC/NBUWMNT.cbl:L274] |
| Observable effect, batch | What a record comparison can see | The driver tests the result code after validation [QCBLLESRC/NBUWB.cbl:L95], the error paragraph copies it into the shared outcome pair and sets the contract status to rejected [QCBLLESRC/NBUWB.cbl:L504-L507], and the record is rewritten [QCBLLESRC/NBUWB.cbl:L97] |
| Observable effect, online | Why the two anchors need separate assertions | The online program moves the result code to a screen field [QCBLLESRC/NBUWMNT.cbl:L210]; it references neither element of the outcome pair anywhere, and the rejected contract status appears nowhere in the member |
| Fixture parameters | Inputs that exercise both sides of the boundary | The plan-parameter branches set the age bounds per plan [QCBLLESRC/NBUWB.cbl:L147-L148], [QCBLLESRC/NBUWB.cbl:L161-L162], [QCBLLESRC/NBUWB.cbl:L175-L176], corroborated by the product table [README.md:L62-L66] |
| Prose verification | Why the source is authoritative over the description | The corpus cell folds the third plan together with the catch-all branch [.swm/nbuwb-new-business-and-underwriting-batch-processing.yatiwmqj.sw.md:L393]; in source the third plan sets bounds of 18 and 50 [QCBLLESRC/NBUWB.cbl:L175-L176] while the catch-all sets no bounds and raises an invalid-plan error [QCBLLESRC/NBUWB.cbl:L189-L191] |

The rule as it appears in source, comment and predicate together — the definition-site form that the counting rules above are built on:

```cobol
      * NB-202: ISSUE AGE LIMITS
           IF PM-ISSUE-AGE < PM-MIN-ISSUE-AGE OR
              PM-ISSUE-AGE > PM-MAX-ISSUE-AGE
```

And the same rule as a single inventory entry, which is the form the assertion is written from:

| Field | Value |
|---|---|
| `rule_id` | `NB-202` |
| `origin` | `in-source` |
| `domain` | New business |
| `paragraph` | `1200-VALIDATE-APPLICATION` |
| `anchor` | [QCBLLESRC/NBUWB.cbl:L231-L237] and [QCBLLESRC/NBUWMNT.cbl:L306-L312] |
| `provenance` | [.swm/nbuwb-new-business-and-underwriting-batch-processing.yatiwmqj.sw.md:L393], table-local `BR-006` |
| `precondition` | Issue age below the plan minimum or above the plan maximum |
| `effect`, batch anchor | Return code 12, the message text, contract status set to rejected, record rewritten — all record-visible |
| `effect`, online anchor | Screen result field and screen message only; no record field is set by this path |
| `divergence` | `none` at the decision, since both paths reject the same inputs on the same bounds |
| `assertable_by` | Record comparison at the batch anchor; screen capture at the online anchor |

One key, two anchors, two effect records, one decision. That is the shape of the majority of this estate's rules, and getting it wrong in either direction is costly: two keys would report a comparison as covering twice the ground it does, and one effect record would assert a record outcome against a path that never writes one.

## The acceptance-oracle record format

The inventory is consumed as a table, one row per rule with repeating groups for anchors and effects. The column definition below is the contract between this document and the two documents that use it. It is a data definition and a worked illustration only — no test code, and no runnable code of any kind, appears in this document or is implied by it.

| Column | Type | Required | Notes |
|---|---|---|---|
| `rule_id` | text | always | An in-source identifier verbatim, or an assigned `INV-` key. Never a corpus `BR-nnn` |
| `origin` | `in-source` or `assigned` | always | Independent of the key's shape, so the distinction cannot be lost |
| `domain` | new business, servicing, claims, inquiry, session | always | From the implementing member, not from the identifier band |
| `paragraph` | text | always | The label, or a cited span of the driver paragraph where a program has no numbered paragraphs |
| `anchor` | repeating: member path plus line range | unless `unlocated` | One per implementing path |
| `provenance` | walkthrough path, line, table-local identifier | always | Traces the entry back to the prose it came from |
| `precondition` | text | always | Taken from the code, corroborated against the prose |
| `effect` | repeating, aligned to `anchor` | unless `unlocated` | Names each field the path sets and whether it is record-visible or screen-only |
| `divergence` | `none`, `latent`, `active` | always | Whether the paths' decisions differ, in the vocabulary of [the current-state architecture](02-architecture-current-state.md) |
| `assertable_by` | record comparison, screen capture, not assertable | always, per anchor | Determines which anchors a record-level comparison can cover at all |
| `family` | text | only when `divergence` is not `none` | Groups the two entries of a divergent rule so neither is read alone |

An illustrative record, in the flattened form a reviewer reads rather than the form a harness parses:

```text
NB-202 | in-source | new business | 1200-VALIDATE-APPLICATION | divergence none
  anchor QCBLLESRC/NBUWB.cbl L231-L237   effect record: code 12, message, status rejected, rewritten
  anchor QCBLLESRC/NBUWMNT.cbl L306-L312 effect screen: result field and message only
```

### Coverage target

**333 of 333** documented rules represented, each carrying a stable identifier and a source anchor — raising anchored coverage from the measured baseline of **59 of 333**, that is 18%, to 100%. The 274 rules that carry no in-source identifier gain an assigned key and a measured anchor; the 59 that do keep their identifier verbatim.

One exception is admitted and must be reported rather than absorbed: a rule whose prose describes behaviour that cannot be located in any member is recorded as `unlocated`, counted, and reported as an open coverage gap. An inventory that reached 333 of 333 by inventing anchors would satisfy every mechanical check in this documentation set and would be worthless, because the citation check confirms that a path and a line range resolve, not that they contain the rule claimed.

### Why this inventory is the gate

Three measured constraints make this document load-bearing rather than merely useful.

- **There is nothing to run.** The member register in [the system inventory](01-system-inventory.md) accounts for the entire estate, and none of its members is a test. There is no test-runner configuration anywhere in the repository, so no existing suite defines what correct behaviour is.
- **There is nothing to compile against, either.** As [the platform and support status document](03-platform-and-support-status.md) records, no command available off the platform can compile, bind or run LIFE400. A behavioural claim about this system cannot be settled by a build; it can only be settled by reading the source or by running the system on the platform it targets.
- **The rules are the specification.** With no tests and no off-platform execution, the anchored rule inventory is the only written behavioural specification that will exist for this estate. Everything downstream depends on it: [the characterization test strategy](../migration/05-characterization-test-strategy.md) builds its fixtures from these entries, and [parallel run and output parity](../migration/06-parallel-run-and-output-parity.md) asserts against these expected effects. The decision that no module is converted before its characterization suite passes against the legacy system is recorded once, in [MOD-ADR-008 on characterization tests first](../decisions/MOD-ADR-008-characterization-tests-first.md); the evidence above is what that record is decided on, and its reasoning is not restated here.

## Figures owned by other documents

This document owns the rule population and its distribution, the rule-table and category counts, the corpus identifier-reuse census, the inline identifier census in all three of its readings, the anchored and unanchored split, the identifier bands and their numbering convention, the extraction procedure and the oracle record format. Every other quantity in the assessment has exactly one owning document, and restating one here would create a second place for it to be wrong.

- The member register, per-member banner facts, line counts and estate size — [the system inventory](01-system-inventory.md).
- Layers, the call and dispatch graph, per-program file dependencies, the paragraph inventory and the online-to-batch duplication analysis, including the active and latent divergence classification this document reuses — [the current-state architecture](02-architecture-current-state.md).
- Platform support status and the consequence of the declared runtime baseline — [the platform and support status document](03-platform-and-support-status.md).
- Column inventories, field types, the contract's group and item counts, the [level-88 condition name](../reference/glossary-ibm-i.md#level-88-condition-name) domains and the persisted-versus-transient classification, including the role of the shared outcome pair in the record layout — [the current data model](04-data-model-current-state.md).
- Work-management object roles, the message vocabulary, the nightly schedule and the build sequence — [the operational model](06-operational-model.md).
- The disposition of every defect, stub, inert feature and anomaly observed above, including the unwritten range interior and the banner that understates its own paragraph — [the known defects and stubs register](07-known-defects-and-stubs.md).
- Fixture design, coverage targets by rule band and the conversion gate itself — [the characterization test strategy](../migration/05-characterization-test-strategy.md).
- Comparison rules, rounding tolerance and the parity exit criterion — [parallel run and output parity](../migration/06-parallel-run-and-output-parity.md).
- The mapping from each paragraph to a target operation, and the single implementation each duplicated pair collapses into — [the program-to-service map](../target-state/02-program-to-service-map.md).
- Requirement-to-document traceability and the inverse index from each member to the documents citing it — [the traceability matrix](../reference/traceability-matrix.md) and [the source citation index](../reference/source-citation-index.md).

## Source citations

Every member cited above, read as evidence and left unmodified. No member of the estate is annotated, reformatted or commented by this documentation set, and the machine-generated corpus under `.swm/` is cited as prior art only: none of its files is edited, and none of its generated markup is reproduced here.

- ILE COBOL, the three members carrying inline rule identifiers — `QCBLLESRC/NBUWB.cbl`, `QCBLLESRC/SVCBILB.cbl`, `QCBLLESRC/CLMADJB.cbl`
- ILE COBOL, the five members carrying none, cited to establish that absence and the outcome-channel asymmetry — `QCBLLESRC/NBUWMNT.cbl`, `QCBLLESRC/SVCMNT.cbl`, `QCBLLESRC/CLMMNT.cbl`, `QCBLLESRC/POLMSTINQ.cbl`, `QCBLLESRC/MAINMENU.cbl`
- Shared data contract, cited for the outcome pair every rule reports through — `QCPYSRC/POLDATA.cpy`
- Repository overview, cited for the independent domain summary and the plan parameter table — `README.md`
- Prior documentation corpus, cited for the rule population, the per-document counts and the rule prose, and never modified — `.swm/business-rules-statistics.md`, `.swm/mainmenu-menu-interaction-and-dispatch.xdjwx2vk.sw.md`, `.swm/nbuwmnt-new-business-maintenance.jngxvgpo.sw.md`, `.swm/nbuwb-new-business-and-underwriting-batch-processing.yatiwmqj.sw.md`, `.swm/svcmnt-policy-servicing-maintenance.um1zpijd.sw.md`, `.swm/svcbilb-batch-policy-servicing-and-amendments.hnvq1sdj.sw.md`, `.swm/clmmnt-interactive-claims-maintenance.fqoabtf9.sw.md`, `.swm/clmadjb-claim-adjudication.2q95z11f.sw.md`, `.swm/polmstinq-policy-master-inquiry.4dyvkwri.sw.md`, `.swm/changing-a-policyholders-insurance-plan.1ogi2aoz.sw.md`
