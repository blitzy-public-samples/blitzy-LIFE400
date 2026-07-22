# LIFE400 → Guidewire APD — Multi-Product Traceability Matrix

> **Deliverable #4** of *LIFE400 COBOL Product Model Distillation to Guidewire APD* (AAP §0.3.1, §0.4.1, §0.6.4). This document is the **audit proof of 100 % field coverage**: one unified matrix mapping **every** COBOL field/record and **every** standalone persisted DDS field to exactly one destination — either a concrete Guidewire APD JSON path (into `product/TermLife.json` or `editions/TermLife-BaseEdition.json`) **or** an explicit *Unmapped (runtime state)* entry routed to `unmapped-fields-report.md`. **Every field name appears exactly once — zero silent drops** (AAP §0.7.1).

- **Single product / mono-line:** `ProductCode` is **`TermLife`** on every row; the one line is `TermLifeLine` (AAP §0.3.2, §0.6.1).
- **Read-only provenance:** all legacy sources under `QCBLLESRC/`, `QCPYSRC/`, `QDDSSRC/` are cited but never modified (AAP §0.2.2, §0.7).
- **This `.md` is written alongside the bundle but is EXCLUDED from `TermLife-APD-Bundle.zip`** (AAP §0.2.2, §0.7.2).
- **APD paths & codes below are byte-identical** to the finalized `product/TermLife.json` and `editions/TermLife-BaseEdition.json` (declared in `depends_on_files`).

## Coverage-Count Reconciliation

The exactly-once guarantee is proven by the arithmetic below, verifiable against `QCPYSRC/POLDATA.cpy` L16–L175 and the three physical files `POLMST.pf`, `CLMPF.pf`, `SVCPF.pf`:

| Bucket | Count | Notes |
| --- | --- | --- |
| Copybook `WS-POLICY-MASTER-REC` elementary fields | **89** | level-10/15 items bearing a `PIC`; the 48 level-88 condition-names and the 9 level-05 group items are **not** counted (88s are value-domains of their parent `TypeKey`; `PM-RIDER-TABLE` group header and `PM-RIDER-IDX` index are structural, not elementary) |
| Standalone persisted DDS fields (no `PM-` working-storage equivalent) | **5** | `CLMPF.CLMSTS`, `SVCPF.SVCID`, `SVCPF.NEWMODP`, `SVCPF.SVCDATE`, `SVCPF.USERID` |
| **Total distinct logical fields** | **94** | 89 copybook + 5 standalone DDS |

| Disposition | Count | Breakdown |
| --- | --- | --- |
| **Mapped to APD model** | **37** | 14 line fields + 3 rider fields + 13 plan parameters + 7 classification typeLists |
| **Unmapped (runtime / transaction state)** | **57** | 52 copybook runtime fields + 5 standalone DDS fields |

**Reconciliation:** `37 mapped + 52 unmapped copybook = 89 copybook`; `89 copybook + 5 standalone DDS = 94 total`; `unmapped total = 52 + 5 = 57`; `37 + 57 = 94` → **100 % coverage**.

> **`POLID` is counted once.** `PM-POLICY-ID` [`QCPYSRC/POLDATA.cpy:L17`] is the policy identifier persisted as the primary key `POLMST.POLID` [`QDDSSRC/POLMST.pf:L16`] and **reused** as the foreign key `CLMPF.POLID` [`QDDSSRC/CLMPF.pf:L18`] and `SVCPF.POLID` [`QDDSSRC/SVCPF.pf:L18`]. The FK reuse is annotated on the `PM-POLICY-ID` row only and is **not** double-counted.

---

## Master Traceability Matrix

The matrix is presented in sections (mapped groups A–D first, then the runtime/transaction-state group E). Together the sections enumerate all **94** distinct logical fields, **each exactly once**. Every row resolves to a concrete APD JSON pointer-style path **or** to `Unmapped → unmapped-fields-report.md` — no blank or ambiguous cells.

### A · Line-level fields → `product/TermLife.json#/lines[0]/fields` (14; each row gives its concrete `fields[0]`…`fields[13]` index)

Distribution-channel and billing-frequency indicators are modeled as line-level metadata fields; insured attributes and policy monetary amounts are line fields (AAP §0.3.2). The `riskObjects[0]` (`InsuredLife`) `fields` array is empty in the product model, so no rows map to a risk-object field.

| COBOL Field / Record | COBOL PIC / DDS type | Source [file:line] | ProductCode | Entity / RiskObject | APD Mapping (JSON path + code) OR "Unmapped → report" |
| --- | --- | --- | --- | --- | --- |
| `PM-PLAN-CODE` | `X(05)` | [`QCPYSRC/POLDATA.cpy:L21`]; `PLANCD 5A` [POLMST.pf:L23] | `TermLife` | `Line:TermLifeLine` | `product/TermLife.json#/lines[0]/fields[0]` — code `planCode` (TypeKey → typeLists[0] `PlanCode`) |
| `PM-CONTRACT-STATUS` | `X(02)` | [`QCPYSRC/POLDATA.cpy:L22`]; `CNTRSTS 2A` [POLMST.pf:L25] | `TermLife` | `Line:TermLifeLine` | `product/TermLife.json#/lines[0]/fields[1]` — code `contractStatus` (TypeKey → typeLists[1] `ContractStatus`) |
| `PM-ISSUE-CHANNEL` | `X(02)` | [`QCPYSRC/POLDATA.cpy:L31`]; `ISSCHN 2A` [POLMST.pf:L27] | `TermLife` | `Line:TermLifeLine` | `product/TermLife.json#/lines[0]/fields[2]` — code `issueChannel` (TypeKey → typeLists[2] `IssueChannel`) |
| `PM-BILLING-MODE` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L78`]; `BILMODE 1A` [POLMST.pf:L56] | `TermLife` | `Line:TermLifeLine` | `product/TermLife.json#/lines[0]/fields[3]` — code `billingMode` (TypeKey → typeLists[6] `BillingMode`) |
| `PM-INSURED-NAME` | `X(40)` | [`QCPYSRC/POLDATA.cpy:L55`]; `INSNAME 40A` [POLMST.pf:L32] | `TermLife` | `Line:TermLifeLine` | `product/TermLife.json#/lines[0]/fields[4]` — code `insuredName` (String, length 40) |
| `PM-DATE-OF-BIRTH` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L57`]; `INSDOB 8S0` [POLMST.pf:L35] | `TermLife` | `Line:TermLifeLine` | `product/TermLife.json#/lines[0]/fields[5]` — code `dateOfBirth` (Date — never String) |
| `PM-ISSUE-AGE` | `9(03)` | [`QCPYSRC/POLDATA.cpy:L58`]; `ISSAGE 3S0` [POLMST.pf:L37] | `TermLife` | `Line:TermLifeLine` | `product/TermLife.json#/lines[0]/fields[6]` — code `issueAge` (Integer) |
| `PM-GENDER` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L60`]; `GENDER 1A` [POLMST.pf:L39] | `TermLife` | `Line:TermLifeLine` | `product/TermLife.json#/lines[0]/fields[7]` — code `gender` (TypeKey → typeLists[3] `Gender`) |
| `PM-SMOKER-STATUS` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L63`]; `SMOKER 1A` [POLMST.pf:L41] | `TermLife` | `Line:TermLifeLine` | `product/TermLife.json#/lines[0]/fields[8]` — code `smokerStatus` (TypeKey → typeLists[4] `SmokerStatus`) |
| `PM-UW-CLASS` | `X(02)` | [`QCPYSRC/POLDATA.cpy:L67`]; `UWCLAS 2A` [POLMST.pf:L45] | `TermLife` | `Line:TermLifeLine` | `product/TermLife.json#/lines[0]/fields[9]` — code `uwClass` (TypeKey → typeLists[5] `UWClass`) |
| `PM-OCCUPATION-CLASS` | `9(01)` | [`QCPYSRC/POLDATA.cpy:L66`]; `OCCLAS 1S0` [POLMST.pf:L43] | `TermLife` | `Line:TermLifeLine` | `product/TermLife.json#/lines[0]/fields[10]` — code `occupationClass` (Integer) |
| `PM-HIGH-RISK-AVOCATION` | `X(01)` Y/N | [`QCPYSRC/POLDATA.cpy:L72`]; `HIRAVOC 1A` [POLMST.pf:L47] | `TermLife` | `Line:TermLifeLine` | `product/TermLife.json#/lines[0]/fields[11]` — code `highRiskAvocation` (Boolean) |
| `PM-SUM-ASSURED` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L76`]; `SUMASSR 15S2` [POLMST.pf:L52] | `TermLife` | `Line:TermLifeLine` | `product/TermLife.json#/lines[0]/fields[12]` — code `sumAssured` (Money, precision 15 scale 2) |
| `PM-POLICY-LOAN-BALANCE` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L77`]; `LOANBAL 15S2` [POLMST.pf:L54] | `TermLife` | `Line:TermLifeLine` | `product/TermLife.json#/lines[0]/fields[13]` — code `policyLoanBalance` (Money, precision 15 scale 2) |

### B · Rider fields via `PM-RIDER-TABLE OCCURS 5` → coverage clauses on `InsuredLife` (3)

**`OCCURS` resolution (inline, AAP §0.6.3):** the single `PM-RIDER-TABLE OCCURS 5 TIMES` [`QCPYSRC/POLDATA.cpy:L88-L96`] resolves to the **rider coverage-clause set** on the `InsuredLife` risk object — the rider **code is the clause discriminator** (`ADB01`/`WOP01`/`CI001`) and the rider **sum-assured is a clause term** — **not** a standalone repeating risk object.

| COBOL Field / Record | COBOL PIC / DDS type | Source [file:line] | ProductCode | Entity / RiskObject | APD Mapping (JSON path + code) OR "Unmapped → report" |
| --- | --- | --- | --- | --- | --- |
| `PM-RIDER-CODE` | `X(05)` | [`QCPYSRC/POLDATA.cpy:L90`] | `TermLife` | `Clause:ADB01/WOP01/CI001` | `product/TermLife.json#/lines[0]/clauses[1]`, `product/TermLife.json#/lines[0]/clauses[2]`, `product/TermLife.json#/lines[0]/clauses[3]` — rider discriminator; codes `ADB01`/`WOP01`/`CI001`; domain typeList `product/TermLife.json#/lines[0]/typeLists[7]` (`RiderCode`) |
| `PM-RIDER-SUM-ASSURED` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L91`] | `TermLife` | `Clause:ADB01,CI001` | `product/TermLife.json#/lines[0]/clauses[1]/terms[0]` — term `riderSumAssured` (Money 15/2) on `ADB01`; also `product/TermLife.json#/lines[0]/clauses[3]/terms[0]` on `CI001` (maxValue 500000) |
| `PM-RIDER-STATUS` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L94`] | `TermLife` | `TypeList:RiderStatus` | `product/TermLife.json#/lines[0]/typeLists[8]` — code `RiderStatus` (A/R) |

### C · Plan parameters → `editions/TermLife-BaseEdition.json` plan Option-term option values (13)

Each plan parameter is an edition rule value carried on the base coverage clause's `plan` Option term, across the three options `opt1`/`opt2`/`opt3` (= plan codes `T1001`/`T2001`/`T6501`). Per-plan values are selected by the single `EVALUATE PM-PLAN-CODE` in [`QCBLLESRC/NBUWB.cbl:L145-L192`]. **Each row below enumerates the exact per-option target pointers (`options[0]`, `options[1]`, `options[2]`) rather than a range**, so every path is individually machine-resolvable. **Sum-assured exception:** the `minSumAssured` / `maxSumAssured` target values are README-backed and differ from the raw NBUWB `MOVE` literals (which exceed `Money(15,2)` capacity); those two rows record the NBUWB literal, the README value, and the current edition value, with the ambiguity disclosed in `product-discovery-summary.md` §9.

| COBOL Field / Record | COBOL PIC / DDS type | Source [file:line] | ProductCode | Entity / RiskObject | APD Mapping (JSON path + code) OR "Unmapped → report" |
| --- | --- | --- | --- | --- | --- |
| `PM-MIN-ISSUE-AGE` | `9(03)` | [`QCPYSRC/POLDATA.cpy:L40`]; values [`QCBLLESRC/NBUWB.cbl:L145-L192`] | `TermLife` | `Edition:plan option` | `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[0]/minIssueAge`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[1]/minIssueAge`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[2]/minIssueAge` — edition key `minIssueAge` |
| `PM-MAX-ISSUE-AGE` | `9(03)` | [`QCPYSRC/POLDATA.cpy:L41`]; values [`QCBLLESRC/NBUWB.cbl:L145-L192`] | `TermLife` | `Edition:plan option` | `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[0]/maxIssueAge`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[1]/maxIssueAge`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[2]/maxIssueAge` — edition key `maxIssueAge` |
| `PM-MIN-SUM-ASSURED` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L42`]; raw literal `10000000000000` [`QCBLLESRC/NBUWB.cbl:L149`]; README `10,000,000` [`README.md:L64-L66`] | `TermLife` | `Edition:plan option` | `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[0]/minSumAssured`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[1]/minSumAssured`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[2]/minSumAssured` — edition key `minSumAssured` = `10000000` (README-backed value, **not** the raw NBUWB literal `10000000000000`, which is 14 integer digits and exceeds APD `Money(15,2)` capacity; discrepancy factor 10^6; source ambiguity disclosed in `product-discovery-summary.md` §9) |
| `PM-MAX-SUM-ASSURED` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L43`]; raw literals `50000000000000`/`90000000000000`/`75000000000000` [`QCBLLESRC/NBUWB.cbl:L150,L164,L178`]; README `50,000,000,000`/`90,000,000,000`/`75,000,000,000` [`README.md:L64-L66`] | `TermLife` | `Edition:plan option` | `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[0]/maxSumAssured`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[1]/maxSumAssured`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[2]/maxSumAssured` — edition key `maxSumAssured` = `50000000000`/`90000000000`/`75000000000` (README-backed values, **not** the raw NBUWB literals `50000000000000`/`90000000000000`/`75000000000000`, each 14 integer digits exceeding APD `Money(15,2)` capacity; discrepancy factor 10^3; source ambiguity disclosed in `product-discovery-summary.md` §9) |
| `PM-TERM-YEARS` | `9(03)` | [`QCPYSRC/POLDATA.cpy:L44`]; values [`QCBLLESRC/NBUWB.cbl:L145-L192`] | `TermLife` | `Edition:plan option` | `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[0]/termYears`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[1]/termYears`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[2]/termYears` — edition key `termYears` |
| `PM-MATURITY-AGE` | `9(03)` | [`QCPYSRC/POLDATA.cpy:L45`]; values [`QCBLLESRC/NBUWB.cbl:L145-L192`] | `TermLife` | `Edition:plan option` | `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[0]/maturityAge`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[1]/maturityAge`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[2]/maturityAge` — edition key `maturityAge` |
| `PM-GRACE-DAYS` | `9(03)` | [`QCPYSRC/POLDATA.cpy:L46`]; values [`QCBLLESRC/NBUWB.cbl:L145-L192`] | `TermLife` | `Edition:plan option` | `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[0]/graceDays`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[1]/graceDays`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[2]/graceDays` — edition key `graceDays` |
| `PM-CONTESTABILITY-YRS` | `9(02)` | [`QCPYSRC/POLDATA.cpy:L47`]; values [`QCBLLESRC/NBUWB.cbl:L145-L192`] | `TermLife` | `Edition:plan option` | `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[0]/contestabilityYrs`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[1]/contestabilityYrs`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[2]/contestabilityYrs` — edition key `contestabilityYrs` |
| `PM-SUICIDE-YRS` | `9(02)` | [`QCPYSRC/POLDATA.cpy:L48`]; values [`QCBLLESRC/NBUWB.cbl:L145-L192`] | `TermLife` | `Edition:plan option` | `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[0]/suicideYrs`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[1]/suicideYrs`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[2]/suicideYrs` — edition key `suicideYrs` |
| `PM-REINSTATE-WINDOW` | `9(04)` | [`QCPYSRC/POLDATA.cpy:L49`]; values [`QCBLLESRC/NBUWB.cbl:L145-L192`] | `TermLife` | `Edition:plan option` | `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[0]/reinstateWindow`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[1]/reinstateWindow`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[2]/reinstateWindow` — edition key `reinstateWindow` |
| `PM-ANNUAL-POLICY-FEE` | `9(07)V99` | [`QCPYSRC/POLDATA.cpy:L50`]; values [`QCBLLESRC/NBUWB.cbl:L145-L192`] | `TermLife` | `Edition:plan option` | `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[0]/annualPolicyFee`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[1]/annualPolicyFee`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[2]/annualPolicyFee` — edition key `annualPolicyFee` |
| `PM-SERVICE-FEE` | `9(07)V99` | [`QCPYSRC/POLDATA.cpy:L51`]; values [`QCBLLESRC/NBUWB.cbl:L145-L192`] | `TermLife` | `Edition:plan option` | `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[0]/serviceFee`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[1]/serviceFee`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[2]/serviceFee` — edition key `serviceFee` |
| `PM-TAX-RATE` | `9(02)V9999` | [`QCPYSRC/POLDATA.cpy:L52`]; values [`QCBLLESRC/NBUWB.cbl:L145-L192`] | `TermLife` | `Edition:plan option` | `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[0]/taxRate`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[1]/taxRate`, `editions/TermLife-BaseEdition.json#/rules/clauses[0]/terms[0]/options[2]/taxRate` — edition key `taxRate` |

### D · Classification / status / decision codes → `product/TermLife.json` typeLists (7)

For each field below, the **value domain** (its Level-88 condition-name set) is product configuration and becomes a line-level `typeList`; the **per-transaction value** stored in the record is runtime state. Only the value domain is mapped here (that is what makes the field one of the 37 mapped); the transactional value lives in the DDS records.

| COBOL Field / Record | COBOL PIC / DDS type | Source [file:line] | ProductCode | Entity / RiskObject | APD Mapping (JSON path + code) OR "Unmapped → report" |
| --- | --- | --- | --- | --- | --- |
| `PM-AMENDMENT-TYPE` | `X(02)` | [`QCPYSRC/POLDATA.cpy:L118`]; `AMDTYPE 2A` [SVCPF.pf:L21] | `TermLife` | `TypeList:AmendmentType` | `product/TermLife.json#/lines[0]/typeLists[9]` — code `AmendmentType` (domain PL/SA/BM/AR/RR/RI); value domain only, per-transaction value is runtime |
| `PM-AMENDMENT-STATUS` | `X(02)` | [`QCPYSRC/POLDATA.cpy:L133`]; `AMDSTS 2A` [SVCPF.pf:L23] | `TermLife` | `TypeList:AmendmentStatus` | `product/TermLife.json#/lines[0]/typeLists[10]` — code `AmendmentStatus` (domain PE/AP/RJ); value domain only, per-transaction value is runtime |
| `PM-CLAIM-TYPE` | `X(02)` | [`QCPYSRC/POLDATA.cpy:L140`]; `CLMTYPE 2A` [CLMPF.pf:L21] | `TermLife` | `TypeList:ClaimType` | `product/TermLife.json#/lines[0]/typeLists[11]` — code `ClaimType` (domain DT); value domain only, per-transaction value is runtime |
| `PM-CAUSE-OF-DEATH` | `X(03)` | [`QCPYSRC/POLDATA.cpy:L142`]; `CAUSDTH 3A` [CLMPF.pf:L23] | `TermLife` | `TypeList:CauseOfDeath` | `product/TermLife.json#/lines[0]/typeLists[12]` — code `CauseOfDeath` (domain NAT/ACC/SUI/HOM/UNK); value domain only, per-transaction value is runtime |
| `PM-CLAIM-PAYMENT-MODE` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L158`]; `PYMTMODE 1A` [CLMPF.pf:L54] | `TermLife` | `TypeList:ClaimPaymentMode` | `product/TermLife.json#/lines[0]/typeLists[13]` — code `ClaimPaymentMode` (domain C/A); value domain only, per-transaction value is runtime |
| `PM-INVESTIGATION-STATUS` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L161`]; `INVSTS 1A` [CLMPF.pf:L47] | `TermLife` | `TypeList:InvestigationStatus` | `product/TermLife.json#/lines[0]/typeLists[14]` — code `InvestigationStatus` (domain N/P/C); value domain only, per-transaction value is runtime |
| `PM-CLAIM-DECISION` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L165`]; `CLMDEC 1A` [CLMPF.pf:L45] | `TermLife` | `TypeList:ClaimDecision` | `product/TermLife.json#/lines[0]/typeLists[15]` — code `ClaimDecision` (domain A/R/P); value domain only, per-transaction value is runtime |

### E · Unmapped — runtime / transaction state (57)

Every field below is per-policy or per-transaction runtime state (identifiers, computed rating/premium results, lifecycle dates, before/after servicing images, claim processing values, audit artifacts). None defines a coverage, term, limit, eligibility rule, or classification domain, so each is routed to `unmapped-fields-report.md` with an explicit justification. This is what keeps the distillation behavior-preserving — all rating and lifecycle computation stays in the COBOL (AAP §0.6.2).

#### Control / Processing state — group `PM-CONTROL-AREA` [L16] (6)

| COBOL Field / Record | COBOL PIC / DDS type | Source [file:line] | ProductCode | Entity / RiskObject | APD Mapping (JSON path + code) OR "Unmapped → report" |
| --- | --- | --- | --- | --- | --- |
| `PM-POLICY-ID` | `X(12)` | [`QCPYSRC/POLDATA.cpy:L17`]; PK `POLMST.POLID` [POLMST.pf:L16]; FK `CLMPF.POLID` [CLMPF.pf:L18]; FK `SVCPF.POLID` [SVCPF.pf:L18] | `TermLife` | `— (instance id)` | Unmapped → `unmapped-fields-report.md` — policy instance identifier; FK reuse in CLMPF/SVCPF is the SAME logical field (counted once) |
| `PM-APPLICATION-ID` | `X(12)` | [`QCPYSRC/POLDATA.cpy:L18`]; `POLMST.APPID` [POLMST.pf:L18] | `TermLife` | `— (instance id)` | Unmapped → `unmapped-fields-report.md` — per-application instance identifier |
| `PM-PROCESS-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L20`]; `POLMST.PRCDATE` [POLMST.pf:L21] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-run processing date |
| `PM-CURRENCY-CODE` | `X(03)` | [`QCPYSRC/POLDATA.cpy:L35`]; `POLMST.CURCD` [POLMST.pf:L29] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-policy currency value; unconstrained 3-char input (`[QDDSSRC/NBUWDSPF.dspf:L46]`, moved at `[QCBLLESRC/NBUWMNT.cbl:L106]`) with no source-backed product domain — a runtime value, not a product-config choice |
| `PM-RETURN-CODE` | `9(02)` | [`QCPYSRC/POLDATA.cpy:L36`] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — program return/status code (working storage only) |
| `PM-RETURN-MESSAGE` | `X(100)` | [`QCPYSRC/POLDATA.cpy:L37`] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — program return message text (working storage only) |

#### Derived / rating-adjustment — group `PM-INSURED-DETAILS` [L54] (2)

| COBOL Field / Record | COBOL PIC / DDS type | Source [file:line] | ProductCode | Entity / RiskObject | APD Mapping (JSON path + code) OR "Unmapped → report" |
| --- | --- | --- | --- | --- | --- |
| `PM-ATTAINED-AGE` | `9(03)` | [`QCPYSRC/POLDATA.cpy:L59`] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — derived attained age, computed at runtime |
| `PM-FLAT-EXTRA-RATE` | `9(02)V9999` | [`QCPYSRC/POLDATA.cpy:L73`]; `POLMST.FLTXTRA 6S4` [POLMST.pf:L49] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-policy flat-extra rating adjustment |

#### Rating factors — group `PM-BENEFIT-DETAILS` [L75] (5) · rule source [`QCBLLESRC/NBUWB.cbl:L301-L340`]

| COBOL Field / Record | COBOL PIC / DDS type | Source [file:line] | ProductCode | Entity / RiskObject | APD Mapping (JSON path + code) OR "Unmapped → report" |
| --- | --- | --- | --- | --- | --- |
| `PM-BASE-MORTALITY-RATE` | `9(02)V9999` | [`QCPYSRC/POLDATA.cpy:L83`]; [NBUWB.cbl:L301-L340] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — rating factor (base mortality); stays in COBOL rating logic |
| `PM-GENDER-FACTOR` | `9(01)V9999` | [`QCPYSRC/POLDATA.cpy:L84`]; [NBUWB.cbl:L301-L340] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — rating factor (gender multiplier) |
| `PM-SMOKER-FACTOR` | `9(01)V9999` | [`QCPYSRC/POLDATA.cpy:L85`]; [NBUWB.cbl:L301-L340] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — rating factor (smoker multiplier) |
| `PM-OCCUPATION-FACTOR` | `9(01)V9999` | [`QCPYSRC/POLDATA.cpy:L86`]; [NBUWB.cbl:L301-L340] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — rating factor (occupation multiplier) |
| `PM-UW-FACTOR` | `9(01)V9999` | [`QCPYSRC/POLDATA.cpy:L87`]; [NBUWB.cbl:L301-L340] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — rating factor (UW-class multiplier) |

#### Rider rating / premium — `PM-RIDER-TABLE OCCURS 5` [L88-L96] (2)

| COBOL Field / Record | COBOL PIC / DDS type | Source [file:line] | ProductCode | Entity / RiskObject | APD Mapping (JSON path + code) OR "Unmapped → report" |
| --- | --- | --- | --- | --- | --- |
| `PM-RIDER-RATE` | `9(02)V9999` | [`QCPYSRC/POLDATA.cpy:L92`] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-rider rating rate; rating output, not product config |
| `PM-RIDER-ANNUAL-PREM` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L93`] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-rider computed annual premium |

#### Premium results — group `PM-PREMIUM-RESULTS` [L98] (8)

| COBOL Field / Record | COBOL PIC / DDS type | Source [file:line] | ProductCode | Entity / RiskObject | APD Mapping (JSON path + code) OR "Unmapped → report" |
| --- | --- | --- | --- | --- | --- |
| `PM-BASE-ANNUAL-PREMIUM` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L99`] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — computed premium result |
| `PM-RIDER-ANNUAL-TOTAL` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L100`] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — computed premium result |
| `PM-GROSS-ANNUAL-PREMIUM` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L101`] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — computed premium result |
| `PM-TAX-AMOUNT` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L102`] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — computed tax amount |
| `PM-TOTAL-ANNUAL-PREMIUM` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L103`]; `POLMST.ANPREM 15S2` [POLMST.pf:L59] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — computed total annual premium (persisted result) |
| `PM-MODAL-PREMIUM` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L104`]; `POLMST.MODPREM 15S2` [POLMST.pf:L61] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — computed modal premium (persisted result) |
| `PM-OUTSTANDING-PREMIUM` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L105`]; `POLMST.OUTPREM 15S2` [POLMST.pf:L63] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — outstanding premium balance (persisted result) |
| `PM-PREMIUM-DELTA` | `S9(13)V99` | [`QCPYSRC/POLDATA.cpy:L106`]; `SVCPF.PREMDLT 15S2` [SVCPF.pf:L45] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — servicing premium delta (signed) — transaction result |

#### Lifecycle dates — group `PM-DATE-DETAILS` [L108] (6)

| COBOL Field / Record | COBOL PIC / DDS type | Source [file:line] | ProductCode | Entity / RiskObject | APD Mapping (JSON path + code) OR "Unmapped → report" |
| --- | --- | --- | --- | --- | --- |
| `PM-ISSUE-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L110`]; `POLMST.ISSDATE 8S0` [POLMST.pf:L67] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-policy lifecycle date |
| `PM-EFFECTIVE-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L111`]; `POLMST.EFFDATE 8S0` [POLMST.pf:L69] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-policy lifecycle date |
| `PM-PAID-TO-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L112`]; `POLMST.PAIDTO 8S0` [POLMST.pf:L71] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-policy lifecycle date |
| `PM-EXPIRY-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L113`]; `POLMST.EXPDATE 8S0` [POLMST.pf:L73] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-policy lifecycle date |
| `PM-LAST-MAINT-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L114`]; `POLMST.LSTMNT 8S0` [POLMST.pf:L75] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-policy maintenance date |
| `PM-DATE-OF-DEATH` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L115`]; `CLMPF.DTHDTC 8S0` [CLMPF.pf:L26] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-claim date of death |

#### Servicing transaction — group `PM-SERVICING-DETAILS` [L117] (8)

| COBOL Field / Record | COBOL PIC / DDS type | Source [file:line] | ProductCode | Entity / RiskObject | APD Mapping (JSON path + code) OR "Unmapped → report" |
| --- | --- | --- | --- | --- | --- |
| `PM-OLD-PLAN-CODE` | `X(05)` | [`QCPYSRC/POLDATA.cpy:L125`]; `SVCPF.OLDPLAN 5A` [SVCPF.pf:L28] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — servicing before-image |
| `PM-NEW-PLAN-CODE` | `X(05)` | [`QCPYSRC/POLDATA.cpy:L126`]; `SVCPF.NEWPLAN 5A` [SVCPF.pf:L30] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — servicing after-image |
| `PM-OLD-SUM-ASSURED` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L127`]; `SVCPF.OLDSA 15S2` [SVCPF.pf:L33] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — servicing before-image |
| `PM-NEW-SUM-ASSURED` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L128`]; `SVCPF.NEWSA 15S2` [SVCPF.pf:L35] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — servicing after-image |
| `PM-OLD-BILLING-MODE` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L129`]; `SVCPF.OLDBM 1A` [SVCPF.pf:L38] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — servicing before-image |
| `PM-NEW-BILLING-MODE` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L130`]; `SVCPF.NEWBM 1A` [SVCPF.pf:L40] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — servicing after-image |
| `PM-SERVICE-FEE-CHARGED` | `9(07)V99` | [`QCPYSRC/POLDATA.cpy:L131`]; `SVCPF.SVCFEE 9S2` [SVCPF.pf:L25] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-transaction fee charged (result) |
| `PM-UW-REQUIRED` | `X(01)` Y/N | [`QCPYSRC/POLDATA.cpy:L132`] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — runtime servicing flag — AAP §0.4.2's Boolean *example*, but it is transaction state, NOT a product field (the product Boolean is `highRiskAvocation`) |

#### Claim transaction — group `PM-CLAIM-DETAILS` [L138] (13)

| COBOL Field / Record | COBOL PIC / DDS type | Source [file:line] | ProductCode | Entity / RiskObject | APD Mapping (JSON path + code) OR "Unmapped → report" |
| --- | --- | --- | --- | --- | --- |
| `PM-CLAIM-ID` | `X(12)` | [`QCPYSRC/POLDATA.cpy:L139`]; PK `CLMPF.CLMID` [CLMPF.pf:L16] | `TermLife` | `— (instance id)` | Unmapped → `unmapped-fields-report.md` — claim instance identifier |
| `PM-DEATH-CERT-RECD` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L148`]; `CLMPF.DTHCERT 1A` [CLMPF.pf:L29] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-claim document-received flag |
| `PM-CLAIM-FORM-RECD` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L149`]; `CLMPF.CLMFORM 1A` [CLMPF.pf:L31] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-claim document-received flag |
| `PM-ID-PROOF-RECD` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L150`]; `CLMPF.IDPROOF 1A` [CLMPF.pf:L33] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-claim document-received flag |
| `PM-MEDICAL-RECORDS-RECD` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L151`]; `CLMPF.MEDRECS 1A` [CLMPF.pf:L35] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-claim document-received flag |
| `PM-CLAIM-SUBMIT-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L152`]; `CLMPF.CLMDATE 8S0` [CLMPF.pf:L58] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-claim lifecycle date |
| `PM-CLAIM-INVEST-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L153`]; `CLMPF.INVDATE 8S0` [CLMPF.pf:L60] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-claim lifecycle date |
| `PM-CLAIM-ADJUDIC-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L154`]; `CLMPF.ADJDATE 8S0` [CLMPF.pf:L62] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-claim lifecycle date |
| `PM-CLAIM-SETTLE-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L155`]; `CLMPF.SETDATE 8S0` [CLMPF.pf:L64] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-claim lifecycle date |
| `PM-BENEFICIARY-NAME` | `X(40)` | [`QCPYSRC/POLDATA.cpy:L156`]; `CLMPF.BENNAME 40A` [CLMPF.pf:L38] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-claim beneficiary value |
| `PM-BENEFICIARY-RELATION` | `X(20)` | [`QCPYSRC/POLDATA.cpy:L157`]; `CLMPF.BENREL 20A` [CLMPF.pf:L40] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-claim beneficiary value |
| `PM-CLAIM-PAYMENT-AMT` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L169`]; `CLMPF.PYMTAMT 15S2` [CLMPF.pf:L52] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-claim payment amount (result) |
| `PM-CLAIM-HOLD-REASON` | `X(50)` | [`QCPYSRC/POLDATA.cpy:L170`]; `CLMPF.CLMHOLD 50A` [CLMPF.pf:L49] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — per-claim hold-reason text |

#### Audit — group `PM-AUDIT-DETAILS` [L172] (2)

| COBOL Field / Record | COBOL PIC / DDS type | Source [file:line] | ProductCode | Entity / RiskObject | APD Mapping (JSON path + code) OR "Unmapped → report" |
| --- | --- | --- | --- | --- | --- |
| `PM-LAST-ACTION-USER` | `X(10)` | [`QCPYSRC/POLDATA.cpy:L173`]; `POLMST.LSTUSR 10A` [POLMST.pf:L78] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — audit: last-action user |
| `PM-LAST-ACTION-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L175`] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — audit: last-action date (working storage) |

#### Standalone persisted DDS fields — NO `PM-` working-storage equivalent (5)

| COBOL Field / Record | COBOL PIC / DDS type | Source [file:line] | ProductCode | Entity / RiskObject | APD Mapping (JSON path + code) OR "Unmapped → report" |
| --- | --- | --- | --- | --- | --- |
| `CLMPF.CLMSTS` ⭐ | `1A` | [`QDDSSRC/CLMPF.pf:L43`] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — claim status persisted only in CLMPF; AAP-mandated inclusion — has no copybook field |
| `SVCPF.SVCID` ⭐ | `12A` | [`QDDSSRC/SVCPF.pf:L16`] | `TermLife` | `— (instance id)` | Unmapped → `unmapped-fields-report.md` — service-request primary key; no copybook equivalent |
| `SVCPF.NEWMODP` ⭐ | `15S2` | [`QDDSSRC/SVCPF.pf:L43`] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — recalculated modal premium (servicing result); no copybook equivalent |
| `SVCPF.SVCDATE` ⭐ | `8S0` | [`QDDSSRC/SVCPF.pf:L49`] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — service-request date; no copybook equivalent |
| `SVCPF.USERID` ⭐ | `10A` | [`QDDSSRC/SVCPF.pf:L51`] | `TermLife` | `— (runtime)` | Unmapped → `unmapped-fields-report.md` — requesting user id (distinct from `PM-LAST-ACTION-USER`); no copybook equivalent |

---

## Validation Footer

| Check | Result |
| --- | --- |
| Copybook elementary fields enumerated | **89** (POLDATA.cpy L16–L175; 88-level & group items excluded) |
| Standalone persisted DDS fields included | **5** (`CLMPF.CLMSTS`, `SVCPF.SVCID`, `SVCPF.NEWMODP`, `SVCPF.SVCDATE`, `SVCPF.USERID`) |
| Total distinct logical fields | **94** |
| Mapped to APD | **37** (14 line + 3 rider + 13 plan + 7 classification) |
| Unmapped (runtime state) | **57** (52 copybook + 5 standalone DDS) |
| Arithmetic | `37 + 52 = 89 copybook`; `89 + 5 = 94`; `37 + 57 = 94` |
| Every field appears exactly once | **Yes** — 0 duplicates, 0 missing, 0 extra (cross-checked against copybook) |
| Three physical files reconciled | **Yes** — `POLMST.pf`, `CLMPF.pf`, `SVCPF.pf` |
| `POLID` double-counting | **Avoided** — FK reuse annotated on `PM-POLICY-ID` only |
| `ProductCode` on every row | **`TermLife`** (single product) |
| APD paths/codes byte-identical to dependency JSON | **Yes** |
| Blank / ambiguous cells | **None** |

**Result: mapped (37) + unmapped (57) = 94 = 100 % of all distinct logical fields.** The fail-closed, zero-silent-drop guarantee (AAP §0.6.4, §0.7.1) holds: every COBOL field/record and every standalone persisted DDS field is accounted for exactly once. This document is written to `discovery/traceability-matrix.md` and is **not** a member of `TermLife-APD-Bundle.zip`.
