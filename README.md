# LIFE400 — Term Life Policy System (AS/400 / IBM i)

ACME Life Insurance Co. — Term Life Policy System |
Platform: IBM AS/400 (iSeries) | ILE COBOL + DDS + ILE CL |
Original Build: March 24 2026 (mimics 1997–1998) | Library: `LIFE400`

---

## Overview

LIFE400 is the AS/400 version of the term life insurance administration system. It covers the full policy lifecycle across three business domains:

| Domain | Batch Program | Online Program | CL Submitter |
|--------|--------------|---------------|--------------|
| New Business & Underwriting | `NBUWB` | `NBUWMNT` | `RUNNBUW` |
| Policy Servicing & Billing | `SVCBILB` | `SVCMNT` | `RUNSVC` |
| Claims Adjudication | `CLMADJB` | `CLMMNT` | `RUNCLM` |

Interactive entry point: `STRTLIFE` → `MAINMENU`
Nightly batch: `DLYUPD` (scheduled via `ADDJOBSCDE`)

---

## Repository Structure

```text
LIFE400/
├── QCPYSRC/          ILE COBOL copybooks
│   └── POLDATA.cpy   Shared policy master record layout
├── QDDSSRC/          DDS source (database files, display files, printer files)
│   ├── POLMST.pf     Policy master physical file
│   ├── CLMPF.pf      Claims physical file
│   ├── SVCPF.pf      Service amendments physical file
│   ├── POLMSTL1.lf   Logical file over POLMST (keyed by POLID)
│   ├── MNUDSPF.dspf  Main menu 5250 display file
│   ├── NBUWDSPF.dspf New business entry display file
│   ├── CLMDSPF.dspf  Claims entry display file
│   ├── SVCDSPF.dspf  Servicing / inquiry display file
│   ├── POLRPT.prtf   Policy listing printer file
│   └── CLMRPT.prtf   Claims report printer file
├── QCBLLESRC/        ILE COBOL source members
│   ├── NBUWB.cbl     Batch: new business & underwriting
│   ├── CLMADJB.cbl   Batch: claims adjudication
│   ├── SVCBILB.cbl   Batch: servicing & billing
│   ├── MAINMENU.cbl  Online: main menu
│   ├── NBUWMNT.cbl   Online: new business maintenance
│   ├── CLMMNT.cbl    Online: claims maintenance
│   ├── SVCMNT.cbl    Online: servicing maintenance
│   └── POLMSTINQ.cbl Online: policy master inquiry (read-only)
└── QCLSRC/           ILE CL source members
    ├── STRTLIFE.clle  Start life system (interactive entry point)
    ├── RUNNBUW.clle   Submit NB batch job
    ├── RUNCLM.clle    Submit claims batch job
    ├── RUNSVC.clle    Submit servicing batch job
    └── DLYUPD.clle    Nightly daily update (grace/lapse sweep)
```

---

## Plans and Products

| Code | Description | Issue Ages | Min SA | Max SA |
|------|-------------|-----------|--------|--------|
| T1001 | 10-Year Term | 18–60 | 10,000,000 | 50,000,000,000 |
| T2001 | 20-Year Term | 18–55 | 10,000,000 | 90,000,000,000 |
| T6501 | Term-to-65 | 18–50 | 10,000,000 | 75,000,000,000 |

---

## Modernization

A modernization assessment for LIFE400 — current-state evidence, a target state, and a single recommended migration path — is maintained in this repository under `docs/modernization/`. It is evaluated against two business drivers:

- **Minimize security risk.** The declared platform baseline is `ILE COBOL V3R7 · OS/400 V4R2 · IBM AS/400 Model 9406` [README.md:L264], and the application delegates access control entirely to the platform: the menu dispatch loop offers every signed-on user the identical option set, with no role or authority branching anywhere in it [QCBLLESRC/MAINMENU.cbl:L60-L92].
- **Acquire engineering talent fluent in a modern mainstream language.** Maintaining LIFE400 requires ILE COBOL, DDS, and ILE CL together on one platform [README.md:L4], so the assessment names a target language and defends the choice rather than leaving it abstract.

The assessment is **documentation only**: no ILE COBOL, ILE CL, copybook, or DDS member is modified by it. `QCBLLESRC/`, `QCPYSRC/`, `QCLSRC/`, and `QDDSSRC/` are read as evidence and left unchanged.

Entry point: **[Modernization assessment](docs/modernization/README.md)**, which publishes both reading paths in full.

| Reader | Start with | Then read |
|--------|------------|-----------|
| Decision-maker | [Executive recommendation](docs/modernization/00-executive-recommendation.md) — the recommended path, the named target stack, and the rejected alternatives | [Business drivers and success criteria](docs/modernization/01-business-drivers-and-success-criteria.md), then the `risk/` and `talent/` layers |
| Engineer | [System inventory](docs/modernization/current-state/01-system-inventory.md) — the as-built member register — then the `current-state/` layer forward | `target-state/` for the destination, `migration/` for the route, and the [IBM i glossary](docs/modernization/reference/glossary-ibm-i.md) for platform terminology |

---

## Documentation

Documentation lives under `docs/`. [`docs/README.md`](docs/README.md) is the landing page, and `docs/modernization/` holds the assessment in seven layers — `current-state/`, `risk/`, `talent/`, `target-state/`, `migration/`, `decisions/`, and `reference/`. The set **describes this system without modifying it**: every claim about LIFE400 carries an inline citation to the source member, DDS member, or build step that establishes it.

Install the pinned documentation dependencies listed in `requirements-docs.txt`:

```bash
pip install -r requirements-docs.txt
```

Build the site. Navigation lives in `mkdocs.yml`, and `--strict` turns a missing navigation entry or a broken internal link into a build failure rather than a warning:

```bash
mkdocs build --strict
```

Lint the markdown against `.markdownlint-cli2.jsonc`:

```bash
npx --yes markdownlint-cli2@0.23.2 "**/*.md"
```

Check that every documentation link resolves, using `.mlc-config.json`:

```bash
find docs -name '*.md' -exec npx --yes markdown-link-check@3.15.0 --config .mlc-config.json {} \;
```

`mkdocs serve` previews the site locally. It is long-running and interactive, so it belongs in a terminal session and never in an automated step.

---

## Building on a Real AS/400 / IBM i

### Step 1 — Create the Library and Source Physical Files

```text
CRTLIB LIB(LIFE400) TYPE(*PROD) TEXT('ACME LIFE INS SYSTEM')

CRTSRCPF FILE(LIFE400/QCPYSRC)   RCDLEN(92)  TEXT('COBOL COPYBOOKS')
CRTSRCPF FILE(LIFE400/QDDSSRC)   RCDLEN(92)  TEXT('DDS SOURCE')
CRTSRCPF FILE(LIFE400/QCBLLESRC) RCDLEN(92)  TEXT('ILE COBOL SOURCE')
CRTSRCPF FILE(LIFE400/QCLSRC)    RCDLEN(92)  TEXT('ILE CL SOURCE')
```

### Step 2 — Upload Source Members

Upload each file from this repo into the corresponding source physical file using FTP or IFS copy, then use `CPYFRMSTMF` to copy into source members.

### Step 3 — Create Database Files

```text
CRTPF FILE(LIFE400/POLMST)   SRCFILE(LIFE400/QDDSSRC) SRCMBR(POLMST)   TEXT('POLICY MASTER')
CRTPF FILE(LIFE400/CLMPF)    SRCFILE(LIFE400/QDDSSRC) SRCMBR(CLMPF)    TEXT('CLAIMS')
CRTPF FILE(LIFE400/SVCPF)    SRCFILE(LIFE400/QDDSSRC) SRCMBR(SVCPF)    TEXT('SERVICE REQUESTS')
CRTLF FILE(LIFE400/POLMSTL1) SRCFILE(LIFE400/QDDSSRC) SRCMBR(POLMSTL1) TEXT('POLMST LOGICAL')
```

### Step 4 — Create Display and Printer Files

```text
CRTDSPF FILE(LIFE400/MNUDSPF)   SRCFILE(LIFE400/QDDSSRC) SRCMBR(MNUDSPF)
CRTDSPF FILE(LIFE400/NBUWDSPF)  SRCFILE(LIFE400/QDDSSRC) SRCMBR(NBUWDSPF)
CRTDSPF FILE(LIFE400/CLMDSPF)   SRCFILE(LIFE400/QDDSSRC) SRCMBR(CLMDSPF)
CRTDSPF FILE(LIFE400/SVCDSPF)   SRCFILE(LIFE400/QDDSSRC) SRCMBR(SVCDSPF)
CRTPRTF FILE(LIFE400/POLRPT)    SRCFILE(LIFE400/QDDSSRC) SRCMBR(POLRPT)
CRTPRTF FILE(LIFE400/CLMRPT)    SRCFILE(LIFE400/QDDSSRC) SRCMBR(CLMRPT)
```

### Step 5 — Compile ILE COBOL Programs

```text
CRTCBLMOD MODULE(LIFE400/NBUWB)     SRCFILE(LIFE400/QCBLLESRC) SRCMBR(NBUWB)
CRTCBLMOD MODULE(LIFE400/CLMADJB)   SRCFILE(LIFE400/QCBLLESRC) SRCMBR(CLMADJB)
CRTCBLMOD MODULE(LIFE400/SVCBILB)   SRCFILE(LIFE400/QCBLLESRC) SRCMBR(SVCBILB)
CRTCBLMOD MODULE(LIFE400/MAINMENU)  SRCFILE(LIFE400/QCBLLESRC) SRCMBR(MAINMENU)
CRTCBLMOD MODULE(LIFE400/NBUWMNT)   SRCFILE(LIFE400/QCBLLESRC) SRCMBR(NBUWMNT)
CRTCBLMOD MODULE(LIFE400/CLMMNT)    SRCFILE(LIFE400/QCBLLESRC) SRCMBR(CLMMNT)
CRTCBLMOD MODULE(LIFE400/SVCMNT)    SRCFILE(LIFE400/QCBLLESRC) SRCMBR(SVCMNT)
CRTCBLMOD MODULE(LIFE400/POLMSTINQ) SRCFILE(LIFE400/QCBLLESRC) SRCMBR(POLMSTINQ)

CRTPGM PGM(LIFE400/NBUWB)     MODULE(LIFE400/NBUWB)
CRTPGM PGM(LIFE400/CLMADJB)   MODULE(LIFE400/CLMADJB)
CRTPGM PGM(LIFE400/SVCBILB)   MODULE(LIFE400/SVCBILB)
CRTPGM PGM(LIFE400/MAINMENU)  MODULE(LIFE400/MAINMENU)
CRTPGM PGM(LIFE400/NBUWMNT)   MODULE(LIFE400/NBUWMNT)
CRTPGM PGM(LIFE400/CLMMNT)    MODULE(LIFE400/CLMMNT)
CRTPGM PGM(LIFE400/SVCMNT)    MODULE(LIFE400/SVCMNT)
CRTPGM PGM(LIFE400/POLMSTINQ) MODULE(LIFE400/POLMSTINQ)
```

### Step 6 — Compile ILE CL Programs

```text
CRTCLMOD MODULE(LIFE400/STRTLIFE) SRCFILE(LIFE400/QCLSRC) SRCMBR(STRTLIFE)
CRTCLMOD MODULE(LIFE400/RUNNBUW)  SRCFILE(LIFE400/QCLSRC) SRCMBR(RUNNBUW)
CRTCLMOD MODULE(LIFE400/RUNCLM)   SRCFILE(LIFE400/QCLSRC) SRCMBR(RUNCLM)
CRTCLMOD MODULE(LIFE400/RUNSVC)   SRCFILE(LIFE400/QCLSRC) SRCMBR(RUNSVC)
CRTCLMOD MODULE(LIFE400/DLYUPD)   SRCFILE(LIFE400/QCLSRC) SRCMBR(DLYUPD)

CRTPGM PGM(LIFE400/STRTLIFE) MODULE(LIFE400/STRTLIFE)
CRTPGM PGM(LIFE400/RUNNBUW)  MODULE(LIFE400/RUNNBUW)
CRTPGM PGM(LIFE400/RUNCLM)   MODULE(LIFE400/RUNCLM)
CRTPGM PGM(LIFE400/RUNSVC)   MODULE(LIFE400/RUNSVC)
CRTPGM PGM(LIFE400/DLYUPD)   MODULE(LIFE400/DLYUPD)
```

### Step 7 — Create Supporting Objects

```text
/* JOB QUEUE AND JOB DESCRIPTION */
CRTJOBD JOBD(LIFE400/LIFEJD) JOBQ(LIFE400/LIFEQ) TEXT('LIFE400 JOB DESC')
CRTJOBQ JOBQ(LIFE400/LIFEQ) TEXT('LIFE400 JOB QUEUE')

/* OUTPUT QUEUE */
CRTOUTQ OUTQ(LIFE400/LIFEOUTQ) TEXT('LIFE400 OUTPUT QUEUE')

/* MESSAGE QUEUE */
CRTMSGQ MSGQ(LIFE400/LIFEMSGQ) TEXT('LIFE400 MESSAGE QUEUE')

/* SCHEDULE NIGHTLY JOB */
ADDJOBSCDE JOB(DLYUPD) CMD(CALL LIFE400/DLYUPD) +
    FRQ(*WEEKLY) SCDDAY(*ALL) SCDTIME(233000) +
    JOBD(LIFE400/LIFEJD) TEXT('LIFE400 NIGHTLY UPDATE')
```

### Step 8 — Start the System

```text
CALL LIFE400/STRTLIFE
```

---

## 5250 Screen Reference

| Screen | Program | Description |
|--------|---------|-------------|
| MNUDSPF/MAINSCR | MAINMENU | Main menu — route to subsystems |
| NBUWDSPF/NBHDR | NBUWMNT | New business: policy/plan header |
| NBUWDSPF/NBINSKD | NBUWMNT | New business: insured details |
| NBUWDSPF/NBBENEFIT | NBUWMNT | New business: benefit/sum assured |
| NBUWDSPF/NBRIDERS | NBUWMNT | New business: rider entry |
| NBUWDSPF/NBRESULT | NBUWMNT | New business: issue result |
| CLMDSPF/CLMHDR | CLMMNT | Claims: claim ID + policy lookup |
| CLMDSPF/CLMDETAIL | CLMMNT | Claims: cause, date, beneficiary |
| CLMDSPF/CLMDOCS | CLMMNT | Claims: document checklist |
| CLMDSPF/CLMRESULT | CLMMNT | Claims: adjudication result |
| SVCDSPF/SVCHDR | SVCMNT / POLMSTINQ | Servicing: policy lookup |
| SVCDSPF/SVCPOL | SVCMNT / POLMSTINQ | Servicing: current policy display |
| SVCDSPF/SVCAMEND | SVCMNT | Servicing: amendment input |
| SVCDSPF/SVCRESULT | SVCMNT | Servicing: amendment result |

**Common Function Keys:**
`F3=Exit` · `F5=Refresh` · `F6=Submit/Issue/Apply` · `F10=Reinstate` · `F12=Cancel`

---

## Business Rules Summary

**New Business (50+ rules):** Issue age limits by plan, sum assured limits, maturity age cap, T65 occupation restriction, severe occupation auto-decline, UW class determination (Preferred/Standard/Table-B/Decline), mortality rate by age band, gender/smoker/occupation/UW rating factors, rider validation (ADB age cap, WOP age range, CI SA cap), modal premium loading (A/S/Q/M), reinsurance referral >45B SA, manual UW referral triggers.

**Servicing:** Grace period transition (Active→Grace after 30 days overdue), lapse transition (Active/Grace→Lapsed after grace expires), reinstatement within 730-day window, plan change with age/maturity validation, SA increase >25% or >25B requires UW, billing mode change with modal recalculation, add/remove ADB01 rider with age check.

**Claims:** Death-only claim type support, active/grace eligibility check, contestability period investigation (2 years), suicide window exclusion (2 years), accidental death ADB rider payout, grace period deduction, loan balance deduction, settlement floor at zero, payment via check or ACH.

---

## Y2K Notes

All date fields use 8-digit `YYYYMMDD` format. Programs reviewed November 1998.
See `*Y2K-REVIEWED 1998-11-14` comments throughout source members.

---

*LIFE400 — ACME Life Insurance Co. — AS/400 Term Life System*
*ILE COBOL V3R7 · OS/400 V4R2 · IBM AS/400 Model 9406*
