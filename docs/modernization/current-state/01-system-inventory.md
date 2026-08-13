# LIFE400 System Inventory

This document is the as-built register of the LIFE400 estate. It records every [source member](../reference/glossary-ibm-i.md#source-physical-file-and-source-member) the repository holds, what kind of artifact each one is, the version, author, creation date and change notes its own comment banner declares, how many lines it contains, how those members reconcile against the objects the documented build procedure creates, and how much of the estate carried any dedicated documentation before this assessment existed. It is the baseline every other document in this set measures against: a migration path cannot be recommended for a system whose size, shape and authorship have not first been written down.

**Scope.** This document establishes facts and stops there. It does not assess the platform's support status, which is the subject of [the platform and support status document](03-platform-and-support-status.md). It does not analyse structure, call paths or the online-to-batch duplication, which belong to [the current-state architecture](02-architecture-current-state.md). It does not describe columns, field types or domains, which belong to [the current data model](04-data-model-current-state.md). It does not census business rules, which belongs to [the business rule inventory](05-business-rule-inventory.md). It does not explain the build sequence or the runtime that carries the batch work, which belong to [the operational model](06-operational-model.md). And it does not decide what becomes of any defect, stub or inert feature, which is the sole business of [the known defects and stubs register](07-known-defects-and-stubs.md). Where a member has no documentation, this document says so; it does not disposition it. Estate size is one of the two drivers behind a recorded decision, [MOD-ADR-002 on the migration pattern](../decisions/MOD-ADR-002-migration-pattern.md), and the reasoning there is not restated here. A reader entering the assessment for the first time should start at [the section index](../README.md), which publishes a reading path for each audience.

**Reading the citations.** Every claim about LIFE400 carries an inline citation of the form `[<path>:<locator>]` immediately after the claim. Citations are plain text and deliberately not hyperlinks: a plain-text citation stays meaningful in a diff and does not depend on a hosting provider's line-anchor syntax. Open the member to read the construct in place. No member is annotated, altered, reformatted or commented by this documentation set — the estate is read as evidence and left exactly as it is. Platform terms link to [the IBM i glossary](../reference/glossary-ibm-i.md) on first use in this document.

## How this register was extracted

Nothing below is carried over from prior description. Each figure was taken from the working tree by the method named here, so any reader can re-derive it.

- **Identity, version, authorship and change notes** come from the fixed comment banner at the head of every member. The banner is a house convention applied consistently across all three languages, with one difference worth knowing before reading a citation: [ILE](../reference/glossary-ibm-i.md#ile-integrated-language-environment) COBOL and copybook banners put the author on the fourth line, the creation date on the fifth and the version on the sixth [QCBLLESRC/NBUWMNT.cbl:L1-L20], [QCPYSRC/POLDATA.cpy:L1-L13]; [ILE CL](../reference/glossary-ibm-i.md#cl-control-language) banners use the same offsets inside CL comment delimiters [QCLSRC/DLYUPD.clle:L1-L23]; and [DDS](../reference/glossary-ibm-i.md#dds-data-description-specifications) banners carry an extra `TYPE:` line, which shifts author, date and version down to the fifth, sixth and seventh lines [QDDSSRC/POLMST.pf:L1-L13].
- **Change notes** are transcribed, never inferred. A member whose banner records no change is recorded here as having none rather than being given a plausible one, and the four members in that position are named explicitly below.
- **Line counts** are whole-file counts, including banner and comment lines, because the banner is part of the member a maintainer opens and reads. Every one of the 24 members ends with a newline, so no count is ambiguous by one line. The count is independently checkable from a citation: the menu program's register row states 95 lines, and the range citation [QCBLLESRC/MAINMENU.cbl:L1-L95] resolves only against a file that is at least that long.
- **Member set.** The estate is 24 source members in four directories that stand in for the platform's four source physical files [README.md:L127-L130]. The repository's own structure diagram enumerates the same 24 — one copybook [README.md:L29], ten DDS members [README.md:L31-L40], eight ILE COBOL members [README.md:L42-L49] and five ILE CL members [README.md:L51-L55] — so the register and the entry-point document agree on the member set, not merely on its size.
- **No generated inventory was consulted.** The machine-generated corpus under `.swm/` is treated as prior art and read only where it is cited as such; it is never edited, and none of the figures in this register is taken from it.

## Member register

The register is one row per member, in library order, presented as two tables over the same member key so that every column stays readable rather than scrolling out of view. The first gives what each member is and how large it is; the second gives what its banner declares about version, authorship and dates.

`Type` is the artifact type. The online-versus-batch discriminator comes from each COBOL member's own banner description and matches the vocabulary of the README's domain table [README.md:L13-L17]; what each program actually does, and which other members it calls, belongs to [the current-state architecture](02-architecture-current-state.md). The `Banner` column cites the banner block that establishes every declared fact about that member in either table, so each row is verifiable on its own.

| Member | Type | Lines | Banner |
|--------|------|-------|--------|
| `QCBLLESRC/MAINMENU.cbl` | ILE COBOL, online | 95 | [QCBLLESRC/MAINMENU.cbl:L1-L19] |
| `QCBLLESRC/NBUWMNT.cbl` | ILE COBOL, online | 498 | [QCBLLESRC/NBUWMNT.cbl:L1-L20] |
| `QCBLLESRC/NBUWB.cbl` | ILE COBOL, batch | 507 | [QCBLLESRC/NBUWB.cbl:L1-L37] |
| `QCBLLESRC/SVCMNT.cbl` | ILE COBOL, online | 448 | [QCBLLESRC/SVCMNT.cbl:L1-L20] |
| `QCBLLESRC/SVCBILB.cbl` | ILE COBOL, batch | 543 | [QCBLLESRC/SVCBILB.cbl:L1-L35] |
| `QCBLLESRC/CLMMNT.cbl` | ILE COBOL, online | 291 | [QCBLLESRC/CLMMNT.cbl:L1-L19] |
| `QCBLLESRC/CLMADJB.cbl` | ILE COBOL, batch | 314 | [QCBLLESRC/CLMADJB.cbl:L1-L29] |
| `QCBLLESRC/POLMSTINQ.cbl` | ILE COBOL, online | 131 | [QCBLLESRC/POLMSTINQ.cbl:L1-L20] |
| `QCLSRC/STRTLIFE.clle` | ILE CL | 43 | [QCLSRC/STRTLIFE.clle:L1-L17] |
| `QCLSRC/RUNNBUW.clle` | ILE CL | 58 | [QCLSRC/RUNNBUW.clle:L1-L20] |
| `QCLSRC/RUNSVC.clle` | ILE CL | 65 | [QCLSRC/RUNSVC.clle:L1-L20] |
| `QCLSRC/RUNCLM.clle` | ILE CL | 65 | [QCLSRC/RUNCLM.clle:L1-L20] |
| `QCLSRC/DLYUPD.clle` | ILE CL | 105 | [QCLSRC/DLYUPD.clle:L1-L23] |
| `QCPYSRC/POLDATA.cpy` | [Copybook](../reference/glossary-ibm-i.md#copybook) | 175 | [QCPYSRC/POLDATA.cpy:L1-L13] |
| `QDDSSRC/POLMST.pf` | DDS [physical file](../reference/glossary-ibm-i.md#physical-file) | 81 | [QDDSSRC/POLMST.pf:L1-L13] |
| `QDDSSRC/POLMSTL1.lf` | DDS [logical file](../reference/glossary-ibm-i.md#logical-file) | 14 | [QDDSSRC/POLMSTL1.lf:L1-L12] |
| `QDDSSRC/SVCPF.pf` | DDS physical file | 54 | [QDDSSRC/SVCPF.pf:L1-L13] |
| `QDDSSRC/CLMPF.pf` | DDS physical file | 67 | [QDDSSRC/CLMPF.pf:L1-L13] |
| `QDDSSRC/MNUDSPF.dspf` | DDS [display file](../reference/glossary-ibm-i.md#display-file) | 55 | [QDDSSRC/MNUDSPF.dspf:L1-L11] |
| `QDDSSRC/NBUWDSPF.dspf` | DDS display file | 147 | [QDDSSRC/NBUWDSPF.dspf:L1-L13] |
| `QDDSSRC/SVCDSPF.dspf` | DDS display file | 129 | [QDDSSRC/SVCDSPF.dspf:L1-L13] |
| `QDDSSRC/CLMDSPF.dspf` | DDS display file | 115 | [QDDSSRC/CLMDSPF.dspf:L1-L12] |
| `QDDSSRC/POLRPT.prtf` | DDS [printer file](../reference/glossary-ibm-i.md#printer-file) | 66 | [QDDSSRC/POLRPT.prtf:L1-L11] |
| `QDDSSRC/CLMRPT.prtf` | DDS printer file | 60 | [QDDSSRC/CLMRPT.prtf:L1-L11] |

The same 24 members again, with the version, authorship and dates their banners declare. Every cell below is established by the same banner block cited in the table above, at the fixed offsets given in [How this register was extracted](#how-this-register-was-extracted) — author, creation date and version on three consecutive lines, change notes immediately beneath them — and every `Modified` value is cited to its own banner line, one row per note, in [Recorded change history](#recorded-change-history).

| Member | Version | Author | Created | Modified |
|--------|---------|--------|---------|----------|
| `QCBLLESRC/MAINMENU.cbl` | 1.2 | R. KOWALSKI | 1997-09-12 | 1998-04-20 |
| `QCBLLESRC/NBUWMNT.cbl` | 1.4 | R. KOWALSKI | 1997-09-15 | 1998-11-14 |
| `QCBLLESRC/NBUWB.cbl` | 1.3 | R. KOWALSKI | 1997-09-15 | 1998-11-14 |
| `QCBLLESRC/SVCMNT.cbl` | 1.1 | R. KOWALSKI | 1998-03-10 | 1998-11-14 |
| `QCBLLESRC/SVCBILB.cbl` | 1.1 | R. KOWALSKI | 1998-03-05 | 1998-11-14 |
| `QCBLLESRC/CLMMNT.cbl` | 1.2 | D. BRENNAN | 1997-11-10 | 1998-11-14 |
| `QCBLLESRC/CLMADJB.cbl` | 1.2 | D. BRENNAN | 1997-11-02 | 1998-11-14 |
| `QCBLLESRC/POLMSTINQ.cbl` | 1.0 | T. WALSH | 1998-01-20 | 1998-11-14 |
| `QCLSRC/STRTLIFE.clle` | 1.1 | R. KOWALSKI | 1997-09-12 | 1998-04-20 |
| `QCLSRC/RUNNBUW.clle` | 1.2 | R. KOWALSKI | 1997-09-15 | 1998-11-14 |
| `QCLSRC/RUNSVC.clle` | 1.0 | R. KOWALSKI | 1998-03-10 | 1998-11-14 |
| `QCLSRC/RUNCLM.clle` | 1.1 | D. BRENNAN | 1997-11-10 | 1998-11-14 |
| `QCLSRC/DLYUPD.clle` | 1.2 | R. KOWALSKI | 1998-03-20 | 1998-11-14 |
| `QCPYSRC/POLDATA.cpy` | 1.4 | R. KOWALSKI | 1997-09-12 | 1998-11-14 |
| `QDDSSRC/POLMST.pf` | 1.2 | R. KOWALSKI | 1997-09-12 | 1998-11-14 |
| `QDDSSRC/POLMSTL1.lf` | 1.0 | R. KOWALSKI | 1997-09-12 | — |
| `QDDSSRC/SVCPF.pf` | 1.0 | R. KOWALSKI | 1998-03-05 | 1998-11-14 |
| `QDDSSRC/CLMPF.pf` | 1.1 | D. BRENNAN | 1997-11-02 | 1998-11-14 |
| `QDDSSRC/MNUDSPF.dspf` | 1.2 | R. KOWALSKI | 1997-09-12 | — |
| `QDDSSRC/NBUWDSPF.dspf` | 1.3 | R. KOWALSKI | 1997-09-15 | 1998-11-14 |
| `QDDSSRC/SVCDSPF.dspf` | 1.1 | R. KOWALSKI | 1998-03-10 | 1998-11-14 |
| `QDDSSRC/CLMDSPF.dspf` | 1.2 | D. BRENNAN | 1997-11-10 | 1998-11-14 |
| `QDDSSRC/POLRPT.prtf` | 1.1 | R. KOWALSKI | 1997-09-12 | — |
| `QDDSSRC/CLMRPT.prtf` | 1.0 | D. BRENNAN | 1997-11-02 | — |

Five observations follow from the register itself, each measurable from the rows above.

- **Three authors of record, unevenly distributed.** Counted across the register above, R. KOWALSKI is named on 17 of the 24 members, D. BRENNAN on 6 and T. WALSH on 1 [QCBLLESRC/POLMSTINQ.cbl:L4]. The six D. BRENNAN members are exactly the claims domain end to end — batch program, online program, submitter, physical file, display file and printer file [QCBLLESRC/CLMADJB.cbl:L4], [QCBLLESRC/CLMMNT.cbl:L4], [QCLSRC/RUNCLM.clle:L4], [QDDSSRC/CLMPF.pf:L5], [QDDSSRC/CLMDSPF.dspf:L5], [QDDSSRC/CLMRPT.prtf:L5]. What that concentration implies for hiring and knowledge transfer is assessed in [the skills inventory and gap analysis](../talent/01-skills-inventory-and-gap.md); the register only establishes that it is so.
- **No member ever reached a second major version.** The declared versions are 1.0 on five members, 1.1 on seven, 1.2 on eight, 1.3 on two and 1.4 on two, and the highest anywhere in the estate is 1.4 [QCBLLESRC/NBUWMNT.cbl:L6], [QCPYSRC/POLDATA.cpy:L6].
- **Members arrive in domain cohorts on a single date.** Seven members share the creation date 1997-09-12 — the menu program, the session entry program, the shared contract, the policy master, its access path, the menu screen and the policy listing report [QCBLLESRC/MAINMENU.cbl:L5], [QCLSRC/STRTLIFE.clle:L5], [QCPYSRC/POLDATA.cpy:L5], [QDDSSRC/POLMST.pf:L6], [QDDSSRC/POLMSTL1.lf:L6], [QDDSSRC/MNUDSPF.dspf:L6], [QDDSSRC/POLRPT.prtf:L6]. The same pattern repeats per domain: new business on 1997-09-15 across four members [QCBLLESRC/NBUWB.cbl:L5], [QCBLLESRC/NBUWMNT.cbl:L5], [QCLSRC/RUNNBUW.clle:L5], [QDDSSRC/NBUWDSPF.dspf:L6]; claims across 1997-11-02 [QCBLLESRC/CLMADJB.cbl:L5], [QDDSSRC/CLMPF.pf:L6], [QDDSSRC/CLMRPT.prtf:L6] and 1997-11-10 [QCBLLESRC/CLMMNT.cbl:L5], [QCLSRC/RUNCLM.clle:L5], [QDDSSRC/CLMDSPF.dspf:L6]; and servicing across 1998-03-05 [QCBLLESRC/SVCBILB.cbl:L5], [QDDSSRC/SVCPF.pf:L6] and 1998-03-10 [QCBLLESRC/SVCMNT.cbl:L5], [QCLSRC/RUNSVC.clle:L5], [QDDSSRC/SVCDSPF.dspf:L6].
- **The newest member of the estate is the nightly job.** `QCLSRC/DLYUPD.clle` declares the latest creation date of any member, 1998-03-20 [QCLSRC/DLYUPD.clle:L5]; no member declares a creation date after it.
- **The read-only inquiry program is the estate's one outlier on every axis of the register.** `QCBLLESRC/POLMSTINQ.cbl` is the only member attributed to a third author, the only COBOL program still at version 1.0, and the only COBOL program whose banner declares input-only access to the policy master rather than update access [QCBLLESRC/POLMSTINQ.cbl:L4-L6], [QCBLLESRC/POLMSTINQ.cbl:L18].

## Recorded change history

The banners record 33 dated change notes across the estate. They are the only change history the repository carries for these members, so they are reproduced here in full rather than summarised: each note names what was changed, and together they are the closest thing to a release history a migration team will find. Note ordering is not uniform — the COBOL and copybook banners list the newest note first, while every CL banner and the new business display file list notes oldest-first [QCPYSRC/POLDATA.cpy:L7-L9], [QCLSRC/DLYUPD.clle:L7-L8], [QDDSSRC/NBUWDSPF.dspf:L8-L9] — which is why the register's `Modified` column carries the newest date rather than the first note encountered.

| Member | Banner line | Recorded change |
|--------|-------------|-----------------|
| `QCBLLESRC/MAINMENU.cbl` | [QCBLLESRC/MAINMENU.cbl:L7] | `1998-04-20 - ADDED REPORTS MENU OPTION` |
| `QCBLLESRC/NBUWMNT.cbl` | [QCBLLESRC/NBUWMNT.cbl:L7] | `1998-11-14 - Y2K DATE FIELD REVIEW` |
| `QCBLLESRC/NBUWMNT.cbl` | [QCBLLESRC/NBUWMNT.cbl:L8] | `1998-04-10 - ADDED RIDER ENTRY SECTION` |
| `QCBLLESRC/NBUWMNT.cbl` | [QCBLLESRC/NBUWMNT.cbl:L9] | `1998-01-08 - ADDED T6501 PLAN SUPPORT` |
| `QCBLLESRC/NBUWB.cbl` | [QCBLLESRC/NBUWB.cbl:L7] | `1998-11-14 - Y2K DATE FIELD REVIEW` |
| `QCBLLESRC/NBUWB.cbl` | [QCBLLESRC/NBUWB.cbl:L8] | `1998-05-20 - ADDED REINSURANCE REFERRAL LOGIC` |
| `QCBLLESRC/NBUWB.cbl` | [QCBLLESRC/NBUWB.cbl:L9] | `1998-01-08 - ADDED T6501 PLAN CODE` |
| `QCBLLESRC/SVCMNT.cbl` | [QCBLLESRC/SVCMNT.cbl:L7] | `1998-11-14 - Y2K DATE FIELD REVIEW` |
| `QCBLLESRC/SVCMNT.cbl` | [QCBLLESRC/SVCMNT.cbl:L8] | `1998-07-22 - ADDED F10=REINSTATE SUPPORT` |
| `QCBLLESRC/SVCBILB.cbl` | [QCBLLESRC/SVCBILB.cbl:L7] | `1998-11-14 - Y2K DATE FIELD REVIEW` |
| `QCBLLESRC/SVCBILB.cbl` | [QCBLLESRC/SVCBILB.cbl:L8] | `1998-07-22 - ADDED REINSTATEMENT LOGIC` |
| `QCBLLESRC/CLMMNT.cbl` | [QCBLLESRC/CLMMNT.cbl:L7] | `1998-11-14 - Y2K DATE FIELD REVIEW` |
| `QCBLLESRC/CLMMNT.cbl` | [QCBLLESRC/CLMMNT.cbl:L8] | `1998-02-15 - ADDED ADB PAYOUT FOR ACCIDENTS` |
| `QCBLLESRC/CLMADJB.cbl` | [QCBLLESRC/CLMADJB.cbl:L7] | `1998-11-14 - Y2K DATE FIELD REVIEW` |
| `QCBLLESRC/CLMADJB.cbl` | [QCBLLESRC/CLMADJB.cbl:L8] | `1998-02-15 - ADDED ADB RIDER PAYOUT LOGIC` |
| `QCBLLESRC/POLMSTINQ.cbl` | [QCBLLESRC/POLMSTINQ.cbl:L7] | `1998-11-14 - Y2K DATE FIELD REVIEW` |
| `QCLSRC/STRTLIFE.clle` | [QCLSRC/STRTLIFE.clle:L7] | `1998-04-20 - ADD LIFE400 TO LIB LIST IF MISSING` |
| `QCLSRC/RUNNBUW.clle` | [QCLSRC/RUNNBUW.clle:L7] | `1998-06-10 - ADDED JOBLOG CAPTURE TO OUTQ` |
| `QCLSRC/RUNNBUW.clle` | [QCLSRC/RUNNBUW.clle:L8] | `1998-11-14 - Y2K: PROCESS DATE FROM SYSTEM` |
| `QCLSRC/RUNSVC.clle` | [QCLSRC/RUNSVC.clle:L7] | `1998-11-14 - Y2K: PROCESS DATE FROM SYSTEM` |
| `QCLSRC/RUNCLM.clle` | [QCLSRC/RUNCLM.clle:L7] | `1998-11-14 - Y2K: PROCESS DATE FROM SYSTEM` |
| `QCLSRC/DLYUPD.clle` | [QCLSRC/DLYUPD.clle:L7] | `1998-09-15 - ADDED LAPSE TRANSITION SWEEP` |
| `QCLSRC/DLYUPD.clle` | [QCLSRC/DLYUPD.clle:L8] | `1998-11-14 - Y2K: PROCESS DATE FROM SYSTEM` |
| `QCPYSRC/POLDATA.cpy` | [QCPYSRC/POLDATA.cpy:L7] | `1998-11-14 - Y2K DATE FIELD REVIEW` |
| `QCPYSRC/POLDATA.cpy` | [QCPYSRC/POLDATA.cpy:L8] | `1998-03-05 - ADDED SERVICING DETAILS SECTION` |
| `QCPYSRC/POLDATA.cpy` | [QCPYSRC/POLDATA.cpy:L9] | `1997-11-02 - ADDED CLAIM DETAILS SECTION` |
| `QDDSSRC/POLMST.pf` | [QDDSSRC/POLMST.pf:L8] | `1998-11-14 - Y2K DATE FIELD REVIEW` |
| `QDDSSRC/SVCPF.pf` | [QDDSSRC/SVCPF.pf:L8] | `1998-11-14 - Y2K DATE FIELD REVIEW` |
| `QDDSSRC/CLMPF.pf` | [QDDSSRC/CLMPF.pf:L8] | `1998-11-14 - Y2K DATE FIELD REVIEW` |
| `QDDSSRC/NBUWDSPF.dspf` | [QDDSSRC/NBUWDSPF.dspf:L8] | `1998-04-10 - ADDED RIDER SECTION` |
| `QDDSSRC/NBUWDSPF.dspf` | [QDDSSRC/NBUWDSPF.dspf:L9] | `1998-11-14 - Y2K DATE FIELD REVIEW` |
| `QDDSSRC/SVCDSPF.dspf` | [QDDSSRC/SVCDSPF.dspf:L8] | `1998-11-14 - Y2K DATE FIELD REVIEW` |
| `QDDSSRC/CLMDSPF.dspf` | [QDDSSRC/CLMDSPF.dspf:L8] | `1998-11-14 - Y2K DATE FIELD REVIEW` |

Four facts about that history matter to anyone planning work against these members.

- **Four of the 24 members record no change whatsoever.** The policy master access path [QDDSSRC/POLMSTL1.lf:L1-L12], the menu screen [QDDSSRC/MNUDSPF.dspf:L1-L11] and both printer files [QDDSSRC/POLRPT.prtf:L1-L11], [QDDSSRC/CLMRPT.prtf:L1-L11] carry a creation banner and nothing after it. The remaining 20 members carry at least one dated note.
- **Twenty-two of the 33 notes are introduced by the `MODIFIED:` keyword; the other 11 are continuation lines beneath it.** Both forms are equally authoritative, and a search for the keyword alone therefore under-reports the history — for example the T6501 plan change is recorded on a continuation line in both new business members [QCBLLESRC/NBUWB.cbl:L9], [QCBLLESRC/NBUWMNT.cbl:L9].
- **The shared contract grew on the same dates as the domains it serves.** The copybook records a claim details section added on 1997-11-02 [QCPYSRC/POLDATA.cpy:L9], the date the claims physical file and batch program were created [QDDSSRC/CLMPF.pf:L6], [QCBLLESRC/CLMADJB.cbl:L5]; and a servicing details section added on 1998-03-05 [QCPYSRC/POLDATA.cpy:L8], the date the servicing physical file and batch program were created [QDDSSRC/SVCPF.pf:L6], [QCBLLESRC/SVCBILB.cbl:L5].
- **Paired members record the same change on the same date.** The T6501 plan change is recorded in both the online and the batch new business member, each with its own wording, on 1998-01-08 [QCBLLESRC/NBUWMNT.cbl:L9], [QCBLLESRC/NBUWB.cbl:L9]; reinstatement support is recorded in both servicing members on 1998-07-22 [QCBLLESRC/SVCMNT.cbl:L8], [QCBLLESRC/SVCBILB.cbl:L8]; and accidental-death rider payout is recorded in both claims members on 1998-02-15 [QCBLLESRC/CLMMNT.cbl:L8], [QCBLLESRC/CLMADJB.cbl:L8]. The register records the pairing as a fact of the change history. What the pairing means for the code — whether the two members duplicate logic and must be maintained in step — is analysed in [the current-state architecture](02-architecture-current-state.md), which owns that finding.

## Language distribution

LIFE400 is 4,126 lines of source across 24 members in three languages plus one shared copybook.

| Language | Members | Lines | Share of estate |
|----------|---------|-------|-----------------|
| ILE COBOL | 8 | 2,827 | 68.5% |
| ILE CL | 5 | 336 | 8.1% |
| COBOL copybook | 1 | 175 | 4.2% |
| DDS | 10 | 788 | 19.1% |
| **Total** | **24** | **4,126** | **100.0%** |

The subtotals are stated with their addends so the total can be re-derived rather than taken on trust. Every addend is the whole-file line count of the member named in the register above.

```text
ILE COBOL   314 + 291 + 95 + 507 + 498 + 131 + 543 + 448        = 2,827
ILE CL      105 + 65 + 58 + 65 + 43                             =   336
Copybook    175                                                 =   175
DDS         115 + 67 + 60 + 55 + 147 + 81 + 14 + 66 + 129 + 54  =   788
                                                                  -----
Estate total                                                    = 4,126
```

DDS is a single fixed-format language serving four distinct purposes in this estate, so its ten members are broken out by kind. The four data members define what is stored; the six device members define what is shown and printed.

| DDS kind | Members | Members by name | Lines |
|----------|---------|-----------------|-------|
| Physical file | 3 | `POLMST.pf`, `SVCPF.pf`, `CLMPF.pf` | 202 |
| Logical file | 1 | `POLMSTL1.lf` | 14 |
| Display file | 4 | `MNUDSPF.dspf`, `NBUWDSPF.dspf`, `SVCDSPF.dspf`, `CLMDSPF.dspf` | 446 |
| Printer file | 2 | `POLRPT.prtf`, `CLMRPT.prtf` | 126 |
| **Total** | **10** | | **788** |

Two things about the distribution are worth stating plainly, because both bear on how much of the estate a modern-language engineer can read on day one.

- **Presentation outweighs persistence in the DDS layer.** Summing the table above, the four display files and two printer files account for 572 of the 788 DDS lines, against 216 lines for all four data members combined — the single largest device member is larger than the entire storage definition [QDDSSRC/NBUWDSPF.dspf:L1-L147], [QDDSSRC/POLMST.pf:L1-L81].
- **The estate declares its own platform lock in every COBOL member.** All eight name `IBM-AS400` as both source computer and object computer [QCBLLESRC/MAINMENU.cbl:L27-L28], and the declared runtime baseline is `ILE COBOL V3R7 · OS/400 V4R2 · IBM AS/400 Model 9406` [README.md:L264]. The 4,126-line figure above is the one this register hands forward to [MOD-ADR-002 on the migration pattern](../decisions/MOD-ADR-002-migration-pattern.md) as a decision driver; what the baseline implies for supportability is assessed in [the platform and support status document](03-platform-and-support-status.md), which owns that conclusion.

## Object-count reconciliation

Members and objects are not the same thing, and the difference matters when scoping a migration: 24 members produce 23 independently compiled objects, because one member is compiled into no object of its own. The documented build procedure is the authority for the mapping, and every command it issues is accounted for below.

| Build step | Commands issued | Objects created | Evidence |
|------------|-----------------|-----------------|----------|
| Create database files | 3 × `CRTPF`, 1 × `CRTLF` | 3 physical files, 1 logical file | [README.md:L140-L142], [README.md:L143] |
| Create display and printer files | 4 × `CRTDSPF`, 2 × `CRTPRTF` | 4 display files, 2 printer files | [README.md:L149-L152], [README.md:L153-L154] |
| Compile ILE COBOL programs | 8 × `CRTCBLMOD`, then 8 × `CRTPGM` | 8 program objects, from 8 modules | [README.md:L160-L167], [README.md:L169-L176] |
| Compile ILE CL programs | 5 × `CRTCLMOD`, then 5 × `CRTPGM` | 5 program objects, from 5 modules | [README.md:L182-L186], [README.md:L188-L192] |

```text
database file objects   3 physical + 1 logical                      =  4
device file objects     4 display  + 2 printer                      =  6
ILE COBOL programs      8 modules bound into 8 programs             =  8
ILE CL programs         5 modules bound into 5 programs             =  5
                                                                      ---
independently compiled objects                                      = 23
copybook                textually included, compiled to no object   =  0
                                                                      ---
source members accounted for                                        = 24
```

Four qualifications keep that arithmetic honest.

- **The copybook is included, not compiled.** `QCPYSRC/POLDATA.cpy` appears in no build step, because it is pulled into its consumers textually at compile time by a single statement: `COPY POLDATA.` in seven of the eight COBOL members [QCBLLESRC/NBUWMNT.cbl:L50], [QCBLLESRC/NBUWB.cbl:L60], [QCBLLESRC/SVCMNT.cbl:L56], [QCBLLESRC/SVCBILB.cbl:L64], [QCBLLESRC/CLMMNT.cbl:L55], [QCBLLESRC/CLMADJB.cbl:L58], [QCBLLESRC/POLMSTINQ.cbl:L50]. The eighth, the menu program, does not copy it and declares no database file at all — only its display file [QCBLLESRC/MAINMENU.cbl:L32-L36].
- **The copybook's own banner under-reports its consumers.** It names six programs as users — the two new business members, the two servicing members and the two claims members [QCPYSRC/POLDATA.cpy:L12] — and omits the inquiry program, which copies it nonetheless [QCBLLESRC/POLMSTINQ.cbl:L50]. Seven members copy it; the banner lists six.
- **Thirteen module objects exist as intermediates.** The two-step ILE build creates a module and then binds it into a program, so 13 module objects are produced on the way to the 13 program objects and are not additional deployable components [README.md:L160-L167], [README.md:L182-L186].
- **Four runtime objects have no source member in this repository.** A [job description](../reference/glossary-ibm-i.md#job-description), a [job queue](../reference/glossary-ibm-i.md#job-queue), an [output queue](../reference/glossary-ibm-i.md#output-queue) and a [message queue](../reference/glossary-ibm-i.md#message-queue) are created by command rather than compiled from source, alongside the scheduler entry for the nightly job [README.md:L197-L212]. They are therefore outside the 23 and outside the 24, and they are absent from any register that counts only members. Their runtime roles belong to [the operational model](06-operational-model.md).

Persistent structure is defined entirely by DDS. The four data members named in the register — three physical files and one logical file — are the whole of the estate's storage definition, and no other definition of stored structure exists anywhere in the tree [QDDSSRC/POLMST.pf:L14], [QDDSSRC/SVCPF.pf:L14], [QDDSSRC/CLMPF.pf:L14], [QDDSSRC/POLMSTL1.lf:L13].

### Documentation coverage before this assessment

The estate was not undocumented, but its documentation was uneven in a way the member counts make precise. Nine machine-generated walkthroughs exist under `.swm/`, named and counted in the corpus's own statistics file [.swm/business-rules-statistics.md:L6-L14]. Eight of the nine describe one COBOL program each; the ninth is a scenario document describing a plan-change journey rather than a member, and it is credited with no rules at all [.swm/business-rules-statistics.md:L6].

| Member group | Members | Had a dedicated document | Coverage |
|--------------|---------|--------------------------|----------|
| ILE COBOL programs | 8 | 8 | 100% |
| ILE CL programs | 5 | 0 | 0% |
| COBOL copybook | 1 | 0 | 0% |
| DDS members | 10 | 0 | 0% |
| **Total** | **24** | **8** | **33%** |

- **Sixteen members receive their first dedicated documentation in this layer** — all five CL members, the shared copybook and all ten DDS members. Nothing in the prior corpus describes the session entry program [QCLSRC/STRTLIFE.clle:L1-L17], the three batch submitters, the nightly job [QCLSRC/DLYUPD.clle:L1-L23], the record layout every program shares [QCPYSRC/POLDATA.cpy:L14], or any stored or displayed structure.
- **Screen coverage was partial rather than absent.** The README's screen reference documents 14 [record formats](../reference/glossary-ibm-i.md#record-format) across the four display files [README.md:L226-L239]. The six device members declare 28 formats between them — two in the menu display file [QDDSSRC/MNUDSPF.dspf:L19], [QDDSSRC/MNUDSPF.dspf:L50], six in the new business display file [QDDSSRC/NBUWDSPF.dspf:L24-L137], five each in the servicing and claims display files [QDDSSRC/SVCDSPF.dspf:L23-L117], [QDDSSRC/CLMDSPF.dspf:L21-L109], and five in each printer file [QDDSSRC/POLRPT.prtf:L15-L64], [QDDSSRC/CLMRPT.prtf:L15-L58] — so 14 of 28 were described and 14 were not. The format-by-format inventory and the disposition of each format are owned by [the UI modernization document](../target-state/05-ui-modernization.md).
- **The prior corpus is prior art and stays that way.** It is machine-generated, so it is cited where it is used and never hand-edited; no figure in this register is taken from it, and its proprietary path and token tags are not reproduced anywhere in this documentation set [.swm/polmstinq-policy-master-inquiry.4dyvkwri.sw.md:L1-L3].

## Last change of record

The latest date recorded anywhere in the estate is `1998-11-14`, and it is recorded as a Y2K review. No member carries a dated annotation later than it, in a banner or anywhere in procedural code. That single date is the most-cited fact in this whole assessment, and it is also the easiest to overstate, because the review left **three different annotation forms** in the source and each one touches a different number of members. The counts below are not interchangeable.

| Annotation form | Where it is written | Occurrences | Members carrying it | Example |
|-----------------|---------------------|-------------|---------------------|---------|
| Inline marker `*Y2K-REVIEWED 1998-11-14` | In procedural code, beside the date logic it certifies | 17 | 9 — the 8 COBOL programs and the copybook | [QCBLLESRC/SVCBILB.cbl:L136] |
| Banner note `MODIFIED: 1998-11-14 - Y2K DATE FIELD REVIEW` | In the member banner | 14 | 14 — 7 COBOL programs, the copybook and 6 DDS members | [QDDSSRC/POLMST.pf:L8] |
| Banner note `MODIFIED: 1998-11-14 - Y2K: PROCESS DATE FROM SYSTEM` | In the CL member banner | 4 | 4 — the nightly job and the 3 batch submitters | [QCLSRC/DLYUPD.clle:L8] |

**The union is 19 of the 24 members.** The three member counts do not sum to 19 because members carry more than one form: every COBOL program and the copybook carry the inline marker, and all of those except the menu program also carry the banner note. Two consequences need stating in exact terms, because conflating them is the easiest error to make about this system.

- **Nineteen of 24 members carry a dated 1998-11-14 Y2K review annotation** in one form or another.
- **The literal string `*Y2K-REVIEWED` appears in 9 members, not 19.** It is absent from every CL member and every DDS member. A reader who takes the inline-marker form as estate-wide will be wrong by ten members.

All 17 inline-marker sites are listed below, so the count can be checked exhaustively rather than sampled.

| Member | Inline marker lines |
|--------|---------------------|
| `QCBLLESRC/MAINMENU.cbl` | [QCBLLESRC/MAINMENU.cbl:L54] |
| `QCBLLESRC/NBUWMNT.cbl` | [QCBLLESRC/NBUWMNT.cbl:L97] |
| `QCBLLESRC/NBUWB.cbl` | [QCBLLESRC/NBUWB.cbl:L133], [QCBLLESRC/NBUWB.cbl:L493] |
| `QCBLLESRC/SVCMNT.cbl` | [QCBLLESRC/SVCMNT.cbl:L107] |
| `QCBLLESRC/SVCBILB.cbl` | [QCBLLESRC/SVCBILB.cbl:L136], [QCBLLESRC/SVCBILB.cbl:L188], [QCBLLESRC/SVCBILB.cbl:L197], [QCBLLESRC/SVCBILB.cbl:L401] |
| `QCBLLESRC/CLMMNT.cbl` | [QCBLLESRC/CLMMNT.cbl:L106] |
| `QCBLLESRC/CLMADJB.cbl` | [QCBLLESRC/CLMADJB.cbl:L132], [QCBLLESRC/CLMADJB.cbl:L203] |
| `QCBLLESRC/POLMSTINQ.cbl` | [QCBLLESRC/POLMSTINQ.cbl:L89] |
| `QCPYSRC/POLDATA.cpy` | [QCPYSRC/POLDATA.cpy:L19], [QCPYSRC/POLDATA.cpy:L56], [QCPYSRC/POLDATA.cpy:L109], [QCPYSRC/POLDATA.cpy:L174] |

Five members carry no 1998-11-14 annotation of any kind, and their banners end at creation: the session entry program [QCLSRC/STRTLIFE.clle:L1-L17], the policy master access path [QDDSSRC/POLMSTL1.lf:L1-L12], the menu screen [QDDSSRC/MNUDSPF.dspf:L1-L11], and both printer files [QDDSSRC/POLRPT.prtf:L1-L11], [QDDSSRC/CLMRPT.prtf:L1-L11]. Four of those five are also the four members that record no change note whatsoever; the session entry program is the exception, recording one earlier change to its [library list](../reference/glossary-ibm-i.md#library-list) handling [QCLSRC/STRTLIFE.clle:L7].

### What "last change of record" does and does not mean

This distinction is load-bearing for the whole assessment, so it is stated once, precisely, here.

- **What it means.** `1998-11-14` is the newest change *of record*: the latest date any banner or inline comment in the 24 members carries. The estate's own documented change history ends there, and it ends with a compliance review rather than a functional change — the README records that the programs were reviewed in November 1998 and points the reader at the inline comments as the evidence [README.md:L258-L259].
- **What it does not mean.** It is not a claim about when the files on disk were last written. The repository's own front matter records `Original Build: March 24 2026 (mimics 1997–1998)` [README.md:L5], so this tree is a faithful reconstruction of that history rather than an artifact continuously edited since it. Both statements are true at once, and neither should be read as the other. Every date in this register is quoted evidence from a member's banner, not an assertion about file-system timestamps.
- **Why it still matters.** The declared runtime baseline is a separate fact from the change history and is recorded separately [README.md:L264]. What that baseline implies about supportability and security is assessed in [the platform and support status document](03-platform-and-support-status.md), which owns that conclusion; this register only fixes the dates it rests on.

## Figures owned by other documents

This register owns the member set, the per-member banner facts, the language distribution, the object reconciliation and the coverage baseline. Every other quantity in the assessment has exactly one owning document, and restating one here would create a second place for it to be wrong. Look those up at the owner instead.

- Paragraph inventories, the call and dispatch graph, and the online-to-batch duplication analysis — [the current-state architecture](02-architecture-current-state.md).
- Platform support status and the consequence of the declared baseline — [the platform and support status document](03-platform-and-support-status.md).
- Column counts, elementary-item counts, level-88 domains and the persisted-versus-transient classification — [the current data model](04-data-model-current-state.md).
- The business-rule census, the inline rule-identifier bands and how many rules carry a source anchor — [the business rule inventory](05-business-rule-inventory.md).
- Work-management object roles, the message vocabulary, queue behaviour and the build sequence in operational terms — [the operational model](06-operational-model.md).
- Defect, stub and inert-feature dispositions — [the known defects and stubs register](07-known-defects-and-stubs.md).
- Record-format totals and the disposition of every screen and report format — [the UI modernization document](../target-state/05-ui-modernization.md).
- The skills the estate demands and the knowledge-concentration exposure the authorship pattern creates — [the skills inventory and gap analysis](../talent/01-skills-inventory-and-gap.md).

## Source citations

Every member cited above, read as evidence and left unmodified. All 24 members of the estate appear in this register, so this list is the complete member set.

- ILE COBOL — `QCBLLESRC/MAINMENU.cbl`, `QCBLLESRC/NBUWMNT.cbl`, `QCBLLESRC/NBUWB.cbl`, `QCBLLESRC/SVCMNT.cbl`, `QCBLLESRC/SVCBILB.cbl`, `QCBLLESRC/CLMMNT.cbl`, `QCBLLESRC/CLMADJB.cbl`, `QCBLLESRC/POLMSTINQ.cbl`
- ILE CL — `QCLSRC/STRTLIFE.clle`, `QCLSRC/RUNNBUW.clle`, `QCLSRC/RUNSVC.clle`, `QCLSRC/RUNCLM.clle`, `QCLSRC/DLYUPD.clle`
- Copybook — `QCPYSRC/POLDATA.cpy`
- DDS database — `QDDSSRC/POLMST.pf`, `QDDSSRC/POLMSTL1.lf`, `QDDSSRC/SVCPF.pf`, `QDDSSRC/CLMPF.pf`
- DDS display — `QDDSSRC/MNUDSPF.dspf`, `QDDSSRC/NBUWDSPF.dspf`, `QDDSSRC/SVCDSPF.dspf`, `QDDSSRC/CLMDSPF.dspf`
- DDS printer — `QDDSSRC/POLRPT.prtf`, `QDDSSRC/CLMRPT.prtf`
- Repository overview, build procedure and screen reference — `README.md`
- Prior documentation corpus, cited as prior art and never modified — `.swm/business-rules-statistics.md`, `.swm/polmstinq-policy-master-inquiry.4dyvkwri.sw.md`
