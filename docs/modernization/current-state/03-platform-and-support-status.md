# Platform and Support Status

This document establishes what platform LIFE400 is built for, whether that platform release is still supported, and what follows from the answer. It is the shortest document in the current-state layer and the one the security half of the business case rests on, because the declared runtime baseline is the single fact about this system that no amount of better application code can remediate — and until now it was recorded nowhere in the repository.

**Scope.** It owns three things: the declared platform baseline and the evidence for it, the support status of that baseline as published by named third parties, and the consequence that follows from that status. It also fixes the boundary between the language-and-skills question and the hardware-exit question, which are separable and are treated separately here. It does **not** enumerate the estate, which is the subject of [the system inventory](01-system-inventory.md); it does **not** register or rate individual findings, which is the subject of [the security risk register](../risk/01-security-risk-register.md); it does **not** design any control, which is the subject of [the target security control design](../target-state/04-security-control-design.md); and it names no target language, no migration strategy and no regulatory framework.

**Reading the citations.** A citation of the form `[<path>:<locator>]` is plain text rather than a hyperlink, and points at a path in this repository. A plain-text citation stays meaningful in a diff and does not depend on a hosting provider's line-anchor syntax. Every line range cited here was opened and read; none is carried over from a prior description, and no source member was altered to produce this document. Platform terms link to [the IBM i glossary](../reference/glossary-ibm-i.md) on first use in this document.

**Attribution of external figures.** This is the only document in the current-state layer whose conclusion depends on information that is not in this repository, because a support status cannot be read out of source code. Every such figure below is attributed to the publisher that states it, and none is presented as a measurement of this system. Release lines are identified by version number rather than by date, and the comparison drawn is deliberately relational — whether the declared baseline falls inside or outside the set a publisher lists as supported — because a vendor support calendar is not a programme schedule and must not be read as one. No end-of-support date is reproduced here and no duration, ordering or calendar of work is implied. The one date that does appear is quoted from a repository line as evidence of what that line says, and is not planning content.

## The declared baseline

LIFE400 declares its platform in two places in the entry-point document, and they agree with each other.

The header names the platform and the languages: `Platform: IBM AS/400 (iSeries) | ILE COBOL + DDS + ILE CL` [README.md:L4]. The three languages named there — [ILE](../reference/glossary-ibm-i.md#ile-integrated-language-environment) COBOL, [DDS](../reference/glossary-ibm-i.md#dds-data-description-specifications) and [ILE CL](../reference/glossary-ibm-i.md#cl-control-language) — are all platform languages, none of which has an off-platform implementation. The footer names the release the estate targets: `ILE COBOL V3R7 · OS/400 V4R2 · IBM AS/400 Model 9406` [README.md:L264]. That footer is the declared runtime baseline, and it is the fact this whole document turns on.

The declaration is not confined to prose. Every one of the eight COBOL programs names the platform in its configuration section, as both the machine that compiles it and the machine that runs it:

```text
SOURCE-COMPUTER. IBM-AS400.
OBJECT-COMPUTER. IBM-AS400.
```

That pair appears in the menu program at [QCBLLESRC/MAINMENU.cbl:L27-L28] and again in the new-business batch program at [QCBLLESRC/NBUWB.cbl:L45-L46], and the same two statements are present in each of the six remaining COBOL members — sixteen declarations across eight programs, with no member declaring anything else. This is the statement that binds the source to that hardware, and the reason none of it builds anywhere else.

Three further facts make the baseline a live runtime dependency rather than a historical label.

- **One member depends on the release by name.** The nightly driver retrieves the system date and annotates the retrieval with the release that guarantees the format it expects: `/* Y2K: SYSTEM DATE IS YYYYMMDD ON V4R2 AND LATER */` [QCLSRC/DLYUPD.clle:L44], immediately above the retrieval itself [QCLSRC/DLYUPD.clle:L45]. An exhaustive search of all twenty-four members finds this the only reference to a specific platform release anywhere in the estate, which makes it the estate's own record of the release it was written against — and it names the same release as the footer.
- **The orchestration layer depends on operating-system work management rather than on portable primitives.** Session entry inserts the application library into the job's [library list](../reference/glossary-ibm-i.md#library-list) and monitors the operating system's own failure condition for that operation [QCLSRC/STRTLIFE.clle:L27], then removes it again on exit under the same discipline [QCLSRC/STRTLIFE.clle:L41]. The nightly driver does the same [QCLSRC/DLYUPD.clle:L57]. None of this has meaning outside the platform.
- **Notification and error handling depend on the platform's message facility.** Both the session entry program and the nightly driver announce themselves by sending a platform message through the system message file [QCLSRC/STRTLIFE.clle:L32-L34], [QCLSRC/DLYUPD.clle:L47], and every failure path in the orchestration layer is guarded by a [CPF message](../reference/glossary-ibm-i.md#cpf-message) identifier rather than by a language-level exception [QCLSRC/DLYUPD.clle:L76], [QCLSRC/DLYUPD.clle:L100], [QCLSRC/DLYUPD.clle:L103].

The single user interface is likewise platform-bound: the menu display file fixes a twenty-four row by eighty column character grid [QDDSSRC/MNUDSPF.dspf:L12], which is the geometry of the [5250 datastream](../reference/glossary-ibm-i.md#5250-datastream). There is no other presentation path in the estate.

### What the baseline is and is not

Two statements are both true, and conflating them would make this document indefensible.

**What the repository establishes.** `OS/400 V4R2` is the *declared* runtime baseline of this estate [README.md:L264], corroborated inside the source by the sixteen platform declarations above and by the estate's only in-source release reference [QCLSRC/DLYUPD.clle:L44]. Nothing in the estate's own change record moves that baseline forward; the recorded change history stops at a Y2K review, and [the system inventory](01-system-inventory.md) owns the arithmetic of which members carry which form of that annotation.

**What the repository does not establish.** It does not establish that a physical machine has been running this release unpatched for decades. The repository's own front matter records `Original Build: March 24 2026 (mimics 1997–1998)` [README.md:L5], so this tree is a faithful reconstruction of that history rather than an artifact continuously edited since it. This document therefore asserts only the verifiable claim: **the declared target platform is a release outside every set that publishers list as supported, and nothing in the estate moves it forward.** That claim is sufficient for the security argument, and it avoids resting the business case on an assertion about a machine that this repository cannot support.

## Supported release comparison

A support status cannot be derived from this repository, so it is taken from publishers and attributed to them. Two independent end-of-life trackers and the vendor's own lifecycle pages were consulted. The two trackers agree with each other and credit the same upstream data source, so they are treated here as one corroborated position rather than as two independent confirmations.

### What each support tier carries

The tiers matter more than any individual release number, because the whole argument in the next section turns on which tier the declared baseline occupies.

| Support tier | Publisher stating it | What that publisher says the tier carries |
| --- | --- | --- |
| Active support — a currently supported release | `endoflife.date`, and `eol.wiki`, which credits `endoflife.date` as its data source | The releases a system is expected to be running. The guidance published on the tracker page is that one of the listed supported release numbers should be in use. |
| Standard support | IBM, on its *Release life cycle* and *IBM i Release Support* pages | The full support period, which those pages distinguish explicitly from extended support. Vendor lifecycle commentary published by `babyblueit.com` adds that Technology Refreshes — the periodic bundles that add function, hardware enablement and performance improvements on the same base version — are issued only while a release remains in standard support. |
| Extended support, offered as a Service Extension | IBM, on its *Release life cycle* page, in its *IBM i Support Roadmap and Service Extension* document, and in the *Service Extension for IBM i* offering description | IBM states that once a release reaches its effective end-of-service date an Extended Support Offering is required in order to obtain technical assistance, that active software maintenance is a prerequisite for ordering one, and that such offerings exist only where available for a given release. The offering description characterises the coverage as basic usage and problem-rediscovery support for out-of-service releases. `babyblueit.com` describes the tier as still being vendor support but focused on risk management rather than ongoing development, with fixes typically limited to critical or security-related issues. |
| End of life, also stated as end of support | `endoflife.date` and `eol.wiki` | `eol.wiki` states that a release at end of life no longer receives security patches or bug fixes, and that continuing to use it may expose systems to known vulnerabilities. |

One distinction in that table does most of the work below, so it is worth stating on its own: **extended support is a purchasable, deliberately narrowed arrangement for a release that is already out of service. It is not the same status as supported, and it is not a substitute for it.** IBM's own framing reinforces the point from the other direction — its release-support page states that every release of the operating system has a finite support period, and that when a release reaches end of support the direct upgrade paths to later releases are themselves available only for a limited time, after which a multi-step upgrade through an interim release, or in the limiting case a labour-intensive manual upgrade requiring custom services, is the only route forward. Support exhaustion and upgrade-path exhaustion are separate events, and the second follows the first.

### Where the declared baseline sits

| Release line | Classification by the publisher | Fix supply that classification carries |
| --- | --- | --- |
| `7.6`, `7.5`, `7.4` | `endoflife.date` tracks twenty-four versions of this product line and reports three of them in active support; `eol.wiki` names those three as `7.6`, `7.5` and `7.4`, with `7.6.0` as the latest | Technology Refreshes while the release remains in standard support, plus Program Temporary Fixes, which IBM documents as the corrective updates that address specific reported defects |
| `7.3`, `7.2`, `7.1`, `6.1`, `5.4`, `5.3`, `5.2`, `5.1` | End of life, named individually by `eol.wiki` | None. `eol.wiki` states that these releases no longer receive security patches or bug fixes |
| A further thirteen releases older than `5.1`, not named individually | End of life. `endoflife.date` reports twenty-one of the twenty-four tracked versions at end of life, which is the eight named above together with thirteen more | None, on the same basis |
| `OS/400 V4R2` — **the declared baseline of this estate** [README.md:L264] | Outside every published supported set. It is older than every release either tracker names, so it falls at or below that unnamed remainder, and no publisher consulted enumerates it individually | None available |

The trackers identify this product line as a single lineage: `eol.wiki` describes IBM i as the integrated operating system for the vendor's Power systems, formerly known as AS/400 and iSeries. That matters for reading the table, because it means the `OS/400` release named in the footer [README.md:L264] and the `7.x` releases named as supported are points on one release history rather than different products being compared.

### What could not be established

Four gaps are recorded rather than papered over. For a document whose only purpose is to support a decision, an honest gap is worth more than a confident invention.

- **No publisher consulted enumerates `OS/400 V4R2` by name with a status of its own.** The trackers' named lists stop at `5.1` and fold everything older into an unnamed remainder. The claim this document makes is therefore the claim the evidence supports — that the declared baseline falls outside every published supported set and below the oldest release named — and not a specific end-of-support point for `V4R2`.
- **No extended-support arrangement could be shown to exist for the declared baseline.** Every Service Extension offering found in the sources listed at the end of this document is for a `7.x` release; this is a limit of that search rather than a statement any publisher makes. There is accordingly no attributable basis for saying that even narrowed coverage is purchasable for `OS/400 V4R2`, and this document does not say so in either direction.
- **No vulnerability inventory for the declared baseline could be attributed.** One tracker reports zero tracked vulnerability records for the product line. That is a statement about the tracker's own coverage of the line rather than an assurance about an out-of-service release, and it is deliberately not used here as evidence of safety. The absence of a published count is not evidence either way, and neither reading is asserted.
- **Nothing in this document was verified by execution, and nothing in it could be.** No command available in this environment can compile, bind or run LIFE400. The estate states the reason itself: each COBOL program names the platform as its object computer as well as its source computer [QCBLLESRC/MAINMENU.cbl:L27-L28], so the compile-and-bind chain targets that platform by declaration, and no off-platform compiler for ILE COBOL or ILE CL exists to substitute for it. Every claim about the estate therefore rests on a citation a reader can resolve against the working tree, and every claim about support status rests on an attributed publisher. No claim here rests on a build.

## The security-fix consequence

The argument is three steps, and it is deliberately short. Each step is either established above or cited here.

- **Step one — the declared baseline is outside the supported set.** Established above from the footer [README.md:L264], corroborated in source [QCLSRC/DLYUPD.clle:L44], and classified by the publishers in the table above.
- **Step two — therefore no platform security fix is available for it.** This is not an estimate of likelihood; it is what the classification means. `eol.wiki` states that a release at end of life no longer receives security patches or bug fixes, and IBM states that a release's support period is finite and that technical assistance beyond the effective end-of-service date requires an Extended Support Offering, which could not be shown to exist for this release line at all. A defect in the operating system, in the ILE COBOL runtime that every one of these programs compiles against [QCBLLESRC/MAINMENU.cbl:L27-L28], or in the platform's handling of the character datastream that carries the only user interface [QDDSSRC/MNUDSPF.dspf:L12], cannot be remediated by patching a release for which no patch is issued. The remediation channel is not slow or expensive here — it is absent.
- **Step three — the only remaining response is compensating controls in the layers above, and this estate provides none.** An unsupported platform is survivable when the application above it compensates. This one does not, and the evidence is in the source rather than in an opinion.

The compensating-control evidence, stated only as far as step three needs it:

- The signed-on profile is available to the application and is used cosmetically. It is captured at [QCBLLESRC/MAINMENU.cbl:L57] and moved straight to a screen field for display at [QCBLLESRC/MAINMENU.cbl:L58]. It is never persisted and never passed onward to any called program, so no decision anywhere in the estate depends on who is signed on.
- Every authenticated user reaches an identical option set. The dispatch loop reads the selection and routes it to one of four programs by static call, with no role, authority or permission test at any branch [QCBLLESRC/MAINMENU.cbl:L60-L92].
- Object authority is left at the platform default rather than being hardened deliberately. The application library is created with no authority parameter [README.md:L125], and the supporting work-management objects are created the same way [README.md:L199-L206]. The platform's [adopted authority](../reference/glossary-ibm-i.md#adopted-authority) mechanism is not instantiated anywhere in the estate. One caution belongs with that observation: the only user-profile keyword in the whole estate is a configuration comment describing how to install the session-entry program as a user's initial program [QCLSRC/STRTLIFE.clle:L15], and reading that comment as evidence of authority adoption would misstate this system's security posture.

That is where this document stops. It does not enter these observations as findings, does not rate them, and does not design anything that closes them. The register that carries severity, impact and a closing control for each is [the security risk register](../risk/01-security-risk-register.md); the design of those controls is [the target security control design](../target-state/04-security-control-design.md); and the handling of personal and health data is [the compliance and data-protection analysis](../risk/02-compliance-and-data-protection.md), which also owns the question of which regulatory frameworks apply — a question this document does not answer in either direction. The observations appear here only because step three of the platform argument requires them.

## Hardware exit is a separate decision

There is a well-documented failure mode in programmes that begin from a platform-support finding. The finding is about an operating system running on particular hardware, so the programme is scoped as a hardware exit; the language-and-skills question that actually motivated it is folded into a far larger and far riskier decision; and the combined decision then absorbs the scope, the scrutiny and the delay that a hardware migration attracts. Conflating the two inflates scope and stalls work that could have proceeded on its own. This document separates them explicitly, before the migration layer inherits the confusion.

- **The two drivers are separable.** The talent driver is a statement about the languages the estate is written in — ILE COBOL, DDS and ILE CL, three platform languages named together in the header [README.md:L4] and locked to the platform in every COBOL configuration section [QCBLLESRC/MAINMENU.cbl:L27-L28], [QCBLLESRC/NBUWB.cbl:L45-L46]. Moving the business logic into a mainstream language addresses that driver directly. Where the resulting system runs is a different question, resting on different evidence, with a different blast radius.
- **Neither direction blocks the other.** Modernizing the language does not require a hardware-exit decision to be taken first, and taking a hardware-exit decision does not by itself deliver either the language or the skills profile the business asked for. Carrying the existing COBOL onto other infrastructure would change where the estate runs while leaving the talent driver exactly where it started.
- **The target is specified without a hosting commitment.** The destination described in the target-state layer is hosting-agnostic, with a containerized reference deployment as an illustrative form rather than a required one, so that the hosting question can be answered later on its own evidence instead of being settled by implication now.
- **Deferred, not rejected.** Nothing in this document argues for remaining on the hardware. It argues only that the hardware question is a distinct decision and is not taken in this layer.

The decision itself — its drivers, the options weighed, and the consequences accepted — is recorded once, in [MOD-ADR-007, hardware exit deferred](../decisions/MOD-ADR-007-hardware-exit-deferred.md). This document supplies the platform evidence that record rests on and deliberately does not restate its reasoning, so that if the decision is revisited it is superseded in one place and nothing here has to change.

## What this means for Driver 1

The stated business driver is to minimize security risk. Against that driver the platform baseline is not one risk among several in this estate; it is the one with a different shape from all the others, and the difference is what makes it decisive.

Every other exposure in LIFE400 is remediable in place, at least in principle. A menu that offers every signed-on user the same options [QCBLLESRC/MAINMENU.cbl:L60-L92] could be given an authority test. A profile that is captured and merely displayed [QCBLLESRC/MAINMENU.cbl:L57-L58] could be carried into the records the system writes. A library created with no authority parameter [README.md:L125] could be secured, as could the work-management objects created alongside it [README.md:L199-L206]. Each of those is a matter of writing different code, or issuing different commands, on the platform the system already runs on — and each is registered, rated and given a closing control in [the security risk register](../risk/01-security-risk-register.md).

The platform baseline is not like that. It cannot be remediated by writing better application code, because the deficiency is not in the application code. It is that the supply of fixes for the release beneath the code is exhausted, and no degree of application quality restores it. Only two responses reach that layer at all: move the release, which is the hardware-and-operating-system decision deliberately deferred in the section above, or move the business logic out of the platform languages, which is what the talent driver independently asks for.

That is the shape of the argument the security driver produces. Hardening the application is necessary and worth doing, and it is not sufficient, because no application change can reach an unsupported layer beneath it. This is why Driver 1 points toward migration rather than toward hardening alone — and it converges with Driver 2 on the same conclusion from entirely independent premises, one about fix supply and one about labour supply, which is the strongest form the case can take. Which migration strategy follows, and which language it targets, are settled in [the strategy options and selection document](../migration/01-strategy-options-and-selection.md) and [the target-language decision matrix](../talent/02-target-language-decision-matrix.md). Neither is settled here.

The argument above is about what is remediable and what is not. No cost, effort, duration or staffing figure is asserted anywhere in this document, and none should be inferred from it.

## Figures owned by other documents

Each figure has exactly one owning document, so that a number is corrected in one place. This document owns the declared baseline, its published support status, and the consequence that follows. It owns nothing else, and it restates none of the following.

- The member register, per-member line counts, the language distribution and the estate's recorded change history — [the system inventory](01-system-inventory.md).
- Findings carrying severity, impact and a closing control — [the security risk register](../risk/01-security-risk-register.md).
- Personal and health data handling, retention exposure, and which regulatory frameworks apply, which must be confirmed by the business — [the compliance and data-protection analysis](../risk/02-compliance-and-data-protection.md).
- The design of every control named in this document — [the target security control design](../target-state/04-security-control-design.md).
- The hardware-exit decision and its rationale — [MOD-ADR-007](../decisions/MOD-ADR-007-hardware-exit-deferred.md).
- The evaluation and selection of a migration strategy — [the strategy options and selection document](../migration/01-strategy-options-and-selection.md).
- The target language, runtime and datastore — [the target-language decision matrix](../talent/02-target-language-decision-matrix.md).
- Definitions of every platform term used above — [the IBM i glossary](../reference/glossary-ibm-i.md).

## Source citations

Every member cited by this document, grouped by artifact class. No member was modified.

- ILE COBOL — `QCBLLESRC/MAINMENU.cbl`, `QCBLLESRC/NBUWB.cbl`
- ILE CL — `QCLSRC/STRTLIFE.clle`, `QCLSRC/DLYUPD.clle`
- DDS display file — `QDDSSRC/MNUDSPF.dspf`
- Repository overview — `README.md`

### External sources

Support-status information is not present in this repository and is attributed here to the publisher that states it. Locations are given as plain text for the same reason source citations are.

- `endoflife.date` — IBM i / iSeries product page, `endoflife.date/ibm-i`: tracked version count, count in active support, count at end of life, and the guidance that a supported release number should be in use.
- `eol.wiki` — IBM iSeries end-of-life page, `eol.wiki/ibm-i`, which credits `endoflife.date` as its data source: the named supported releases, the named end-of-life releases, the product-lineage description, and the statement of what end of life means for security patches and bug fixes.
- IBM — *IBM i Release Support*, `www.ibm.com/support/pages/ibm-i-release-support`: that each release has a finite support period, and the consequences for upgrade paths once end of support is reached.
- IBM — *Release life cycle*, `www.ibm.com/support/pages/release-life-cycle`: the distinction between the standard support period and extended support, and the existence of Service Extension offerings for named releases.
- IBM — *IBM i Support Roadmap and Service Extension*: that an Extended Support Offering is required for technical assistance beyond the effective end-of-service date, that active software maintenance is a prerequisite for ordering one, and that such offerings exist only where available.
- IBM — *Service Extension for IBM i* offering description: the characterisation of the coverage as basic usage and problem-rediscovery support for out-of-service releases.
- IBM — *PTFs: FAQs* and *IBM i Support: Recommended fixes*: the description of a Program Temporary Fix as a corrective update addressing specific reported problems, and of Technology Refresh and cumulative fix groups.
- `babyblueit.com` — vendor lifecycle commentary on the change from standard support to Service Extension: that Technology Refreshes are issued only during standard support, and that Service Extension coverage is focused on risk management rather than ongoing development, with fixes typically limited to critical or security-related issues.
