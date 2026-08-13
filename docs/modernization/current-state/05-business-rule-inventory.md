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

Three sources are reconciled, and each contributes something the other two cannot. The prior corpus contributes the rule population and the descriptive prose. The [source members](../reference/glossary-ibm-i.md#source-physical-file-and-source-member) contribute the anchors, the literals and the observable effects. The repository overview contributes an independent domain summary that was written by hand rather than generated, so it serves as a check that no whole domain has been missed.

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

All 333 come from the eight program walkthroughs. The ninth document is a scenario narrative and contributes **0** [.swm/business-rules-statistics.md:L6], so the rule population is exactly co-extensive with the eight ILE COBOL programs and nothing else. Nothing in the corpus describes a rule in the five [ILE CL](../reference/glossary-ibm-i.md#cl-control-language) members, and nothing describes a rule expressed only in [DDS](../reference/glossary-ibm-i.md#dds-data-description-specifications).

### The same total, derived a second way

The published counts were taken on trust by every earlier description of this system. They are independently reproducible, and reproducing them establishes what the corpus means by "a rule" — which the extraction procedure then depends on.

A rule in the corpus is one **row of a rule table**. The tables carry five columns, headed Rule ID, Category, Rule Name, Description and Implementation Details, for example [.swm/nbuwb-new-business-and-underwriting-batch-processing.yatiwmqj.sw.md:L386]. Counting rows across the corpus yields **333**, and the per-document breakdown agrees with the published table for **all nine** documents, not merely in total. The rows are spread over **73** rule tables: 14 in the online new-business walkthrough, 13 in the batch servicing walkthrough, 11 in the batch new-business walkthrough, 10 each in the menu and online servicing walkthroughs, 7 in the online claims walkthrough, 6 in the batch claims walkthrough, 2 in the inquiry walkthrough and none in the scenario document.

One measurement caveat matters for anyone re-running this count. A naive count of identifier occurrences rather than table rows returns 71 for the menu walkthrough against its 67 rows, because one rule table was emitted flattened onto a single physical line together with adjoining prose [.swm/mainmenu-menu-interaction-and-dispatch.xdjwx2vk.sw.md:L2301]. Row counting is correct; occurrence counting is not. This is the first of several places where a plausible pattern gives a wrong answer, and it is the reason every counting rule in this document is stated before it is used.

### What kind of rules they are

The Category column classifies all 333 rows — it is the second column of every rule table in the corpus [.swm/polmstinq-policy-master-inquiry.4dyvkwri.sw.md:L146], [.swm/clmadjb-claim-adjudication.2q95z11f.sw.md:L121], and the row counts below were taken over all nine walkthroughs against the corpus total of 333 [.swm/business-rules-statistics.md:L29]. The distribution is worth publishing because it predicts how each rule can be asserted on: a calculation rule yields a number to compare, a decision rule yields a branch outcome, and an output rule yields a written record.

| Category | Rows |
|---|---|
| Decision Making | 123 |
| Calculation | 85 |
| Data validation | 78 |
| Writing Output | 25 |
| Invoking a Service or a Process | 13 |
| Reading Input | 9 |

123 + 85 + 78 + 25 + 13 + 9 = **333**, a third independent reconciliation of the same total [.swm/business-rules-statistics.md:L29].

Two thirds of the population — the 208 rows classified as decision or calculation — turn on a value the code computes rather than on a field the user supplied, which is precisely the class of rule that cannot be verified by reading a screen and must be captured from the record the program writes.

### The corpus supplies no stable per-rule identifier

The corpus does assign identifiers, in the form `BR-nnn` [.swm/polmstinq-policy-master-inquiry.4dyvkwri.sw.md:L148]. They cannot be used as inventory keys, and the measurement showing why is unambiguous: numbering **restarts at `BR-001` in every table** — the same walkthrough opens two tables with that identifier, at [.swm/polmstinq-policy-master-inquiry.4dyvkwri.sw.md:L148] and again at [.swm/polmstinq-policy-master-inquiry.4dyvkwri.sw.md:L238] — so the identifiers are table-local rather than document-local, let alone corpus-wide. Only **13** distinct identifiers exist across all 333 rules, counted over all nine walkthroughs.

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

73 + 71 + 67 + 48 + 29 + 15 + 7 + 7 + 6 + 4 + 3 + 2 + 1 = **333** [.swm/business-rules-statistics.md:L29]. `BR-001` occurring exactly 73 times is the same 73 as the table count, which is what "one per table" means.

The consequence is structural rather than cosmetic. A parity report that cited `BR-006` would be ambiguous across 15 different rules, and a defect raised against `BR-001` would be ambiguous across 73. The inventory therefore cannot adopt the corpus identifiers; it needs its own, and it needs them stable, which is what the extraction procedure below is for.

## Where the rules are anchored in source

### The measured identifier census

Three of the eight programs carry inline rule identifiers in their comments — the `NB-` band in [QCBLLESRC/NBUWB.cbl:L198], the `SV-` band in [QCBLLESRC/SVCBILB.cbl:L200] and the `CL-` band in [QCBLLESRC/CLMADJB.cbl:L208]. The counts below are of the source as it stands in this repository, each row measured over the whole member it names.

| Program | Band | Unique identifiers | Raw occurrences | Definition sites | Measured over |
|---|---|---|---|---|---|
| `QCBLLESRC/NBUWB.cbl` | `NB-` | 28 | 42 | 25 | [QCBLLESRC/NBUWB.cbl:L1-L507] |
| `QCBLLESRC/SVCBILB.cbl` | `SV-` | 16 | 24 | 8 | [QCBLLESRC/SVCBILB.cbl:L1-L543] |
| `QCBLLESRC/CLMADJB.cbl` | `CL-` | 15 | 23 | 13 | [QCBLLESRC/CLMADJB.cbl:L1-L314] |
| `QCBLLESRC/NBUWMNT.cbl` | none | 0 | 0 | 0 | [QCBLLESRC/NBUWMNT.cbl:L1-L498] |
| `QCBLLESRC/SVCMNT.cbl` | none | 0 | 0 | 0 | [QCBLLESRC/SVCMNT.cbl:L1-L448] |
| `QCBLLESRC/CLMMNT.cbl` | none | 0 | 0 | 0 | [QCBLLESRC/CLMMNT.cbl:L1-L291] |
| `QCBLLESRC/POLMSTINQ.cbl` | none | 0 | 0 | 0 | [QCBLLESRC/POLMSTINQ.cbl:L1-L131] |
| `QCBLLESRC/MAINMENU.cbl` | none | 0 | 0 | 0 | [QCBLLESRC/MAINMENU.cbl:L1-L95] |
| **Total** | | **59** | **89** | **46** | the eight members above |

Every identifier in the estate lives in a comment — a line carrying an asterisk in column 7 [QCBLLESRC/NBUWB.cbl:L198], [QCBLLESRC/SVCBILB.cbl:L200], [QCBLLESRC/CLMADJB.cbl:L208]. Not one is a data name, a paragraph label or a literal, so no identifier is visible to the compiler and nothing in the build would notice if one were deleted, duplicated or misnumbered [README.md:L160-L176].

### Reconciling the published figure of 46

Earlier descriptions of this estate state **46** anchored identifiers, broken down as 25 in the batch new-business program [QCBLLESRC/NBUWB.cbl:L1-L507], 13 in the batch claims program [QCBLLESRC/CLMADJB.cbl:L1-L314] and 8 in the batch servicing program [QCBLLESRC/SVCBILB.cbl:L1-L543]. This document publishes **59** unique identifiers and **89** raw occurrences. Both figures are correct, and the difference is fully explained rather than merely noted.

**46 is the definition-site count.** Measured per band, definition sites number 25 for `NB-` [QCBLLESRC/NBUWB.cbl:L1-L507], 8 for `SV-` [QCBLLESRC/SVCBILB.cbl:L1-L543] and 13 for `CL-` [QCBLLESRC/CLMADJB.cbl:L1-L314] — matching the earlier breakdown band for band, exactly. So the earlier figure counts identifiers that carry a description at the line where the rule is implemented, and it counts them correctly. It simply counts a narrower thing than "identifiers the source names".

The 89 raw occurrences decompose without remainder:

| Occurrence kind | Count | Where it appears |
|---|---|---|
| Definition site | 46 | A comment line naming one rule and describing it, immediately above that rule's code |
| Paragraph-banner occurrence | 43 | The paragraph's comment banner, which names either a single rule or a range |
| **Total raw occurrences** | **89** | |

The 43 banner occurrences come from **26** banner lines: 17 banners state a range and therefore name two identifiers each [QCBLLESRC/NBUWB.cbl:L195], [QCBLLESRC/SVCBILB.cbl:L194], and 9 banners name a single identifier [QCBLLESRC/NBUWB.cbl:L142], [QCBLLESRC/SVCBILB.cbl:L306], giving 17 × 2 + 9 = 43. Adding the 46 definition sites gives 89.

The unique set follows from the same decomposition: 46 identifiers have a definition site, and a further **13** are named only on a banner and nowhere else, giving 59. Those 13 are `NB-101`, `NB-601`, `NB-1001`, `SV-101`, `SV-401`, `SV-403`, `SV-501`, `SV-601`, `SV-701`, `SV-801`, `SV-1001`, `CL-101` and `CL-601`. A banner-only identifier names a rule without describing it, so it locates the rule to a paragraph but not to a line.

The counting rule this document adopts, and the reason for it: **the unique identifier set, 59.** An inventory key has to exist for every rule the source names, including the thirteen the source names without describing. Adopting 46 would leave those thirteen rules with no key at all, and they include the whole of plan-parameter loading in all three domains and the whole of the plan-change and repricing logic in servicing — not marginal rules. The two narrower figures are published alongside so that neither can be mistaken for the other again.

### What that leaves unanchored

Anchoring is now arithmetic. 333 documented rules [.swm/business-rules-statistics.md:L29], 59 of them named by an identifier in source [QCBLLESRC/NBUWB.cbl:L1-L507], [QCBLLESRC/SVCBILB.cbl:L1-L543], [QCBLLESRC/CLMADJB.cbl:L1-L314]:

| Quantity | Value | Share of 333 |
|---|---|---|
| Rules named by an inline source identifier | 59 | 18% |
| Rules with no inline source identifier | 274 | 82% |

333 − 59 = **274** [.swm/business-rules-statistics.md:L29]. Earlier derived figures of 287 unanchored and 86% rest on the 46 count and are superseded by this recomputation; they are named here only so a reader meeting them elsewhere knows which figure replaced them and why.

The 82% is the size of the extraction job, and it is not distributed evenly. It is concentrated in the five programs that carry no identifier at all, which between them account for 207 of the 333 documented rules — 67 for the menu program [.swm/business-rules-statistics.md:L9], 64 for online new business [.swm/business-rules-statistics.md:L11], 43 for online servicing [.swm/business-rules-statistics.md:L14], 25 for online claims [.swm/business-rules-statistics.md:L8] and 8 for the inquiry program [.swm/business-rules-statistics.md:L12].

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

This is the qualitative fact that shapes the extraction problem, and unlike every count above it needs no threshold or convention to be exact. Inline rule identifiers exist **only** in the three batch programs [QCBLLESRC/NBUWB.cbl:L1-L507], [QCBLLESRC/SVCBILB.cbl:L1-L543], [QCBLLESRC/CLMADJB.cbl:L1-L314]. `NBUWMNT`, `SVCMNT`, `CLMMNT`, `POLMSTINQ` and `MAINMENU` carry **none** — not one occurrence between them, over every line of all five members: [QCBLLESRC/NBUWMNT.cbl:L1-L498], [QCBLLESRC/SVCMNT.cbl:L1-L448], [QCBLLESRC/CLMMNT.cbl:L1-L291], [QCBLLESRC/POLMSTINQ.cbl:L1-L131], [QCBLLESRC/MAINMENU.cbl:L1-L95].

That would be a tolerable gap if the five unanchored programs implemented different logic from the three anchored ones. They do not. [The current-state architecture](02-architecture-current-state.md) establishes that each business domain is implemented twice, and that in new business the online program repeats **nine** paragraph labels from the batch program with 221 of the online engine's 250 significant lines shared. The consequence for this inventory is direct and is the single largest extraction hazard in the estate:

- **The same rule is anchored on one path and unanchored on the other.** The rider-pricing rules are the clearest case. The batch program carries an identifier above each of the three rider codes [QCBLLESRC/NBUWB.cbl:L411-L427]; the online program prices the same three codes with the same literals and carries no comment at all [QCBLLESRC/NBUWMNT.cbl:L434-L445]. An inventory built by walking the 59 identifiers would record three rules and cite only the batch anchors, leaving the online implementation of those rules invisible to it.
- **A whole domain can be missed on one path.** The claims paths share no paragraph label at all, and the batch program carries a referral rule, `CL-303`, that the online path does not implement anywhere [QCBLLESRC/CLMADJB.cbl:L220-L226] against [QCBLLESRC/CLMMNT.cbl:L210-L226]. Anchoring by identifier alone would attribute that rule to the domain rather than to one of its two paths, and a comparison built on it would expect an answer the online path never produces.

The procedure below therefore treats the identifier set as a starting point and the paragraph structure as the frame, never the reverse. Every rule is bound to a paragraph in every program that implements it, whether or not that program names it.

### The outcome channel is anchored on the same three programs

One further measurement determines what an assertion can be made against, and it lines up with the identifier census exactly.

The shared outcome pair — the return code and return message declared in the shared data contract [QCPYSRC/POLDATA.cpy:L36-L37] — is written **only** by the three batch programs: [QCBLLESRC/NBUWB.cbl:L487-L488], [QCBLLESRC/NBUWB.cbl:L497-L498], [QCBLLESRC/NBUWB.cbl:L505-L506], [QCBLLESRC/SVCBILB.cbl:L107-L108], [QCBLLESRC/SVCBILB.cbl:L121-L122], [QCBLLESRC/CLMADJB.cbl:L298], [QCBLLESRC/CLMADJB.cbl:L304-L305] and [QCBLLESRC/CLMADJB.cbl:L312-L313]. The five interactive programs reference neither field even once. The online new-business program, for instance, moves its result code to a [display file](../reference/glossary-ibm-i.md#display-file) field instead [QCBLLESRC/NBUWMNT.cbl:L210].

### Writing a field into the record area is not the same as storing it in a column

That census says which programs write the pair. It does not say what a comparison of records would find afterwards, and the difference is the single most important constraint on this inventory. Two facts owned by [the current data model](04-data-model-current-state.md) settle it, and this document uses them without restating the map: the shared contract is a program-described record area of 979 bytes, the policy master's own record is 233, and the two agree item-for-column only for the first 44 bytes.

Three consequences follow for anything this inventory calls an effect.

- **A status transition is observable in the column it names.** The contract status sits inside the first 44 bytes, where the contract and the file agree, so a rule that moves a status value and rewrites — the rejected status the batch new-business program sets [QCBLLESRC/NBUWB.cbl:L507] — is visible in that column afterwards. Status effects are therefore assertable by column, and so is anything else the contract declares in its first seven items.
- **The outcome pair is not observable as an outcome.** The pair occupies bytes 45 to 146 of the record area, and on the policy master those bytes are the insured, benefit and premium columns rather than any outcome column: the code lands on the first two bytes of the insured name and the message runs from there through the first eight bytes of the annual premium. So a batch program that sets a return code of 12 and a message and then rewrites [QCBLLESRC/NBUWB.cbl:L504-L507], [QCBLLESRC/NBUWB.cbl:L97] does not leave a return code anywhere a comparison can read as a return code — it leaves the digits and the message text lying across a 102-byte region the file defines as something else. An assertion of the form "the record's return-code column equals 12" cannot be written, because there is no such column; an assertion that those bytes changed, and to what, can be.
- **Items beyond byte 233 do not reach the record at all.** Six of the contract's nine groups begin past the end of the policy master's record, so a rule whose only effect is a mutation in one of them has no record-visible effect whatever, on either path. The audit stamp is the clearest case and is where a rule inventory is most likely to over-claim.

So the programs whose rules are anchored are the same programs whose outcomes leave *some* trace in a record, but "record-visible" has to be decided per field and per byte range rather than per program. For the five interactive programs an outcome is a screen field, which no record comparison can see at all. The inventory therefore records the observable effect per anchor, and for each effect it records the channel — an aligned column, a byte range under a column of another name, a screen field, or nothing observable.

## Extracting the remaining 274

The procedure below is dependency-ordered: each step consumes the output of the one above it, and no step may be started on a rule whose predecessor step is incomplete. It reconciles the three sources named earlier, and it produces one inventory entry per rule per implementing path.

- **Enumerate the population.** Take every rule-table row in the corpus, keyed by walkthrough file and line, giving 333 rows [.swm/business-rules-statistics.md:L6-L14], [.swm/business-rules-statistics.md:L29] — a row being one line of a five-column rule table [.swm/polmstinq-policy-master-inquiry.4dyvkwri.sw.md:L146], [.swm/polmstinq-policy-master-inquiry.4dyvkwri.sw.md:L148]. The row's Rule Name, Description, Implementation Details and Category carry over as the rule's descriptive fields, and its table-local `BR-nnn` value is retained only as provenance, never as a key.
- **Bind each row to a paragraph.** Rule tables follow the walkthrough's workflow sections, and each section corresponds to a paragraph or to a contiguous span of one. Use the paragraph inventory in [the current-state architecture](02-architecture-current-state.md) as the frame and confirm the binding against the member. A row that cannot be bound to a paragraph is carried forward as unlocated rather than forced.
- **Bind to an in-source identifier where one exists.** For each of the 59 identifiers [QCBLLESRC/NBUWB.cbl:L1-L507], [QCBLLESRC/SVCBILB.cbl:L1-L543], [QCBLLESRC/CLMADJB.cbl:L1-L314], attach it to the rule its definition site describes [QCBLLESRC/NBUWB.cbl:L198], or to the rules of its paragraph where the identifier is banner-only [QCBLLESRC/NBUWB.cbl:L142]. This is what raises the 59 from comments to keys.
- **Assign an identifier where none exists.** See the convention below.
- **Locate the code span and verify the prose against it.** Record the line range that implements the rule, and check the corpus description against the code before accepting it. This step is not optional and is not a formality: in the issue-age rule below, the corpus Implementation Details cell states that plans other than the two it names use an age range of 18 to 50 [.swm/nbuwb-new-business-and-underwriting-batch-processing.yatiwmqj.sw.md:L393], whereas the plan-parameter paragraph assigns that range to the third named plan [QCBLLESRC/NBUWB.cbl:L175-L176] and its catch-all branch assigns no ages at all, setting an invalid-plan error instead [QCBLLESRC/NBUWB.cbl:L189-L191]. The prose conflates a plan with the fallback. A test written from the prose would encode a rule the system does not have.
- **Record the observable effect, and the channel it is observable through.** Name the item the code sets, then resolve what a comparison would see: an aligned column, a byte range the file defines under another column name, a screen field, or nothing observable. The section above establishes why the last three are all real cases and why "the outcome pair was set" is not by itself an observable effect.
- **Check the prerequisites before declaring the rule assertable.** Provenance, numeric representation and interval arithmetic each disqualify assertions that would otherwise look well formed. The three checks are specified below, after the four cases.
- **Classify the path relationship.** Duplicated, divergent, single-path, or unlocated, per the four cases below, and where the two paths differ record which divergence family it belongs to.
- **Reconcile and close.** The entry count must equal 333 [.swm/business-rules-statistics.md:L29] plus one additional entry for each divergent rule's second path, every one of the 59 in-source identifiers must appear on at least one entry [QCBLLESRC/NBUWB.cbl:L1-L507], [QCBLLESRC/SVCBILB.cbl:L1-L543], [QCBLLESRC/CLMADJB.cbl:L1-L314], and every entry must carry either a source anchor or an explicit unlocated marker. A mismatch in any of the three is a defect in the inventory, not a tolerance.

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
| `effect` | The observable effect and the channel it is observable through — aligned column, byte range under a column of another name, screen field, or nothing observable | Recorded **per anchor**, because two paths implementing one rule can report the same decision through different channels, and because whether an effect reaches a column at all is a property of byte position rather than of the item's name |
| `divergence` | `none`, `latent`, `active` or `inert` | Whether the paths' **decisions** differ, in the vocabulary of [the current-state architecture](02-architecture-current-state.md): `latent` means they agree on current data and would part company on a change the estate permits, `active` means they already differ, and `inert` means the source differs while the answer does not |
| `prerequisites` | Any provenance, numeric-representation or interval-arithmetic condition that limits what may be asserted | Empty for most rules; where it is not empty it constrains or removes the assertion, and the three checks are specified below |

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
- **Divergent logic — two keys, one family.** Where the two paths genuinely differ, one key cannot carry one expected answer, so the rule gets **two** entries sharing a family reference, each with its own anchor, its own expected effect and a `divergence` value. The claims domain supplies all three flavours on its own: the batch-only medical-records referral is `active`, since the paths already answer differently [QCBLLESRC/CLMADJB.cbl:L220-L226]; the contestability window is `latent`, since the online path compiles the window in [QCBLLESRC/CLMMNT.cbl:L214] while the batch path reads it from plan parameters [QCBLLESRC/CLMADJB.cbl:L206-L207] and every plan currently sets the same value; and the overwritten outcome code in the batch document check is `inert`, since the value it lands on is the one the online path sets directly [QCBLLESRC/CLMADJB.cbl:L192], [QCBLLESRC/CLMADJB.cbl:L195] against [QCBLLESRC/CLMMNT.cbl:L205]. Collapsing a latent divergence into one entry is the most dangerous of the three, because a comparison of current outputs would show no difference and would certify a rule that is not in fact single-valued; collapsing an inert one loses the fact that a documented outcome code is unreachable.
- **Unanchored rules — assigned key, real anchor.** The 274 rules with no in-source identifier get an assigned key and a **measured** anchor: the line range of the code that implements them, in the five members that name none of their own [QCBLLESRC/NBUWMNT.cbl:L1-L498], [QCBLLESRC/SVCMNT.cbl:L1-L448], [QCBLLESRC/CLMMNT.cbl:L1-L291], [QCBLLESRC/POLMSTINQ.cbl:L1-L131], [QCBLLESRC/MAINMENU.cbl:L1-L95]. Unanchored means unnamed, not unlocatable, and for four of the five silent programs the paragraph structure makes location straightforward. `MAINMENU` is the exception and is called out as such: with no numbered paragraph to bind to, its 67 rules bind to spans of the single driver paragraph, and the span boundaries are part of the inventory entry rather than derivable from a label.
- **Rules with no locatable code — recorded as unlocated.** Where corpus prose describes behaviour that cannot be found in any member — the 24 members enumerated in the repository's own structure diagram [README.md:L27-L55] — the entry is marked `unlocated` and carries no anchor. Fabricating an anchor to satisfy a completeness target would be the worst available outcome: it would pass the citation check, since the path and line range would resolve, and it would put a rule into the acceptance oracle that no code implements. Unlocated entries are counted and reported as a coverage gap, and they are the one class of entry that is allowed to remain open.

### The divergence families the four cases have to cover

The four cases say how a divergent rule is recorded. They do not say where divergence occurs, and an extraction that discovered divergences one at a time while walking rules would find the arithmetic ones and miss most of the rest. [The current-state architecture](02-architecture-current-state.md) owns the complete inventory — every divergence found by reading both bodies of each pair, with both sides cited and each classified active, latent or inert. This inventory consumes that table rather than re-deriving it, and no count from it is restated here so that the two documents cannot drift apart.

What matters for extraction is that the divergences fall into families, and that only one family is the kind an extraction naturally looks for.

- **Session-state carryover, in new business.** The batch program resets its accumulators, its outcome pair and both referral flags before validating [QCBLLESRC/NBUWB.cbl:L122-L139]; the online program has no such paragraph and re-enters its issue coordinator with the previous application's state still in place [QCBLLESRC/NBUWMNT.cbl:L166]. A stale outcome code short-circuits the next application, both referral flags latch once set, and the rider screen is written back only where a field is non-blank. Every entry in this family is a **sequence** property: a fixture that runs one transaction per invocation cannot exhibit it on either path, so the inventory must record for each affected rule that its expected effect holds only for the first transaction of a session.
- **Coverage gaps, in servicing.** Rules present on one path and absent on the other: the batch dispatcher accepts an unrecognised amendment type where the online path rejects it, the third plan's remaining-term refusal and the overdue-premium accrual exist only in batch, and the two paths reprice different sets of riders. Each of these is two entries sharing a family, and each is found only if the fixture exercises the specific amendment type or plan — which makes the fixture matrix, not the rule walk, the thing that determines whether the family is covered.
- **Reporting and persistence, in claims.** The two claims paths decide alike far more often than they report or persist alike: the batch path rewrites the master on four separate paths where the online path writes only after settlement, one path sets an explanatory message where the other leaves the message it found, the settled batch path never sets the outcome code its own return paragraphs set, and the audit stamp is written on every batch path but only on the settled online path. An assertion on the decision alone would pass for all of these; the inventory therefore records the message and the write as part of the effect, not as commentary on it.
- **Parameterised windows, across servicing and claims.** The reinstatement, contestability and suicide windows are compiled-in literals on the interactive path and plan parameters on the batch path, and every plan currently loads the value the literal already holds. These are the `latent` entries, and collapsing one into a single entry is the most dangerous error available here, because a comparison of current outputs would certify a rule that is not single-valued.
- **Differences that produce no difference.** One claims divergence is `inert`: the batch document check moves an outcome code and then overwrites it with the value the online path uses directly, so the paths agree and the overwritten code is unreachable in the estate. It earns an entry because a rule keyed on the overwritten code would otherwise be recorded as expected behaviour that nothing can produce.

### Three prerequisites that decide whether a rule is assertable at all

A rule can be located, anchored, described accurately and still not be assertable. Three conditions are checked before the `assertable_by` value is set, and each of the three disqualifies assertions that look well formed. None is a property of the rule's prose; all three are properties of the code and data the rule runs on.

- **Provenance — is every input the rule reads ever supplied?** The issue-age rules are the case that matters most, because they are the estate's most frequently read input and nothing writes them. `PM-ISSUE-AGE` [QCPYSRC/POLDATA.cpy:L58] is read at twenty-nine lines across five programs and is the receiving field of no statement in any member; the underwriting display file collects a date of birth [QDDSSRC/NBUWDSPF.dspf:L57] and no age; and the only program that reads that date-of-birth field moves it into the contract's own date of birth [QCBLLESRC/NBUWMNT.cbl:L115], which nothing then reads. The consequence for the inventory is precise: every rule that reads issue age can be asserted only against a fixture that plants the value in the record beforehand, and no rule may claim that the application derives it. The full analysis is owned by [the current data model](04-data-model-current-state.md).
- **Numeric representation — can the fields hold the values the rules compare?** Three cases are established and each changes an expected result. The plan-parameter branches move fourteen-digit literals into thirteen-digit fields [QCBLLESRC/NBUWB.cbl:L149-L150], [QCBLLESRC/NBUWB.cbl:L163-L164], [QCBLLESRC/NBUWB.cbl:L177-L178] declared `PIC 9(13)V99` [QCPYSRC/POLDATA.cpy:L42-L43], so the sum-assured bounds the validation rule tests against [QCBLLESRC/NBUWB.cbl:L239-L240] are not the bounds the product table publishes [README.md:L62-L66]. The reinsurance referral compares the sum assured with a literal larger than its own field can represent [QCBLLESRC/NBUWB.cbl:L471], so `NB-901` cannot fire. And the claim payment amount is unsigned [QCPYSRC/POLDATA.cpy:L169] while the settlement logic decrements it and then tests it for a negative value [QCBLLESRC/CLMADJB.cbl:L281], [QCBLLESRC/CLMMNT.cbl:L266]. An expected value taken from the prose in any of these three cases would encode a rule the system does not have; the expected value has to be the one the declared field arithmetic produces.
- **Interval arithmetic — is the quantity the rule tests the quantity its name claims?** Treated separately below, because it applies to a whole family of rules rather than to individual fields.

### Interval rules record intended behaviour and actual behaviour separately

Every rule in this estate whose precondition is an interval — the grace and lapse transitions, the reinstatement window, the attained-age calculation, and the contestability and suicide windows — is written as a subtraction of one eight-digit `YYYYMMDD` value from another, compared against a term in days or a term multiplied by 365. The grace evaluation is the reference form [QCBLLESRC/SVCBILB.cbl:L198-L199], and the receiving fields are plain numerics [QCBLLESRC/SVCBILB.cbl:L77-L78]. Nothing in the estate converts such a difference into elapsed days: as [the current-state architecture](02-architecture-current-state.md) records in its absence census, there is no date type in any file, no intrinsic date function and no date-arithmetic construct anywhere, and the boundary analysis is owned by [the current data model](04-data-model-current-state.md).

Two rules follow for the inventory, and they exist to stop a characterization suite from quietly correcting the system it is supposed to characterise.

- **Both readings are recorded, and the actual one is the expected value.** Each interval rule's entry carries the intended calendar behaviour — what the rule is evidently for — as description, and the arithmetic the code performs as its `precondition` and expected effect. Where the two diverge at a boundary, the entry says so in `prerequisites`. A grace rule described as "thirty days after the paid-to date" and asserted as thirty days would fail against the legacy system on most inputs and pass on some, which is the worst of both outcomes: an intermittent failure that looks like a fixture problem.
- **A fixture may not compute an expected interval with real dates.** Expected values are derived by performing the same integer subtraction the member performs, on the same eight-digit inputs. This is stated as a constraint on the inventory rather than left to the test author, because the natural way to write the fixture — take two dates, subtract them properly — produces expectations the legacy system does not meet, and a suite built that way would be rejected as failing when it is in fact the specification that is wrong.

### Independent corroboration of domain coverage

The repository overview summarises the rules of each domain in hand-written prose [README.md:L248-L252], and because it was authored independently of the generated corpus it is a useful check that no whole area of behaviour has been missed. Its new-business paragraph names issue-age and sum-assured limits, the maturity-age cap, the occupation restrictions, underwriting-class determination, the rating factors, rider validation, modal loading and reinsurance referral [README.md:L248]; its servicing paragraph names the grace and lapse transitions, the reinstatement window, plan change, the underwriting threshold on a sum-assured increase, billing-mode change and rider addition or removal [README.md:L250]; its claims paragraph names death-only intake, the eligibility check, the contestability and suicide windows, the accidental-death payout, the grace and loan deductions, the settlement floor and the payment modes [README.md:L252].

Every item in those three sentences binds to something locatable in the source, and in that direction the check passes: the overview names no behaviour that has no paragraph or line range to point at. What it does not do is map uniformly onto the identifier bands enumerated above, and three of its items are the reason. Each names behaviour that is real, reachable and commented in the source, and none of the three carries an identifier of its own.

- **The lapse transition**, named in the servicing sentence [README.md:L250]. It is a commented block, `* LAPSE TRANSITION` [QCBLLESRC/SVCBILB.cbl:L206], writing the lapsed status [QCBLLESRC/SVCBILB.cbl:L209]. It sits inside `1300-EVALUATE-PAYMENT-STATUS` [QCBLLESRC/SVCBILB.cbl:L196], whose banner declares the range `SV-201 THRU SV-202` [QCBLLESRC/SVCBILB.cbl:L194] — and it falls between those two definition sites [QCBLLESRC/SVCBILB.cbl:L200], [QCBLLESRC/SVCBILB.cbl:L211] without being either of them.
- **The settlement floor**, named in the claims sentence [README.md:L252]. `* FLOOR AT ZERO` [QCBLLESRC/CLMADJB.cbl:L280] clamps a negative payment amount back to zero [QCBLLESRC/CLMADJB.cbl:L281-L283]. It sits inside `1500-CALCULATE-SETTLEMENT` [QCBLLESRC/CLMADJB.cbl:L256], past the last definition site that its banner's `CL-501 THRU CL-504` range accounts for [QCBLLESRC/CLMADJB.cbl:L275].
- **The payment-mode default**, also named in the claims sentence [README.md:L252]. An unset mode is defaulted [QCBLLESRC/CLMADJB.cbl:L294-L296] inside `1600-SETTLE-CLAIM` [QCBLLESRC/CLMADJB.cbl:L288], whose banner names `CL-601` [QCBLLESRC/CLMADJB.cbl:L286] and whose body defines no identifier at all — `CL-601` is one of the 13 banner-only identifiers counted above.

So the corroboration runs to paragraphs and line ranges rather than uniformly to band identifiers, which is why the extraction procedure binds an identifier only **where one exists** and specifies the `anchor` field as a member path plus a line range. An identifier is available for 59 rules and no more; a paragraph and a line range are available for all of them. The check in the other direction cannot be exhaustive either, and does not need to be: the overview is a summary, and it characterises new business as carrying "50+ rules" [README.md:L248], which is a narrative figure rather than a count. The authoritative counts are the ones in this document.

The plan parameters the rules test against are tabulated in the same overview [README.md:L62-L66], and the three plan codes and their issue-age ranges there agree with the parameter branches in the source [QCBLLESRC/NBUWB.cbl:L147-L148], [QCBLLESRC/NBUWB.cbl:L161-L162], [QCBLLESRC/NBUWB.cbl:L175-L176]. That agreement is what makes the overview table usable as fixture data.

## One rule carried end to end

The issue-age rule is traced in full below, from the prose that describes it to the assertion that would verify it. It is chosen because it exercises every part of the procedure at once: it is anchored in one program and unanchored in its twin, its corpus description is imprecise in a way only source verification catches, its two paths report through different channels, and it fails the provenance prerequisite — the input it tests is read by five programs and written by none. That last property is the reason it is the right worked example rather than a badly chosen one: a rule can be perfectly located and still not be assertable end to end, and this is the estate's most-read input.

| Step | What it establishes | Evidence |
|---|---|---|
| Corpus prose | The rule exists and what it is said to do | A narrative sentence [.swm/nbuwb-new-business-and-underwriting-batch-processing.yatiwmqj.sw.md:L501] and a rule-table row giving its category as data validation, its result code as 12 and its message text [.swm/nbuwb-new-business-and-underwriting-batch-processing.yatiwmqj.sw.md:L393] |
| Provenance key | The corpus identifier, retained but unusable as a key | Table-local `BR-006`, one of the 15 rules corpus-wide sharing that string |
| In-source identifier | The rule is named in source | Definition site `NB-202` [QCBLLESRC/NBUWB.cbl:L230] |
| Paragraph and group | Where it sits, and which group it belongs to | `1200-VALIDATE-APPLICATION` [QCBLLESRC/NBUWB.cbl:L197], group 2, the second annotated paragraph of the program |
| Batch anchor | Precondition and immediate effect | [QCBLLESRC/NBUWB.cbl:L231-L237] |
| Online anchor | The same rule, unnamed | The same predicate and the same literals inside the online program's validation paragraph [QCBLLESRC/NBUWMNT.cbl:L306-L312], within `1200-VALIDATE-APPLICATION` [QCBLLESRC/NBUWMNT.cbl:L274] |
| Observable effect, batch | What a record comparison can see, resolved by byte position and not by field name | The driver tests the result code after validation [QCBLLESRC/NBUWB.cbl:L95], the error paragraph copies it into the shared outcome pair and sets the contract status to rejected [QCBLLESRC/NBUWB.cbl:L504-L507], and the record is rewritten [QCBLLESRC/NBUWB.cbl:L97]. Of those two writes only the status is observable in the column it names, because it lies inside the first 44 bytes where contract and file agree; the code and message land on bytes 45 to 146, which the file defines as the insured, benefit and leading premium columns |
| Observable effect, online | Why the two anchors need separate assertions | The online program moves the result code to a screen field [QCBLLESRC/NBUWMNT.cbl:L210]; it references neither element of the outcome pair anywhere, and the rejected contract status appears nowhere in the member. So the online path leaves no record trace of this rule at all, not merely a different one |
| Provenance prerequisite | Why neither anchor is assertable through the application | The input under test is never assigned. `PM-ISSUE-AGE` [QCPYSRC/POLDATA.cpy:L58] is the receiving field of no statement in any member, and the online path has no screen field that could supply it [QDDSSRC/NBUWDSPF.dspf:L57]. The rule can be exercised only against a fixture that plants the value in the policy-master record before the program runs |
| Fixture parameters | Inputs that exercise both sides of the boundary, and one that cannot be taken from the published table | The plan-parameter branches set the age bounds per plan [QCBLLESRC/NBUWB.cbl:L147-L148], [QCBLLESRC/NBUWB.cbl:L161-L162], [QCBLLESRC/NBUWB.cbl:L175-L176], corroborated by the product table [README.md:L62-L66]. The age bounds are three-digit fields and hold their literals exactly [QCPYSRC/POLDATA.cpy:L40-L41], so for this rule the published table is usable as fixture data — unlike the sum-assured bounds set in the same paragraph, whose literals exceed their fields |
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
| `effect`, batch anchor | Contract status set to rejected — observable in its own column. Return code 12 and the message text — written into bytes 45 to 146 of the record area, observable only as a change to the insured, benefit and leading premium columns, not as an outcome column |
| `effect`, online anchor | Screen result field and screen message only; no record field is set by this path |
| `divergence` | `none` at the decision, since both paths reject the same inputs on the same bounds |
| `prerequisites` | Provenance: `PM-ISSUE-AGE` is never assigned by any member, so the value under test must be planted in the record by the fixture |
| `assertable_by` | Batch anchor: record comparison on the status column, plus a byte-range comparison for the code and message. Online anchor: screen capture only |

One key, two anchors, two effect records, one decision, one prerequisite. That is the shape of the majority of this estate's rules, and getting any part of it wrong is costly in a different way each time: two keys would report a comparison as covering twice the ground it does; one effect record would assert a record outcome against a path that never writes one; naming the outcome pair as a column would produce an assertion that cannot be evaluated against the file; and omitting the prerequisite would produce a fixture that exercises the rule with an input the application has no way to set.

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
| `effect` | repeating, aligned to `anchor` | unless `unlocated` | Names each item the path sets **and the channel it is observable through**: aligned column, byte range under a column of another name, screen field, or nothing observable |
| `divergence` | `none`, `latent`, `active`, `inert` | always | Whether the paths' decisions differ, in the vocabulary of [the current-state architecture](02-architecture-current-state.md), which owns the complete divergence inventory |
| `prerequisites` | text, possibly empty | always present, often empty | Provenance, numeric-representation and interval-arithmetic conditions that limit the assertion. An empty value is an assertion that all three checks passed, not that they were skipped |
| `assertable_by` | column comparison, byte-range comparison, screen capture, not assertable | always, per anchor | Determines which anchors a record-level comparison can cover at all. `byte-range comparison` exists because an item written into the record area does not necessarily land in a column of the same meaning |
| `family` | text | only when `divergence` is not `none` | Groups the two entries of a divergent rule so neither is read alone, and names the divergence family so the fixture matrix can be checked against it |

An illustrative record, in the flattened form a reviewer reads rather than the form a harness parses:

```text
NB-202 | in-source | new business | 1200-VALIDATE-APPLICATION | divergence none
  prerequisites provenance: PM-ISSUE-AGE never assigned, fixture must plant it
  anchor QCBLLESRC/NBUWB.cbl L231-L237   effect column: status rejected
                                         effect bytes 45-146: code 12 and message, under insured/benefit columns
                                         assertable_by column comparison + byte-range comparison
  anchor QCBLLESRC/NBUWMNT.cbl L306-L312 effect screen: result field and message only
                                         assertable_by screen capture
```

### Coverage target

**333 of 333** documented rules represented [.swm/business-rules-statistics.md:L29], each carrying a stable identifier and a source anchor — raising anchored coverage from the measured baseline of **59 of 333**, that is 18% [QCBLLESRC/NBUWB.cbl:L1-L507], [QCBLLESRC/SVCBILB.cbl:L1-L543], [QCBLLESRC/CLMADJB.cbl:L1-L314], to 100%. The 274 rules that carry no in-source identifier gain an assigned key and a measured anchor; the 59 that do keep their identifier verbatim.

One exception is admitted and must be reported rather than absorbed: a rule whose prose describes behaviour that cannot be located in any member is recorded as `unlocated`, counted, and reported as an open coverage gap. An inventory that reached 333 of 333 by inventing anchors would satisfy every mechanical check in this documentation set and would be worthless, because the citation check confirms that a path and a line range resolve, not that they contain the rule claimed.

### Why this inventory is the gate

Three measured constraints make this document load-bearing rather than merely useful.

- **There is nothing to run.** The member register in [the system inventory](01-system-inventory.md) accounts for the entire estate, and none of its members is a test: the repository's structure diagram enumerates every directory and member it holds [README.md:L27-L55] and the documented build has eight steps with no test step among them [README.md:L120-L218]. There is no test-runner configuration anywhere in the repository, so no existing suite defines what correct behaviour is.
- **There is nothing to compile against, either.** As [the platform and support status document](03-platform-and-support-status.md) records, no command available off the platform can compile, bind or run LIFE400: every program declares the platform as its source computer [QCBLLESRC/NBUWB.cbl:L45], [QCBLLESRC/MAINMENU.cbl:L27], the build is platform-side [README.md:L120-L218], and the declared runtime baseline is an IBM release [README.md:L264]. A behavioural claim about this system cannot be settled by a build; it can only be settled by reading the source or by running the system on the platform it targets.
- **The rules are the specification.** With no tests and no off-platform execution, the anchored rule inventory is the only written behavioural specification that will exist for this estate — the corpus prose [.swm/business-rules-statistics.md:L29] plus the 59 identifiers the source itself names [QCBLLESRC/NBUWB.cbl:L1-L507], [QCBLLESRC/SVCBILB.cbl:L1-L543], [QCBLLESRC/CLMADJB.cbl:L1-L314]. Everything downstream depends on it: [the characterization test strategy](../migration/05-characterization-test-strategy.md) builds its fixtures from these entries, and [parallel run and output parity](../migration/06-parallel-run-and-output-parity.md) asserts against these expected effects. The decision that no module is converted before its characterization suite passes against the legacy system is recorded once, in [MOD-ADR-008 on characterization tests first](../decisions/MOD-ADR-008-characterization-tests-first.md); the evidence above is what that record is decided on, and its reasoning is not restated here.

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
- Shared data contract, cited for the outcome pair every rule reports through, for the plan-parameter and monetary field widths the numeric prerequisite turns on, and for the issue-age item that no member assigns — `QCPYSRC/POLDATA.cpy`
- DDS display file, cited to establish that the underwriting screen collects a date of birth and no age — `QDDSSRC/NBUWDSPF.dspf`
- Repository overview, cited for the independent domain summary and the plan parameter table — `README.md`
- Prior documentation corpus, cited for the rule population, the per-document counts and the rule prose, and never modified — `.swm/business-rules-statistics.md`, `.swm/mainmenu-menu-interaction-and-dispatch.xdjwx2vk.sw.md`, `.swm/nbuwmnt-new-business-maintenance.jngxvgpo.sw.md`, `.swm/nbuwb-new-business-and-underwriting-batch-processing.yatiwmqj.sw.md`, `.swm/svcmnt-policy-servicing-maintenance.um1zpijd.sw.md`, `.swm/svcbilb-batch-policy-servicing-and-amendments.hnvq1sdj.sw.md`, `.swm/clmmnt-interactive-claims-maintenance.fqoabtf9.sw.md`, `.swm/clmadjb-claim-adjudication.2q95z11f.sw.md`, `.swm/polmstinq-policy-master-inquiry.4dyvkwri.sw.md`, `.swm/changing-a-policyholders-insurance-plan.1ogi2aoz.sw.md`
