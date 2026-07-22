# LIFE400 → Guidewire APD — Unmapped Fields Report

> **Deliverable #5** of *LIFE400 COBOL Product Model Distillation to Guidewire APD* (AAP §0.3.1, §0.4.1, §0.6.2). This document is the **exclusion-side proof of "zero silent drops":** it enumerates **every** LIFE400 field that is classified as **per-policy / per-transaction runtime state** — i.e. *not* product configuration — and gives each an **explicit justification** for its exclusion from the APD product model. It is the companion to `traceability-matrix.md`; together the **37 mapped** fields (matrix) and the **57 unmapped** fields (this report) account for **100 % of the 94 distinct logical fields, each exactly once, with no overlap** (AAP §0.6.4, §0.7.1).

## Classifier rule (AAP §0.6.2)

A field is **product configuration** — and is therefore mapped into `product/TermLife.json` / `editions/TermLife-BaseEdition.json` (see `traceability-matrix.md`) — **if and only if** it defines a **coverage, term, limit, eligibility rule, or classification typeList**. **Otherwise it is per-policy or per-transaction runtime state** and is listed here. Concretely, runtime state = **instance identifiers, computed results, rating factors, lifecycle dates, transaction deltas (before/after images), and audit/processing artifacts**. None of these describe the *product*; each describes a specific policy / claim / servicing instance, or a computation the COBOL performs at runtime.

**Why this is behavior-preserving (AAP §0.1.1, §0.6.2).** The reason these fields are unmapped is precisely that **all rating, premium, lifecycle, and claims/servicing computation stays in the untouched COBOL.** The distillation captures product *structure* only; every numeric factor, every computed premium, every lifecycle transition, and every claim/servicing calculation remains exactly where it is in `LIFE400`. Excluding these fields from the product model is what guarantees the legacy system continues to behave identically.

## Scope & provenance notes

- **Read-only legacy source (AAP §0.2.2, §0.7).** All sources under `QCBLLESRC/`, `QCLSRC/`, `QCPYSRC/`, `QDDSSRC/`, `.swm/`, and `README.md` are *described only* — nothing is modified, annotated, reordered, or reformatted.
- **This `.md` is EXCLUDED from `TermLife-APD-Bundle.zip`** (AAP §0.2.2, §0.7.2) — it is written alongside the bundle but is never packaged inside it.
- **Exactly-once / no overlap (AAP §0.7.1).** Every field named below is unique and does **not** also appear as a *mapped* field in `traceability-matrix.md`. Where a field's *value domain* is mapped as a typeList but its *per-instance value* is runtime (the classification codes), that split is annotated — the field name itself is counted once, on the mapped side.
- **Count is exactly 57** = **52** copybook (`PM-`) runtime fields + **5** standalone persisted DDS fields with no copybook equivalent.

---

## Category 1 — Control / processing artifacts (6)

Instance identifiers and program control values from `PM-CONTROL-AREA` [`QCPYSRC/POLDATA.cpy:L16`]. These identify or bookkeep a specific policy/run; none describes the product.

| COBOL/DDS field | PIC / DDS type | Source [file:line] | Category | Justification (why runtime, not product config) |
| --- | --- | --- | --- | --- |
| `PM-POLICY-ID` | `X(12)` | [`QCPYSRC/POLDATA.cpy:L17`]; PK `POLMST.POLID` [`QDDSSRC/POLMST.pf:L16`]; FK `CLMPF.POLID` [`QDDSSRC/CLMPF.pf:L18`]; FK `SVCPF.POLID` [`QDDSSRC/SVCPF.pf:L18`] | Control / processing | Instance identifier of a specific policy — identity, not product structure. The **same logical field** is reused as the foreign key in `CLMPF`/`SVCPF`; it is counted **exactly once** here (FK reuse annotated, not double-counted). |
| `PM-APPLICATION-ID` | `X(12)` | [`QCPYSRC/POLDATA.cpy:L18`]; `POLMST.APPID` [`QDDSSRC/POLMST.pf:L18`] | Control / processing | Per-application instance identifier; identifies one submission, not the product. |
| `PM-PROCESS-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L20`]; `POLMST.PRCDATE 8S0` [`QDDSSRC/POLMST.pf:L21`] | Control / processing | Per-run processing timestamp (8-digit `YYYYMMDD`); runtime bookkeeping stamped whenever the record is processed. |
| `PM-CURRENCY-CODE` | `X(03)` | [`QCPYSRC/POLDATA.cpy:L35`]; captured at [`QDDSSRC/NBUWDSPF.dspf:L46`] → [`QCBLLESRC/NBUWMNT.cbl:L106`]; `POLMST.CURCD` [`QDDSSRC/POLMST.pf:L29`] | Control / processing | Per-policy currency code captured as an unconstrained 3-character input field (`NBCURCD`, [`QDDSSRC/NBUWDSPF.dspf:L46`]) and moved verbatim into the record [`QCBLLESRC/NBUWMNT.cbl:L106`], then persisted as `POLMST.CURCD` [`QDDSSRC/POLMST.pf:L29`]. The copybook defines no Level-88 value set for this field, so there is no enumerated product currency domain to model as a typeList; it is therefore a per-policy runtime value, not a product-defining term. |
| `PM-RETURN-CODE` | `9(02)` | [`QCPYSRC/POLDATA.cpy:L36`] | Control / processing | Program return/status code (working storage only); transient control flow set by the COBOL on each call. |
| `PM-RETURN-MESSAGE` | `X(100)` | [`QCPYSRC/POLDATA.cpy:L37`] | Control / processing | Program return message text (working storage only); transient control flow, never persisted to `POLMST`. |

## Category 2 — Derived / rating-adjustment values (2)

Values from `PM-INSURED-DETAILS` [`QCPYSRC/POLDATA.cpy:L54`] that are computed or applied at runtime rather than configured on the product.

| COBOL/DDS field | PIC / DDS type | Source [file:line] | Category | Justification (why runtime, not product config) |
| --- | --- | --- | --- | --- |
| `PM-ATTAINED-AGE` | `9(03)` | [`QCPYSRC/POLDATA.cpy:L59`]; computed at [`QCBLLESRC/SVCMNT.cbl:L164-L166`], [`QCBLLESRC/SVCBILB.cbl:L189-L191`] | Derived / rating-adjustment | Attained age derived at runtime as `PM-ISSUE-AGE + ((PM-PROCESS-DATE − PM-ISSUE-DATE) / 365)` — computed in `SVCMNT` [`QCBLLESRC/SVCMNT.cbl:L164-L166`] and in `SVCBILB` paragraph `1200-CALCULATE-ATTAINED-AGE` [`QCBLLESRC/SVCBILB.cbl:L189-L191`]; a derived value, never configured on the product. (Contrast: `PM-ISSUE-AGE` is a mapped line field — see matrix §A.) |
| `PM-FLAT-EXTRA-RATE` | `9(02)V9999` | [`QCPYSRC/POLDATA.cpy:L73`]; `POLMST.FLTXTRA 6S4` [`QDDSSRC/POLMST.pf:L49`] | Derived / rating-adjustment | Per-policy underwriting flat-extra rating adjustment (per 1000), applied inside base-premium calc [`QCBLLESRC/NBUWB.cbl:L388-L404`]; a rating input, not a product-config value. |

## Category 3 — Rating factors (5)

All five are loaded and applied by `1400-LOAD-RATE-FACTORS` [`QCBLLESRC/NBUWB.cbl:L301-L340`] and consumed by the premium computation. They are the numeric multipliers of the COBOL rating engine — premium *calculation* logic that stays in COBOL (AAP §0.2.2, §0.6.2). The product structure references the **classification** (`gender` / `smokerStatus` / `uwClass` typeLists, all mapped) but **never** the numeric factor.

| COBOL/DDS field | PIC / DDS type | Source [file:line] | Category | Justification (why runtime, not product config) |
| --- | --- | --- | --- | --- |
| `PM-BASE-MORTALITY-RATE` | `9(02)V9999` | [`QCPYSRC/POLDATA.cpy:L83`]; loaded by `1400-LOAD-RATE-FACTORS` [`QCBLLESRC/NBUWB.cbl:L301-L340`] | Rating factor | Base mortality rate selected by age band (NB-401) inside `1400-LOAD-RATE-FACTORS`; a COBOL-resident premium input, not product config. |
| `PM-GENDER-FACTOR` | `9(01)V9999` | [`QCPYSRC/POLDATA.cpy:L84`]; [`QCBLLESRC/NBUWB.cbl:L301-L340`] | Rating factor | Gender rating multiplier assigned by NB-402 in `1400-LOAD-RATE-FACTORS`; COBOL-resident rating value. The `Gender` typeList (domain) is mapped; the multiplier is not. |
| `PM-SMOKER-FACTOR` | `9(01)V9999` | [`QCPYSRC/POLDATA.cpy:L85`]; [`QCBLLESRC/NBUWB.cbl:L301-L340`] | Rating factor | Smoker rating multiplier assigned by NB-403 in `1400-LOAD-RATE-FACTORS`; COBOL-resident rating value. |
| `PM-OCCUPATION-FACTOR` | `9(01)V9999` | [`QCPYSRC/POLDATA.cpy:L86`]; [`QCBLLESRC/NBUWB.cbl:L301-L340`] | Rating factor | Occupation rating multiplier assigned by NB-404 in `1400-LOAD-RATE-FACTORS`; COBOL-resident rating value. |
| `PM-UW-FACTOR` | `9(01)V9999` | [`QCPYSRC/POLDATA.cpy:L87`]; [`QCBLLESRC/NBUWB.cbl:L301-L340`] | Rating factor | UW-class rating multiplier assigned by NB-405 in `1400-LOAD-RATE-FACTORS`; COBOL-resident rating value. The `UWClass` typeList (domain) is mapped; the multiplier is not. |

## Category 4 — Rider rating & premium (2)

The two `PM-RIDER-TABLE` [`QCPYSRC/POLDATA.cpy:L88-L96`] elements that are runtime rider values — one **unused/cleared rate slot** and one **computed per-rider premium**. (Contrast: `PM-RIDER-CODE`, `PM-RIDER-SUM-ASSURED`, `PM-RIDER-STATUS` ARE mapped as coverage-clause discriminator / term / status — see `traceability-matrix.md` §B.)

| COBOL/DDS field | PIC / DDS type | Source [file:line] | Category | Justification (why runtime, not product config) |
| --- | --- | --- | --- | --- |
| `PM-RIDER-RATE` | `9(02)V9999` | [`QCPYSRC/POLDATA.cpy:L92`]; cleared at [`QCBLLESRC/SVCBILB.cbl:L379`] | Rider rating / premium | **Unused / cleared legacy runtime slot.** It is never read by premium calculation; its only reference in the entire codebase is [`QCBLLESRC/SVCBILB.cbl:L379`], which clears it to zeros (`MOVE ZEROS TO PM-RIDER-RATE`). The rider rates that premium calculation actually applies are **hard-coded literals** inside `1700-CALCULATE-RIDER-PREMIUM` [`QCBLLESRC/NBUWB.cbl:L405-L431`] (ADB01 0.1800/1000, WOP01 6% of base, CI001 1.2500/1000) and do **not** flow through this field. |
| `PM-RIDER-ANNUAL-PREM` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L93`] | Rider rating / premium | Per-rider computed annual premium, produced by `1700-CALCULATE-RIDER-PREMIUM` [`QCBLLESRC/NBUWB.cbl:L405-L431`]; a computed result, not product config. |

## Category 5 — Computed premium results (8)

The `PM-PREMIUM-RESULTS` group [`QCPYSRC/POLDATA.cpy:L98-L106`] — per-policy computed values, split by their **actual producer**. **Six** are outputs of the NBUWB new-business premium paragraphs: `1600-CALCULATE-BASE-PREMIUM` [`QCBLLESRC/NBUWB.cbl:L388-L404`] (base annual premium), `1700-CALCULATE-RIDER-PREMIUM` [`QCBLLESRC/NBUWB.cbl:L405-L431`] (rider annual total), and `1800-CALCULATE-TOTAL-PREMIUM` [`QCBLLESRC/NBUWB.cbl:L436-L464`] — gross [`QCBLLESRC/NBUWB.cbl:L438-L441`], tax [`QCBLLESRC/NBUWB.cbl:L443-L444`], total [`QCBLLESRC/NBUWB.cbl:L445-L446`], and modal [`QCBLLESRC/NBUWB.cbl:L462-L464`]. The remaining **two** are produced by **servicing**, not new business: `PM-OUTSTANDING-PREMIUM` is maintained during servicing/billing (`MOVE PM-MODAL-PREMIUM TO PM-OUTSTANDING-PREMIUM` at [`QCBLLESRC/SVCBILB.cbl:L213`], [`QCBLLESRC/SVCBILB.cbl:L411`], [`QCBLLESRC/SVCMNT.cbl:L331`]), and `PM-PREMIUM-DELTA` is computed during servicing amendments in paragraph `3100-REPRICE-POLICY` ([`QCBLLESRC/SVCMNT.cbl:L421`], [`QCBLLESRC/SVCBILB.cbl:L428`]). Every value here is computed per policy; none is a product definition.

| COBOL/DDS field | PIC / DDS type | Source [file:line] | Category | Justification (why runtime, not product config) |
| --- | --- | --- | --- | --- |
| `PM-BASE-ANNUAL-PREMIUM` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L99`]; computed in `1600-CALCULATE-BASE-PREMIUM` [`QCBLLESRC/NBUWB.cbl:L388-L404`] | Computed premium result | Output of the base-premium COBOL paragraph (sum-assured × mortality × factors); a per-policy computed result, never a product definition. |
| `PM-RIDER-ANNUAL-TOTAL` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L100`]; `1700-CALCULATE-RIDER-PREMIUM` [`QCBLLESRC/NBUWB.cbl:L405-L431`] | Computed premium result | Sum of per-rider premiums accumulated at runtime; computed result. |
| `PM-GROSS-ANNUAL-PREMIUM` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L101`]; computed in `1800-CALCULATE-TOTAL-PREMIUM` (NB-801) [`QCBLLESRC/NBUWB.cbl:L438-L441`] | Computed premium result | Gross premium = base annual premium + rider annual total + `PM-ANNUAL-POLICY-FEE`; a per-policy computed result. |
| `PM-TAX-AMOUNT` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L102`]; computed in `1800-CALCULATE-TOTAL-PREMIUM` [`QCBLLESRC/NBUWB.cbl:L443-L444`] | Computed premium result | Computed tax amount = gross annual premium × `PM-TAX-RATE`. The tax *rate* is product config (mapped edition value); the computed *amount* is runtime. |
| `PM-TOTAL-ANNUAL-PREMIUM` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L103`]; computed in `1800-CALCULATE-TOTAL-PREMIUM` [`QCBLLESRC/NBUWB.cbl:L445-L446`]; persisted `POLMST.ANPREM 15S2` [`QDDSSRC/POLMST.pf:L59`] | Computed premium result | Computed total annual premium = gross + tax, persisted per policy; a stored result, not a product value. |
| `PM-MODAL-PREMIUM` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L104`]; computed in `1800-CALCULATE-TOTAL-PREMIUM` (NB-802) [`QCBLLESRC/NBUWB.cbl:L462-L464`]; persisted `POLMST.MODPREM 15S2` [`QDDSSRC/POLMST.pf:L61`] | Computed premium result | Computed modal (per-billing-period) premium, persisted per policy; computed result. |
| `PM-OUTSTANDING-PREMIUM` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L105`]; maintained by servicing [`QCBLLESRC/SVCBILB.cbl:L213`], [`QCBLLESRC/SVCBILB.cbl:L411`], [`QCBLLESRC/SVCMNT.cbl:L331`]; persisted `POLMST.OUTPREM 15S2` [`QDDSSRC/POLMST.pf:L63`] | Computed premium result | Outstanding premium balance set during servicing/billing (`MOVE PM-MODAL-PREMIUM TO PM-OUTSTANDING-PREMIUM`), not by the new-business premium calc; a runtime balance, not product config. |
| `PM-PREMIUM-DELTA` | `S9(13)V99` | [`QCPYSRC/POLDATA.cpy:L106`]; computed in `3100-REPRICE-POLICY` [`QCBLLESRC/SVCMNT.cbl:L421`], [`QCBLLESRC/SVCBILB.cbl:L428`]; persisted `SVCPF.PREMDLT 15S2` [`QDDSSRC/SVCPF.pf:L45`] | Computed premium result | Signed premium delta (new total − old total) computed during a servicing amendment; a per-transaction computed result held in `SVCPF`. |

## Category 6 — Lifecycle dates (6)

The `PM-DATE-DETAILS` group [`QCPYSRC/POLDATA.cpy:L108-L115`] — per-policy / per-claim lifecycle timestamps, all 8-digit `YYYYMMDD` and fully Y2K-remediated per [`README.md:L206-L209`]. These are instance state, not product config. *(If ever surfaced in APD they would be typed `Date`, never `String` — but they belong to runtime state and stay in the COBOL/DDS records. `PM-DATE-OF-BIRTH` is the mapped insured attribute — see matrix §A — and is deliberately not listed here.)*

| COBOL/DDS field | PIC / DDS type | Source [file:line] | Category | Justification (why runtime, not product config) |
| --- | --- | --- | --- | --- |
| `PM-ISSUE-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L110`]; `POLMST.ISSDATE 8S0` [`QDDSSRC/POLMST.pf:L67`] | Lifecycle date | Per-policy issue date (`YYYYMMDD`); instance lifecycle state, not product config. |
| `PM-EFFECTIVE-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L111`]; `POLMST.EFFDATE 8S0` [`QDDSSRC/POLMST.pf:L69`] | Lifecycle date | Per-policy effective date (`YYYYMMDD`); instance lifecycle state. |
| `PM-PAID-TO-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L112`]; `POLMST.PAIDTO 8S0` [`QDDSSRC/POLMST.pf:L71`] | Lifecycle date | Per-policy paid-to date (`YYYYMMDD`); billing lifecycle state. |
| `PM-EXPIRY-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L113`]; `POLMST.EXPDATE 8S0` [`QDDSSRC/POLMST.pf:L73`] | Lifecycle date | Per-policy expiry date (`YYYYMMDD`); lifecycle state. |
| `PM-LAST-MAINT-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L114`]; `POLMST.LSTMNT 8S0` [`QDDSSRC/POLMST.pf:L75`] | Lifecycle date | Per-policy last-maintenance date (`YYYYMMDD`); operational lifecycle state. |
| `PM-DATE-OF-DEATH` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L115`]; `CLMPF.DTHDTC 8S0` [`QDDSSRC/CLMPF.pf:L26`] | Lifecycle date | Per-claim date of death (`YYYYMMDD`); claim instance state. |


## Category 7 — Servicing transaction values (8)

The `PM-SERVICING-DETAILS` group [`QCPYSRC/POLDATA.cpy:L117-L132`] — before/after amendment images and per-servicing-transaction values. The before/after images (old/new plan code, sum assured, billing mode) and the charged fee **are persisted in `SVCPF`** [`QDDSSRC/SVCPF.pf:L25-L40`]; the one exception is `PM-UW-REQUIRED`, which has **no `SVCPF` column** and is a **working-storage-only** runtime flag (only ever set to `'Y'` at [`QCBLLESRC/SVCMNT.cbl:L230`] / [`QCBLLESRC/SVCBILB.cbl:L289`]). These record what changed on a specific amendment; they are transaction state, not product structure. *(The classification codes `PM-AMENDMENT-TYPE` and `PM-AMENDMENT-STATUS` contribute their value domains to product typeLists and are therefore mapped — see matrix §D — so they are deliberately not listed here.)*

| COBOL/DDS field | PIC / DDS type | Source [file:line] | Category | Justification (why runtime, not product config) |
| --- | --- | --- | --- | --- |
| `PM-OLD-PLAN-CODE` | `X(05)` | [`QCPYSRC/POLDATA.cpy:L125`]; `SVCPF.OLDPLAN 5A` [`QDDSSRC/SVCPF.pf:L28`] | Servicing transaction | Amendment **before-image** (old plan code) held in `SVCPF`; per-transaction value, not product config. |
| `PM-NEW-PLAN-CODE` | `X(05)` | [`QCPYSRC/POLDATA.cpy:L126`]; `SVCPF.NEWPLAN 5A` [`QDDSSRC/SVCPF.pf:L30`] | Servicing transaction | Amendment **after-image** (new plan code); per-transaction value. |
| `PM-OLD-SUM-ASSURED` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L127`]; `SVCPF.OLDSA 15S2` [`QDDSSRC/SVCPF.pf:L33`] | Servicing transaction | Amendment **before-image** (old sum assured); per-transaction value. |
| `PM-NEW-SUM-ASSURED` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L128`]; `SVCPF.NEWSA 15S2` [`QDDSSRC/SVCPF.pf:L35`] | Servicing transaction | Amendment **after-image** (new sum assured); per-transaction value. |
| `PM-OLD-BILLING-MODE` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L129`]; `SVCPF.OLDBM 1A` [`QDDSSRC/SVCPF.pf:L38`] | Servicing transaction | Amendment **before-image** (old billing mode); per-transaction value. |
| `PM-NEW-BILLING-MODE` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L130`]; `SVCPF.NEWBM 1A` [`QDDSSRC/SVCPF.pf:L40`] | Servicing transaction | Amendment **after-image** (new billing mode); per-transaction value. |
| `PM-SERVICE-FEE-CHARGED` | `9(07)V99` | [`QCPYSRC/POLDATA.cpy:L131`]; `SVCPF.SVCFEE 9S2` [`QDDSSRC/SVCPF.pf:L25`] | Servicing transaction | Fee **actually charged** on a given amendment (a per-transaction result); distinct from the product-config `PM-SERVICE-FEE` rule value, which IS mapped (matrix §C). |
| `PM-UW-REQUIRED` | `X(01)` (Y/N) | [`QCPYSRC/POLDATA.cpy:L132`] | Servicing transaction | **Runtime servicing flag** — whether underwriting is required for a given amendment. **Special note:** although AAP §0.4.2 uses `PM-UW-REQUIRED` as the *representative Boolean type-mapping example*, it is **not** a product field — it is per-servicing-transaction state. The product's actual Boolean field is **`highRiskAvocation`** (mapped; `product/TermLife.json` field of `type: "Boolean"`, edition rule `required: false`). Documented explicitly to preempt confusion between the spec's illustrative example and the real product field. |

## Category 8 — Claim transaction values (13)

The `PM-CLAIM-DETAILS` group [`QCPYSRC/POLDATA.cpy:L138-L170`] — per-claim adjudication state, evidence flags, beneficiary data, dates, and settled amounts, persisted in `CLMPF`. All are claims runtime, out of scope for the product model (AAP §0.2.2). *(The claim **classification** codes — `PM-CLAIM-TYPE`, `PM-CAUSE-OF-DEATH`, `PM-CLAIM-PAYMENT-MODE`, `PM-INVESTIGATION-STATUS`, `PM-CLAIM-DECISION` — contribute their **value domains** to product typeLists and are therefore mapped in matrix §D; only their **per-claim values** are runtime. They are deliberately not listed here, so there is no overlap.)*

| COBOL/DDS field | PIC / DDS type | Source [file:line] | Category | Justification (why runtime, not product config) |
| --- | --- | --- | --- | --- |
| `PM-CLAIM-ID` | `X(12)` | [`QCPYSRC/POLDATA.cpy:L139`]; PK `CLMPF.CLMID` [`QDDSSRC/CLMPF.pf:L16`] | Claim transaction | Claim instance identifier (primary key of `CLMPF`); identity, not product structure. |
| `PM-DEATH-CERT-RECD` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L148`]; `CLMPF.DTHCERT 1A` [`QDDSSRC/CLMPF.pf:L29`] | Claim transaction | Per-claim document-received flag; claims adjudication state. |
| `PM-CLAIM-FORM-RECD` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L149`]; `CLMPF.CLMFORM 1A` [`QDDSSRC/CLMPF.pf:L31`] | Claim transaction | Per-claim document-received flag; claims state. |
| `PM-ID-PROOF-RECD` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L150`]; `CLMPF.IDPROOF 1A` [`QDDSSRC/CLMPF.pf:L33`] | Claim transaction | Per-claim document-received flag; claims state. |
| `PM-MEDICAL-RECORDS-RECD` | `X(01)` | [`QCPYSRC/POLDATA.cpy:L151`]; `CLMPF.MEDRECS 1A` [`QDDSSRC/CLMPF.pf:L35`] | Claim transaction | Per-claim document-received flag; claims state. |
| `PM-CLAIM-SUBMIT-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L152`]; `CLMPF.CLMDATE 8S0` [`QDDSSRC/CLMPF.pf:L58`] | Claim transaction | Per-claim submission date (`YYYYMMDD`); claims lifecycle state. |
| `PM-CLAIM-INVEST-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L153`]; `CLMPF.INVDATE 8S0` [`QDDSSRC/CLMPF.pf:L60`] | Claim transaction | Per-claim investigation date (`YYYYMMDD`); claims state. |
| `PM-CLAIM-ADJUDIC-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L154`]; `CLMPF.ADJDATE 8S0` [`QDDSSRC/CLMPF.pf:L62`] | Claim transaction | Per-claim adjudication date (`YYYYMMDD`); claims state. |
| `PM-CLAIM-SETTLE-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L155`]; `CLMPF.SETDATE 8S0` [`QDDSSRC/CLMPF.pf:L64`] | Claim transaction | Per-claim settlement date (`YYYYMMDD`); claims state. |
| `PM-BENEFICIARY-NAME` | `X(40)` | [`QCPYSRC/POLDATA.cpy:L156`]; `CLMPF.BENNAME 40A` [`QDDSSRC/CLMPF.pf:L38`] | Claim transaction | Per-claim beneficiary name; instance data, not product config. |
| `PM-BENEFICIARY-RELATION` | `X(20)` | [`QCPYSRC/POLDATA.cpy:L157`]; `CLMPF.BENREL 20A` [`QDDSSRC/CLMPF.pf:L40`] | Claim transaction | Per-claim beneficiary relation; instance data. |
| `PM-CLAIM-PAYMENT-AMT` | `9(13)V99` | [`QCPYSRC/POLDATA.cpy:L169`]; `CLMPF.PYMTAMT 15S2` [`QDDSSRC/CLMPF.pf:L52`] | Claim transaction | Per-claim payment amount (a settled/computed result); claims state. |
| `PM-CLAIM-HOLD-REASON` | `X(50)` | [`QCPYSRC/POLDATA.cpy:L170`]; `CLMPF.CLMHOLD 50A` [`QDDSSRC/CLMPF.pf:L49`] | Claim transaction | Per-claim free-text hold reason; claims state. |


## Category 9 — Audit / processing artifacts (2)

The `PM-AUDIT-DETAILS` group [`QCPYSRC/POLDATA.cpy:L172-L175`] — operational audit trail of who/when last touched the record. Metadata, not product config.

| COBOL/DDS field | PIC / DDS type | Source [file:line] | Category | Justification (why runtime, not product config) |
| --- | --- | --- | --- | --- |
| `PM-LAST-ACTION-USER` | `X(10)` | [`QCPYSRC/POLDATA.cpy:L173`]; `POLMST.LSTUSR 10A` [`QDDSSRC/POLMST.pf:L78`] | Audit / processing | Audit trail: user who last touched the policy record; operational metadata, not product config. |
| `PM-LAST-ACTION-DATE` | `9(08)` | [`QCPYSRC/POLDATA.cpy:L175`] | Audit / processing | Audit trail: date the record was last touched (`YYYYMMDD`, working storage); operational metadata. |

## Category 10 — Standalone persisted DDS fields with no `PM-` working-storage equivalent (5)

These fields exist **only** in the physical files and have **no copybook (`PM-`) counterpart**, so they must be enumerated directly from the `.pf` sources to guarantee 100 % coverage (AAP §0.4.3, §0.6.4). `CLMPF.CLMSTS` in particular is an **AAP-mandated inclusion** — claim status is persisted only in `CLMPF`.

| COBOL/DDS field | PIC / DDS type | Source [file:line] | Category | Justification (why runtime, not product config) |
| --- | --- | --- | --- | --- |
| ⭐ `CLMPF.CLMSTS` | `1A` | [`QDDSSRC/CLMPF.pf:L43`] | Standalone DDS | **Claim status persisted only in `CLMPF`** — has no copybook (`PM-`) field. AAP-mandated inclusion (§0.4.3, §0.6.4). Per-claim lifecycle status; runtime state. |
| `SVCPF.SVCID` | `12A` | [`QDDSSRC/SVCPF.pf:L16`] | Standalone DDS | Service-request primary key; per-transaction instance identifier with no copybook equivalent. |
| `SVCPF.NEWMODP` | `15S2` | [`QDDSSRC/SVCPF.pf:L43`] | Standalone DDS | Recalculated modal premium after an amendment; a computed servicing result with no copybook equivalent. |
| `SVCPF.SVCDATE` | `8S0` | [`QDDSSRC/SVCPF.pf:L49`] | Standalone DDS | Servicing transaction date (`YYYYMMDD`); per-transaction timestamp with no copybook equivalent. |
| `SVCPF.USERID` | `10A` | [`QDDSSRC/SVCPF.pf:L51`] | Standalone DDS | User who performed the servicing transaction (distinct from `PM-LAST-ACTION-USER`); audit metadata with no copybook equivalent. |

---

## Count reconciliation

**Copybook (`PM-`) unmapped fields, by category:**

| Category | Count |
| --- | --- |
| 1 — Control / processing artifacts | 6 |
| 2 — Derived / rating-adjustment values | 2 |
| 3 — Rating factors | 5 |
| 4 — Rider rating & premium | 2 |
| 5 — Computed premium results | 8 |
| 6 — Lifecycle dates | 6 |
| 7 — Servicing transaction values | 8 |
| 8 — Claim transaction values | 13 |
| 9 — Audit / processing artifacts | 2 |
| **Copybook subtotal** | **52** |

**Arithmetic:** `6 + 2 + 5 + 2 + 8 + 6 + 8 + 13 + 2 = ` **52 copybook unmapped**; `+ 5 standalone DDS` (Category 10) `= ` **57 unmapped total**.

**Full-coverage reconciliation (AAP §0.6.4, §0.7.1):**

| Bucket | Count |
| --- | --- |
| Mapped to APD model (`traceability-matrix.md`) | **37** |
| Unmapped — runtime / transaction state (this report) | **57** |
| **Total distinct logical fields** | **94** |

`37 mapped + 57 unmapped = ` **94 distinct logical fields = 100 % coverage, with no overlap** — every COBOL field/record and every standalone persisted DDS field is accounted for **exactly once**, either as a mapped APD path (matrix) or as an explicit unmapped entry (here). There are **no silent drops**.

**Behavior-preservation restatement.** Every field excluded above is excluded precisely because the computation that produces or consumes it — rating factors, premium calculation, lifecycle transitions, claims adjudication, and servicing amendments — **remains in the untouched LIFE400 COBOL**. The APD product model captures product *structure* only; all runtime behavior stays exactly where it is, which is what makes this distillation behavior-preserving.

> This document is written to `discovery/unmapped-fields-report.md` and is **not** a member of `TermLife-APD-Bundle.zip` (AAP §0.2.2, §0.7.2).
