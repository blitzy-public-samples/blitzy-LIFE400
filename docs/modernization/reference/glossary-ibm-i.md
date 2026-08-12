# IBM i and ILE COBOL Glossary for LIFE400

LIFE400 is written in three platform-specific languages — ILE COBOL, fixed-format DDS, and ILE CL — on a platform whose vocabulary shares almost nothing with the mainstream languages the business wants to hire for [README.md:L4]. This glossary is the vocabulary bridge for that reader. It exists because the second business driver of this modernization assessment is to acquire engineers fluent in a modern mainstream language, and every other document in the set is written in terms such an engineer has never met: physical and logical files instead of tables and indexes, record formats instead of templates, copybooks instead of shared models, level-88 condition names instead of enums, indicators instead of view-model flags, and a library list instead of a classpath.

**Scope.** This glossary decodes the terms *this estate actually uses*. It is deliberately not an IBM i tutorial: a term that plays no part in LIFE400 is not defined here, and one term is defined precisely because it is *absent* — adopted authority — so that its absence reads as a finding rather than as a gap in this document. Every other document in the assessment links a term here on first use within that document. This document is the single exception to that convention, because it cannot link to itself.

**How to use it.** Entries are alphabetical and self-contained, so nothing needs to be read in order. If you are entering the assessment for the first time rather than looking up one word, start from the section index at [the modernization README](../README.md), which publishes a reading path for each audience.

## How to read an entry

- **What it is** is written for an engineer who has never used the platform. Nothing is assumed beyond general programming literacy.
- **Modern equivalent** names the construct a Java, C#, TypeScript, or Python engineer would reach for, and says plainly where no honest equivalent exists. A forced analogy is worse than an unfamiliar word: in a migration it becomes a wrong assumption in a design.
- **In LIFE400** cites the member and line where the term is instantiated, so the definition is anchored in this system rather than in general platform lore. Citations are plain text in the form `[<path>:<locator>]` and are deliberately not hyperlinks: a plain-text citation stays meaningful in a diff and does not depend on a hosting provider's line-anchor syntax. Open the member to read the construct in place — no source member is annotated, altered, or commented by this documentation set.
- Counts and inventories are **not** restated here. Each figure has exactly one owning document, and this glossary links the owner rather than repeating a number that could drift away from it. The closing section names each owner.

Two cautions apply to every entry. First, nothing in this repository can be compiled, bound, or executed to check a behavioural statement: ILE COBOL and ILE CL require the IBM i platform, and no off-platform compiler for them exists. Accuracy here rests on citation and review, and no entry should be read as build-verified. Second, LIFE400's declared runtime baseline is `ILE COBOL V3R7 · OS/400 V4R2 · IBM AS/400 Model 9406` [README.md:L264]; where platform behaviour has changed across releases, the entries describe the constructs as this estate uses them.

## Terms at a glance

| Term | What it is | Modern equivalent | Where it appears in LIFE400 |
|------|------------|-------------------|-----------------------------|
| 5250 datastream | Fixed-grid character-terminal protocol | No clean equivalent; nearest is a server-rendered form round-trip | [QDDSSRC/MNUDSPF.dspf:L12] |
| Adopted authority | Program runs with its owner's authority, not the caller's | Service account or assumed role | Not instantiated anywhere in the estate |
| CL (Control Language) | The platform's compiled command language | Shell script, but compiled and callable with typed parameters | [QCLSRC/STRTLIFE.clle:L18] |
| Copybook | Source member textually included at compile time | Shared DTO or generated model, but pasted rather than linked | [QCPYSRC/POLDATA.cpy:L14] |
| CPF message | Platform message-identifier scheme, sent or monitored | Typed exception plus catch block; structured log event | [QCLSRC/STRTLIFE.clle:L28] |
| DDS | Fixed-format declarative language defining files and screens | No single equivalent; its three roles map to three artifacts | [QDDSSRC/POLMSTL1.lf:L1-L14] |
| Display file | Externally described terminal screen, compiled as an object | Server-rendered template or component tree, with fixed geometry | [QDDSSRC/MNUDSPF.dspf:L10] |
| ILE | The platform's compile-to-module, bind-to-program model | Compile then link; cross-language calling has no single counterpart | [QCBLLESRC/MAINMENU.cbl:L27-L28] |
| Indicator | Numbered boolean shared between program and screen | View-model flag plus event handler; nothing combines both roles | [QCBLLESRC/POLMSTINQ.cbl:L92] |
| Initial program | Program a user profile launches at sign-on | Landing route after authentication, or a login shell | [QCLSRC/STRTLIFE.clle:L14-L16] |
| Job description | Named bundle of job attributes a submitted job inherits | Job template or worker configuration profile | [README.md:L199] |
| Job queue | Queue submitted work waits on before it runs | Work queue with a worker pool | [README.md:L200] |
| Level-88 condition name | Named boolean over one literal value of the field above it | Enum, or a check constraint | [QCPYSRC/POLDATA.cpy:L23] |
| Library list | Ordered, job-scoped search path for unqualified object names | Classpath or module resolution path, resolved per job | [QCLSRC/STRTLIFE.clle:L27] |
| Logical file | Keyed access path over a physical file, holding no data | Index, or a read-only view | [QDDSSRC/POLMSTL1.lf:L13] |
| Message queue | Object that jobs send informational and diagnostic messages to | Log stream or notification channel | [QCLSRC/DLYUPD.clle:L53] |
| OCCURS and INDEXED BY | Fixed-size repeating group inside one record | Fixed-size list, or a child table once persisted | [QCPYSRC/POLDATA.cpy:L88-L89] |
| Output queue | Where spooled printed output and job logs land | Artifact store or log sink | [README.md:L203] |
| Override | Job-scoped redirection of a file reference, deleted explicitly | Scoped configuration binding or DI override | [QCLSRC/DLYUPD.clle:L60] |
| Paragraph | Named procedural unit a COBOL program is decomposed into | Method, but parameterless and reached by fall-through too | [QCBLLESRC/NBUWB.cbl:L144] |
| Physical file | The stored table, with one record format and a key | Table, but without null, foreign keys, or check constraints | [QDDSSRC/POLMST.pf:L12] |
| Printer file | Externally described report layout, compiled as an object | Report template feeding a rendered document | [QDDSSRC/POLRPT.prtf:L3] |
| Record format | Named layout inside a DDS file, written or read one at a time | Row type, partial or component, or report band | [QDDSSRC/POLMST.pf:L14] |
| Record-level I/O | One whole record at a time, by key, with no query language | Repository or ORM call, but with no set-based operation at all | [QCBLLESRC/NBUWMNT.cbl:L38-L43] |
| SBMJOB | Command that submits work for asynchronous execution | Enqueue a task for a worker, or a scheduler API call | [QCLSRC/RUNNBUW.clle:L43-L46] |
| Source physical file and source member | Database file whose records are lines of source code | A directory of source files, which is this repository's form | [README.md:L127-L130] |
| Zoned decimal | Decimal stored one digit per byte in character form | Exact decimal type, never binary floating point | [QDDSSRC/POLMST.pf:L52] |

## Entries

### 5250 datastream

- **What it is** — the character-terminal protocol between an IBM i and a display station or terminal emulator. The screen is a fixed grid: every LIFE400 display file declares a 24-row by 80-column device [QDDSSRC/MNUDSPF.dspf:L12], [QDDSSRC/NBUWDSPF.dspf:L14]. A program does not paint pixels or emit markup. It writes a named record format to the device, and the operator's whole reply comes back as one transaction.
- **Modern equivalent** — none that maps cleanly. The nearest analogy is a server-rendered form round-trip, but two differences matter: field positions are compiled into the screen definition rather than laid out by a stylesheet, and the protocol has no notion of a content type, a cookie, or a session token. A COBOL program reaches the datastream by declaring its display file as a workstation device with transaction organization [QCBLLESRC/MAINMENU.cbl:L32-L36].
- **In LIFE400** — every interactive path runs over it. The README's screen reference lists the formats a user actually sees [README.md:L222], and the repository structure identifies the menu display file as a 5250 device [README.md:L35]. The datastream carries no transport security. That exposure is not assessed here: it is registered with severity and evidence in [the security risk register](../risk/01-security-risk-register.md), and the control that closes it is designed in [the target security control design](../target-state/04-security-control-design.md).

### Adopted authority

- **What it is** — a platform mechanism by which a program runs with the authority of the *program's owner* rather than the authority of the user who called it. A user with no direct access to a file can then reach that file through a trusted program and only through it. It is switched on by creating or changing the program with its user-profile attribute set to the owner.
- **Not instantiated in LIFE400.** This entry exists because adopted authority is the control a reader already familiar with IBM i would expect to find in an application of this shape, and because its absence should read as a finding rather than as an omission in this glossary. An exhaustive search of the estate finds no program created or changed with the owner user-profile attribute, no authority parameter on any object-creation command in the build procedure, and no authority-granting or authority-revoking command anywhere. The application library itself is created with no authority specification at all [README.md:L125], and the supporting work-management objects likewise [README.md:L199-L206]. Access control is therefore left entirely to the platform's own defaults.
- **Do not confuse it with initial-program configuration.** The only occurrence of a user-profile keyword anywhere in the tree is a comment in the entry program's header showing how to wire that program in as a user's initial program [QCLSRC/STRTLIFE.clle:L15]. That is sign-on configuration — *what runs* — and says nothing about *whose authority it runs under*. The two are separate attributes of the same command with unrelated effects; reading the comment as evidence of authority adoption would misstate this system's security posture. See the entry for initial program.
- **Modern equivalent** — a service account, or an assumed role that a request executes under, distinct from the caller's identity.
- **Consequence** — not assigned here. The unhardened object-authority finding, with its severity and impact, is owned by [the security risk register](../risk/01-security-risk-register.md), and the control that closes it by [the target security control design](../target-state/04-security-control-design.md).

### CL (Control Language)

- **What it is** — the platform's command language, and the language the operational layer of a platform application is written in. Every command is a named verb with keyword parameters, and a CL *program* is a compiled sequence of them with real variables, conditionals, labels, and error handling. It is the layer that starts sessions, sets up the environment, submits batch work, and decides which platform failures are tolerable.
- **In LIFE400** — CL has its own source area, described as such when it is created [README.md:L130], and each member is bracketed by a program declaration and its terminator [QCLSRC/STRTLIFE.clle:L18], [QCLSRC/STRTLIFE.clle:L43]. Parameters are declared on the program statement and typed in the body [QCLSRC/RUNSVC.clle:L21]. Five CL members carry the entire operational contract of this system: one session entry point, three batch submitters, and one nightly driver. Their runtime roles, and the work-management objects they bind, are documented in [the operational model](../current-state/06-operational-model.md).
- **Modern equivalent** — a shell script or a small orchestration program, with three differences that matter. A CL program is compiled to a program object like any other and is called by name; its parameters are typed rather than positional strings; and error handling is a language feature rather than a convention, which is why the CPF message entry below is inseparable from this one.

### Copybook

- **What it is** — a source member that the compiler splices textually into a program before compiling it. A copybook is a shared *definition*, not a linked library: each consumer ends up with its own compiled copy of the layout, and nothing at run time knows the copybook existed.
- **In LIFE400** — the estate has exactly one. It defines the shared policy-master record as a single top-level group [QCPYSRC/POLDATA.cpy:L14] under a banner that names the members expected to use it [QCPYSRC/POLDATA.cpy:L1-L13], lives in its own source area [README.md:L127], and is pulled into a program with a one-line directive in the file section [QCBLLESRC/NBUWMNT.cbl:L50]. Seven of the eight COBOL members include it — [QCBLLESRC/CLMADJB.cbl:L58], [QCBLLESRC/CLMMNT.cbl:L55], [QCBLLESRC/NBUWB.cbl:L60], [QCBLLESRC/NBUWMNT.cbl:L50], [QCBLLESRC/POLMSTINQ.cbl:L50], [QCBLLESRC/SVCBILB.cbl:L64], [QCBLLESRC/SVCMNT.cbl:L56]. The menu program is the exception: its entire file-control section declares one display file and nothing else [QCBLLESRC/MAINMENU.cbl:L32-L36], and it includes no copybook.
- **Modern equivalent** — a shared data-transfer object or a generated model class, with one consequence a modern engineer needs to hear early: because inclusion is textual, **changing the copybook requires recompiling every program that includes it**. There is no version negotiation, no binary compatibility to fall back on, and no way for two consumers to disagree about the layout and still run. The coupling this creates is analysed in [the current-state architecture](../current-state/02-architecture-current-state.md).

### CPF message

- **What it is** — the platform's message-identifier scheme. An identifier names one specific condition, and the same scheme serves two different purposes that are easy to conflate:
  - **Monitored** — a program names an identifier it is prepared to absorb, immediately after the command that might raise it. An unmonitored escape condition ends the program.
  - **Sent** — a program emits a message under an identifier of its own choosing, to a queue, for a person or another job to read.
- **In LIFE400** — both are used. `CPF9898` appears only ever as a *sent* identifier: it is the general-purpose identifier that carries the program's own text to a queue [QCLSRC/STRTLIFE.clle:L32], [QCLSRC/DLYUPD.clle:L47], and it is never monitored anywhere. The monitored identifiers, each with its meaning in the context this system uses it:

| Identifier | Condition absorbed | Where monitored |
|------------|--------------------|-----------------|
| CPF2103 | The library is already on the library list, so adding it again is not an error | [QCLSRC/STRTLIFE.clle:L28], [QCLSRC/DLYUPD.clle:L57] |
| CPF2105 | The library is not on the library list when the program tries to remove it | [QCLSRC/STRTLIFE.clle:L41], [QCLSRC/DLYUPD.clle:L103] |
| CPF9841 | An override is not there to delete when the program cleans up | [QCLSRC/RUNNBUW.clle:L55], [QCLSRC/RUNSVC.clle:L62], [QCLSRC/RUNCLM.clle:L62], [QCLSRC/DLYUPD.clle:L100] |
| CPF0000 | Catch-all for any condition in the range | [QCLSRC/STRTLIFE.clle:L34], [QCLSRC/DLYUPD.clle:L76] |

- **Modern equivalent** — a typed exception plus a catch block for the monitored case, and a structured log event for the sent case. The catch-all is the platform's equivalent of catching the base exception type, and it behaves the same way: the nightly driver wraps its single unit of work in one, increments an error counter, and sends a diagnostic message telling the reader to consult the job log [QCLSRC/DLYUPD.clle:L76], [QCLSRC/DLYUPD.clle:L80] — so a failure is counted but never identified. What is done about that is not decided here; it is owned by [the known defects and stubs register](../current-state/07-known-defects-and-stubs.md).

### DDS (Data Description Specifications)

- **What it is** — the platform's fixed-format declarative language for describing files. One language covers database tables, terminal screens, and printed reports: the file's fields with their lengths and types, its key, and for screens and reports the exact row and column of every element. It has no control flow. It is not a programming language, and it is not a data-definition dialect of SQL either — persistence in this estate is defined entirely by DDS.
- **Fixed-format means column positions are syntax.** A line reserves the first five positions for a sequence number, carries an `A` in column 6 marking it as a specification line, and then uses fixed zones for the name type, the name, the length and type, the screen or page position, and the keywords. Shifting a name two columns changes what the compiler reads. The smallest member in the estate shows the shape end to end in fourteen lines [QDDSSRC/POLMSTL1.lf:L1-L14]; its two non-comment lines are:

```text
     A          R POLMSTREC                    PFILE(POLMST)
     A          K POLID
```

- **In LIFE400** — DDS defines every persisted table, every screen, and every report, and has its own source area described as DDS when it is created [README.md:L128]. It also defines what is *not* there: an exhaustive search of the DDS members finds no uniqueness keyword on any key, and no value-list, range, or check keyword on any field — so the coded domains the COBOL side names are unconstrained in storage. The persisted contract-status column, for instance, is a bare two-character alphanumeric field [QDDSSRC/POLMST.pf:L25].
- **Modern equivalent** — none as a single language. Its three roles map to three different modern artifacts, covered under physical file, display file, and printer file below.

### Display file

- **What it is** — a DDS-described terminal screen, compiled into an object of its own and then opened by a program as if it were a file [QDDSSRC/MNUDSPF.dspf:L10]. The layout is external to the code: literals, input and output fields, highlighting, colour, and which command keys are enabled are all declared in DDS. The program writes a named record format to the screen and reads the operator's reply back.
- Because the layout is external, moving a field or changing a caption is a DDS edit and a recompile of the display file, not a change to any program. Programs only ever address the screen through format names and field names, which is why several programs can share one screen definition.
- **In LIFE400** — four display files, created from DDS source in the build [README.md:L149-L150]. One of them serves two different programs, which is possible only because of the indicator mechanism described below. The new-business file declares its device and the command keys it enables before any format appears [QDDSSRC/NBUWDSPF.dspf:L14], [QDDSSRC/NBUWDSPF.dspf:L16-L20], then defines its formats in sequence [QDDSSRC/NBUWDSPF.dspf:L24], [QDDSSRC/NBUWDSPF.dspf:L50]. The claims file follows the same pattern [QDDSSRC/CLMDSPF.dspf:L21].
- **Modern equivalent** — a server-rendered template or a component tree, with the important difference that the geometry is fixed at 24 rows by 80 columns [QDDSSRC/MNUDSPF.dspf:L12]: there is no responsive layout, no client-side state, and no styling layer to separate. The mapping of each format to a target screen, and the disposition of every format, are owned by [the UI modernization document](../target-state/05-ui-modernization.md).

### ILE (Integrated Language Environment)

- **What it is** — the platform's compile-and-bind model. Source compiles to a *module*; one or more modules bind into a *program* object; and programs written in different languages can call one another because they share the same call and data conventions. The `ILE` prefix on a language name means the compiler targets this model rather than the older single-step one.
- **In LIFE400** — the estate is ILE COBOL, DDS, and ILE CL together [README.md:L4], with COBOL and CL each given their own source area at creation time [README.md:L129], [README.md:L130]. Every COBOL program names the platform explicitly in its configuration section [QCBLLESRC/MAINMENU.cbl:L27-L28] — the declaration that ties this source to that hardware and the reason none of it builds anywhere else. The build follows the model literally: each member compiles to a module first, then binds into a program of the same name [README.md:L160-L176]. The `.clle` extension on the CL members is itself the ILE CL marker.
- **Modern equivalent** — compilation to an intermediate artifact followed by linking or packaging: a class file plus an archive, or an object file plus a linker. The cross-language calling ILE provides has no single modern counterpart; a modern system would use an in-process interface or a network call, and would pay a serialization cost this system does not.
- The declared runtime baseline for this compile-and-bind chain is recorded in the README footer [README.md:L264]. Whether that baseline is still supported is assessed in [the platform and support status document](../current-state/03-platform-and-support-status.md).

### Indicator

- **What it is** — a numbered boolean shared between a program and a display file. There is a fixed set of them, addressed by number rather than by name, and they carry information in both directions at once. They are not declared anywhere: they simply exist, and both sides refer to the same number. This is one of the least intuitive constructs on the platform, and it is worth reading the two directions separately.
- **Conditioning direction — the program tells the screen what to show.** A DDS line whose condition columns name an indicator is displayed only when that indicator is on. The servicing screen's inquiry-mode heading is conditioned on indicator 90 [QDDSSRC/SVCDSPF.dspf:L26], and the inquiry program turns 90 on immediately before writing the screen, precisely so the amendment fields are suppressed and the screen behaves as read-only [QCBLLESRC/POLMSTINQ.cbl:L91], [QCBLLESRC/POLMSTINQ.cbl:L92]. The program's own header records that intent for the next reader [QCBLLESRC/POLMSTINQ.cbl:L14]. This is the mechanism by which two programs share a single screen definition.
- **Response direction — the screen tells the program what the user did.** Each command-key keyword in a display file names the indicator that key will set [QDDSSRC/NBUWDSPF.dspf:L16-L20]. After reading the screen, the program tests the indicator to discover what the operator asked for [QCBLLESRC/MAINMENU.cbl:L65].
- **Modern equivalent** — a view-model flag for the conditioning direction, and an event handler or a named submit action for the response direction. Nothing in a modern stack fuses the two into one globally numbered boolean, and that fusion is exactly what makes indicators hard to work with: the number is the only documentation, the coupling is invisible from either side in isolation, and no compiler checks that the two sides agree about what 90 means.

### Initial program

- **What it is** — the program a user profile launches automatically at sign-on. The user does not choose it, and where no command line is available cannot step outside it: for that user, the program *is* the session.
- **In LIFE400** — the entry program is designed to be wired this way, and its own header carries the command that does the wiring [QCLSRC/STRTLIFE.clle:L14-L16]. Once running, it places the application library at the front of the job's library list and calls the menu program [QCLSRC/STRTLIFE.clle:L27], [QCLSRC/STRTLIFE.clle:L37], so the whole interactive system is reached through this one object.
- **Keep it distinct from adopted authority.** Configuring an initial program determines *what runs* at sign-on. It says nothing whatsoever about *whose authority* the program runs under. Both are attributes of the same user-profile command, which is why the two are so often conflated, but they have unrelated effects.
- **Modern equivalent** — a landing route entered after authentication, or a program configured as a user's login shell.

### Job description

- **What it is** — a named object bundling the attributes a job runs with: the job queue it goes to, its library list, its output queue, its priority, its message handling. Work is submitted by naming the job description instead of restating every attribute at every call site.
- **In LIFE400** — one job description, created for the application and pointing at the application job queue [README.md:L199], and named by each batch submitter at the moment of submission [QCLSRC/RUNNBUW.clle:L43]. Because it is named rather than described inline, changing where batch work runs is a change to one object rather than to three programs.
- **Modern equivalent** — a job template, a worker configuration profile, or a container job specification: a reusable bundle of execution settings referenced by name.

### Job queue

- **What it is** — the queue that submitted work waits on. Submitting a job does not run it. It places the job on a queue, and a subsystem takes jobs from that queue and runs them according to how the queue and the subsystem are configured.
- **In LIFE400** — one job queue serves the whole application [README.md:L200], and all three batch submitters name it [QCLSRC/RUNNBUW.clle:L43], [QCLSRC/RUNSVC.clle:L50], [QCLSRC/RUNCLM.clle:L50].
- **Modern equivalent** — a work queue or task queue drained by a worker pool.
- How much concurrency this particular queue actually permits, and what that implies for a migration, is not asserted here: it is owned by [the operational model](../current-state/06-operational-model.md).

### Level-88 condition name

- **What it is** — a COBOL declaration that gives a name to one specific literal value of the field immediately above it. It is *not* a field and occupies no storage: it is a named boolean that reads true when the parent field holds the declared value. Testing the name is exactly equivalent to comparing the field with the literal, but it reads as English and it collects a coded field's whole domain in one place:

```text
           10  PM-CONTRACT-STATUS       PIC X(02).
               88  PM-STATUS-PENDING    VALUE 'PE'.
```

- **In LIFE400** — the shared copybook uses them throughout to state the legal values of coded fields. The canonical single example is the first contract-status name [QCPYSRC/POLDATA.cpy:L23], and the whole set of contract statuses forms a contiguous band under one two-character field [QCPYSRC/POLDATA.cpy:L23-L30]. Further bands define the issue channel [QCPYSRC/POLDATA.cpy:L32-L34], the six amendment types [QCPYSRC/POLDATA.cpy:L119-L124], the three claim decisions [QCPYSRC/POLDATA.cpy:L166-L168], and the rider status [QCPYSRC/POLDATA.cpy:L95-L96].
- **Modern equivalent** — an enum, or a check constraint on a column. Two cautions carry into any conversion. First, a condition name constrains nothing: the parent field will hold whatever the program moves into it, and the domain is enforced only where a program chooses to test a name. Second, the DDS file that persists the field declares no equivalent constraint — the contract-status column is a bare two-character alphanumeric [QDDSSRC/POLMST.pf:L25] — so the target must decide whether to enforce the domain that the legacy system only documented.
- The inventory of these domains, and the target constraint each becomes, are owned by [the current data model](../current-state/04-data-model-current-state.md) and [the target schema mapping](../target-state/03-target-data-model-and-schema-mapping.md).

### Library list

- **What it is** — the ordered list of libraries a job searches when it resolves an unqualified object name. It is per-job, not per-system: two jobs on one machine can resolve the same name to different objects at the same moment. Manipulating it is imperative — one command adds a library at a position, another removes it — so name resolution is a side effect of statements that have already run.
- **In LIFE400** — the entry program reads the job's current library into a variable [QCLSRC/STRTLIFE.clle:L24], places the application library at the front of the list [QCLSRC/STRTLIFE.clle:L27], and removes it when the session ends [QCLSRC/STRTLIFE.clle:L40]; the nightly driver does the same for its own job [QCLSRC/DLYUPD.clle:L56]. Where the application library must be named explicitly it is a literal in the CL source rather than a parameter [QCLSRC/STRTLIFE.clle:L37].
- **Modern equivalent** — a classpath or a module resolution path, with two differences: it is established per job at run time rather than fixed at build time or process start, and extending it is a statement inside the program rather than a launch flag outside it.

### Logical file

- **What it is** — a keyed access path over one or more physical files. It stores no data of its own. It is a definition that names the physical file to read, optionally selects or reorders the fields to expose, and declares the key order records are presented in; opening it reads the physical file's records through that path.
- **In LIFE400** — exactly one, declared as a logical file in its own banner [QDDSSRC/POLMSTL1.lf:L3], naming its physical file [QDDSSRC/POLMSTL1.lf:L13] and its key [QDDSSRC/POLMSTL1.lf:L14], and built from that source in the build procedure [README.md:L143]. Two facts about it are worth stating plainly: it declares no fields of its own, and its key is the same single field the physical file already keys on [QDDSSRC/POLMST.pf:L81]. It therefore offers no access path the physical file does not already offer.
- **Modern equivalent** — an index, or a read-only view. Whether this access path needs any counterpart at all in the target is settled in [the target schema mapping](../target-state/03-target-data-model-and-schema-mapping.md).

### Message queue

- **What it is** — an object that programs and jobs send messages to: informational, completion, and diagnostic. It is a queue of records rather than a text file. Each message carries an identifier, its message data, and a type, and is read by a person or by another program.
- **In LIFE400** — one application message queue, created in the build [README.md:L206] and written by the entry program when a session starts [QCLSRC/STRTLIFE.clle:L33], by the nightly driver when its run begins [QCLSRC/DLYUPD.clle:L53], and diagnostically when the driver's unit of work fails [QCLSRC/DLYUPD.clle:L80]. The system operator's own queue receives the same start and completion messages [QCLSRC/DLYUPD.clle:L49], [QCLSRC/DLYUPD.clle:L92], so two audiences are served by two explicit sends rather than by one routed event. A single submission binds the job description, the job queue, the output queue, and the message queue together in four lines [QCLSRC/RUNNBUW.clle:L43-L46].
- **Modern equivalent** — a log stream or a notification channel. The message type is the closest thing this system has to a log level, and the queue is the closest thing it has to a destination that can be configured rather than compiled in.

### OCCURS and INDEXED BY

- **What it is** — a COBOL declaration that repeats a group of fields a fixed number of times inside a single record, with a named index used to address one occurrence. The size is compiled in: there is no growth, no resizing, and no empty state distinguishable from a zero-filled one.
- **In LIFE400** — the shared copybook declares a rider table repeating five times with its own index [QCPYSRC/POLDATA.cpy:L88-L89], each occurrence holding a code, a sum assured, a rate, a premium, and a status [QCPYSRC/POLDATA.cpy:L90-L94]:

```text
               10  PM-RIDER-TABLE OCCURS 5 TIMES
                              INDEXED BY PM-RIDER-IDX.
```

- **Modern equivalent** — a fixed-size list for a purely in-memory structure, or a child table with a foreign key to its parent for anything persisted. Which of the two applies to riders in this system is decided in [MOD-ADR-009](../decisions/MOD-ADR-009-rider-persistence.md); the underlying data-model facts are in [the current data model](../current-state/04-data-model-current-state.md).

### Output queue

- **What it is** — where spooled output lands. Printed output and job logs are not sent straight to a device; they become spooled files on a queue, from which they can be viewed, held, released, printed, or discarded.
- **In LIFE400** — one application output queue, created in the build [README.md:L203], and it *is* referenced: all three batch submitters name it on submission, so each submitted job's spooled output is directed to it [QCLSRC/RUNNBUW.clle:L44], [QCLSRC/RUNSVC.clle:L51], [QCLSRC/RUNCLM.clle:L51].
- **Modern equivalent** — an artifact store for generated documents, or a log sink for job output. The distinction the platform draws — output exists as a retained object before anything prints it — has no default counterpart in a modern stack and has to be built deliberately if it is wanted.

### Override

- **What it is** — a command that redirects a file reference for the duration of a job, without changing the program. A program compiled against an unqualified file name can be pointed at a specific library's copy of that file at run time. The redirection has a declared scope, and within that scope it must be *explicitly deleted* — it does not expire on its own.
- **In LIFE400** — the nightly driver redirects both files it needs to the application library at job scope [QCLSRC/DLYUPD.clle:L60], [QCLSRC/DLYUPD.clle:L61], then deletes both at the end [QCLSRC/DLYUPD.clle:L98-L99] while tolerating the case where an override is not there to delete [QCLSRC/DLYUPD.clle:L100]. Each batch submitter wraps its submission in the same set-up and tear-down pair [QCLSRC/RUNSVC.clle:L44-L45], [QCLSRC/RUNSVC.clle:L60-L61]. The set-up statement reads:

```text
OVRDBF FILE(POLMST) TOFILE(LIFE400/POLMST) OVRSCOPE(*JOB)
```

- **Modern equivalent** — a scoped configuration binding, or a dependency-injection override registered for the lifetime of a request or a worker.
- The point for a modern reader: **this is configuration expressed as an imperative command.** It is established by executing a statement and withdrawn by executing another. There is no declarative file to inspect, nothing fails at build time if a set-up and tear-down pair do not match, and the effective binding of a file can only be known by tracing which statements have already run in that job.

### Paragraph

- **What it is** — the unit a COBOL procedure division is decomposed into: a name, followed by statements, invoked by a `PERFORM`. LIFE400 names them on a numbered verb-noun pattern so that the name states both the step's place in the flow and what it does, for example [QCBLLESRC/NBUWB.cbl:L144].
- **Modern equivalent** — a method, with two differences that matter when converting. A paragraph takes no parameters and returns no value, so everything it reads and writes is shared state in the record layout or in working storage; and control falls through from one paragraph into the next when it is not reached by a `PERFORM`, so the physical order of the source is part of the behaviour and cannot be rearranged freely.
- **In LIFE400** — paragraph names are the natural seam for a target operation, which is why the per-program paragraph inventory and the mapping from paragraph to target operation are owned by [the current-state architecture](../current-state/02-architecture-current-state.md) and [the program-to-service map](../target-state/02-program-to-service-map.md) rather than restated here.

### Physical file

- **What it is** — the stored table. A DDS physical file declares exactly one record format, its fields with their lengths and types, and its key. The object holds the data.
- **In LIFE400** — three physical files, each carrying the command that creates it in its own banner: the policy master [QDDSSRC/POLMST.pf:L12], the service amendments file [QDDSSRC/SVCPF.pf:L12], and the claims file [QDDSSRC/CLMPF.pf:L12]. All three are built from DDS source in the build procedure [README.md:L140-L142], and each declares its key on a single field: [QDDSSRC/POLMST.pf:L81], [QDDSSRC/SVCPF.pf:L54], [QDDSSRC/CLMPF.pf:L67].
- **Modern equivalent** — a table, with three differences a modern engineer must not assume away. There is no null: no member in this estate declares null support, so an unset character field holds blanks and an unset numeric holds zero, and "absent" is indistinguishable from "blank" or "zero". There is no foreign key and no check constraint available in DDS. And a key declaration is an access path, not a uniqueness guarantee unless it is declared unique — no key in this estate is.
- The column-by-column position of all three files is owned by [the current data model](../current-state/04-data-model-current-state.md), and their target counterparts by [the target schema mapping](../target-state/03-target-data-model-and-schema-mapping.md).

### Printer file

- **What it is** — a DDS-described report layout, compiled into its own object and written to by a program exactly as a display file is. It declares record formats, the position of every element on the page, page geometry, and vertical spacing.
- **In LIFE400** — two printer files, each declaring itself one in its banner [QDDSSRC/POLRPT.prtf:L3], [QDDSSRC/CLMRPT.prtf:L3], each carrying the command that builds it [QDDSSRC/POLRPT.prtf:L10], [QDDSSRC/CLMRPT.prtf:L10], and both created in the build [README.md:L153-L154]. A report format can carry a spacing directive alongside its name, which is how a band controls the paper as well as the content [QDDSSRC/POLRPT.prtf:L15].
- **Modern equivalent** — a report template feeding a rendered document. What becomes of these two members in the target, and the question of which programs drive them, are owned by [the UI modernization document](../target-state/05-ui-modernization.md) and [the known defects and stubs register](../current-state/07-known-defects-and-stubs.md).

### Record format

- **What it is** — a named layout inside a DDS file, introduced by `R` in the name-type position. One file holds one or more formats, and a program reads or writes **one format at a time** by naming it in the I/O statement. For a database file the format is the record layout; for a screen it is a region of the display; for a report it is a band on the page.
- **In LIFE400** — a database file's single format is the whole record [QDDSSRC/POLMST.pf:L14], [QDDSSRC/SVCPF.pf:L14], [QDDSSRC/CLMPF.pf:L14]. A screen file holds several: the menu file defines a main screen and, separately, the message line at the bottom of it [QDDSSRC/MNUDSPF.dspf:L19], [QDDSSRC/MNUDSPF.dspf:L50]. A report file holds one format per band [QDDSSRC/POLRPT.prtf:L15]. The program names the format in the statement itself, writing one and reading one back [QCBLLESRC/MAINMENU.cbl:L63-L64].
- **Modern equivalent** — for a database file, the row type; for a screen, a partial or a component; for a report, a band in a report template. The consequence for the target is that what a user experiences as one legacy screen is several formats written in sequence, not one page — so a format-to-screen mapping is not one-to-one. Format inventories and their dispositions are owned by [the UI modernization document](../target-state/05-ui-modernization.md).

### Record-level I/O

- **What it is** — reading and writing one whole record at a time, by key, with no query language. A program declares the file, its organization, its access mode, the field that serves as the key, and a status field. It then moves a key value into the record and issues a read, or fills the record and issues a write or a rewrite. There is no `SELECT`, no `WHERE`, no join, no aggregate, and no set-based update.
- **In LIFE400** — the whole idiom is declared in six lines [QCBLLESRC/NBUWMNT.cbl:L38-L43]: the file is assigned to a database device, organized as indexed, accessed at random, keyed on a named field from the shared record, and given a two-character status field. Every I/O statement sets that status field, and the program either tests it or takes an invalid-key branch to learn what happened [QCBLLESRC/POLMSTINQ.cbl:L102]. In practice the three operations look like a keyed read [QCBLLESRC/POLMSTINQ.cbl:L102], a rewrite of the record just read [QCBLLESRC/SVCMNT.cbl:L190], and a write of a new record to a second file [QCBLLESRC/SVCMNT.cbl:L191].
- **Modern equivalent** — a repository method or an ORM call: `findById`, `save`, `update`. The difference that matters most is what is *missing*. An exhaustive search of the eight COBOL members finds no delete verb, no file-positioning verb, and no sequential-read verb anywhere: every single access in this system is one record fetched or stored by its key. There is no way to express "every policy whose paid-to date has passed" in this design at all.
- What follows from that — for the architecture, for the nightly work, and for data retention — is argued in [the current-state architecture](../current-state/02-architecture-current-state.md) and [the compliance and data protection document](../risk/02-compliance-and-data-protection.md), not here.

### SBMJOB

- **What it is** — the command that submits a job for asynchronous execution. It takes the command to run plus the execution context that job should have — job description, job queue, output queue, message queue — and returns immediately. The work happens later, in a different job, under a different set of attributes.
- **In LIFE400** — each of the three batch submitters ends in one submission that binds all four work-management objects and the program call together [QCLSRC/RUNNBUW.clle:L43-L46], which makes it the single most useful anchor in the estate for seeing how those objects relate:

```text
SBMJOB JOB(&JOBNAM) JOBD(LIFE400/LIFEJD) JOBQ(LIFE400/LIFEQ) +
    OUTQ(LIFE400/LIFEOUTQ) +
```

- **Modern equivalent** — enqueuing a message or a task for a worker, or a scheduler API call. One property to carry forward deliberately: the submitting program holds no handle on the result. It notifies the requester that work was submitted and returns [QCLSRC/RUNNBUW.clle:L49-L50], and nothing in the design correlates a submission with its eventual outcome.

### Source physical file and source member

- **What it is** — on IBM i, source code is not held in a filesystem directory. A **source physical file** is a database file whose records are lines of source text, and each program's source is a **member** inside it. Members are conventionally grouped by language, one source physical file per language.
- **In LIFE400** — this is why the repository's four top-level folders are named as they are: one folder per source physical file, each created with a fixed record length and a description naming the language it holds [README.md:L127-L130]. Putting this repository onto a real machine means copying each file into the matching source physical file as a member [README.md:L135] — the step that turns a directory of files back into members.
- **Modern equivalent** — a directory of source files in a version-controlled repository, which is exactly the form this estate is in today. The only residue is the naming: `QCPYSRC`, `QDDSSRC`, `QCBLLESRC`, and `QCLSRC` are source-physical-file names following the platform's own convention for them, not module or package names, and nothing in the code depends on the folder names as this repository presents them.
- The member-by-member register of what those folders contain is owned by [the system inventory](../current-state/01-system-inventory.md).

### Zoned decimal

- **What it is** — a decimal number stored one digit per byte in character form, with the sign, when the field is signed, encoded in the byte holding the last digit. In COBOL it is a numeric picture with display usage, which is the default; in DDS it is the `S` type. It is not binary and not floating point: the digits are legible in a dump, arithmetic is exact to the declared number of decimal places, and precision is a property of the declaration rather than of a machine word.
- **In LIFE400** — money is declared twice, once on each side of the same field, in two different notations that mean the same thing. DDS declares the sum assured as fifteen digits with two decimals [QDDSSRC/POLMST.pf:L52]; the copybook declares the matching premium fields as thirteen digits plus two decimals [QCPYSRC/POLDATA.cpy:L99] — the same fifteen-digit, two-decimal precision:

```text
     A            SUMASSR       15S 2         TEXT('SUM ASSURED')
               10  PM-BASE-ANNUAL-PREMIUM   PIC 9(13)V99.
```

- Unsigned is the norm here: exactly one field in the shared record is declared signed [QCPYSRC/POLDATA.cpy:L106] and every other numeric item is not, so a negative intermediate value has nowhere to live in most of the record. The scale is not uniformly two either — the flat extra rate is declared with four decimals [QDDSSRC/POLMST.pf:L49] — so no single target numeric type can be chosen once and applied everywhere. Zoned is also the only numeric representation used: an exhaustive search of the DDS members finds no packed, binary, or floating-point field type anywhere.
- **Modern equivalent** — an exact decimal type, and never binary floating point, which cannot represent these values exactly and would produce premiums differing from the legacy system's in the last place. That difference is not academic: output parity against the legacy system is the acceptance test for the migration.
- The representation decision is recorded in [MOD-ADR-006](../decisions/MOD-ADR-006-date-and-decimal-representation.md), and the field-by-field type mapping is owned by [the target schema mapping](../target-state/03-target-data-model-and-schema-mapping.md).

## Figures owned by other documents

The only quantities stated above are the small ones an entry needs in order to define its own construct, each cited at the point it is made — three physical files, four display files, two printer files, one copybook, five CL members, a 24-row by 80-column screen. Every *inventory* figure has exactly one owning document, and restating one here would create a second place for it to be wrong. Look those up at the owner instead:

- Member register, per-member version and authorship, and estate size — [the system inventory](../current-state/01-system-inventory.md).
- Paragraph inventories, call graph, and the online-to-batch duplication analysis — [the current-state architecture](../current-state/02-architecture-current-state.md).
- Column counts, elementary-item counts, and the number of level-88 domains — [the current data model](../current-state/04-data-model-current-state.md).
- Business-rule census, the inline rule-identifier bands, and how many rules carry a source anchor — [the business rule inventory](../current-state/05-business-rule-inventory.md).
- Work-management object roles, message vocabulary, queue behaviour, and the build sequence — [the operational model](../current-state/06-operational-model.md).
- Defect, stub, and inert-feature dispositions — [the known defects and stubs register](../current-state/07-known-defects-and-stubs.md).
- Security findings, their severity, and their evidence — [the security risk register](../risk/01-security-risk-register.md).
- Record-format totals and the disposition of every screen and report format — [the UI modernization document](../target-state/05-ui-modernization.md).

## Source citations

Every member cited above, read as evidence and left unmodified.

- ILE COBOL — `QCBLLESRC/MAINMENU.cbl`, `QCBLLESRC/NBUWMNT.cbl`, `QCBLLESRC/NBUWB.cbl`, `QCBLLESRC/SVCMNT.cbl`, `QCBLLESRC/SVCBILB.cbl`, `QCBLLESRC/CLMMNT.cbl`, `QCBLLESRC/CLMADJB.cbl`, `QCBLLESRC/POLMSTINQ.cbl`
- ILE CL — `QCLSRC/STRTLIFE.clle`, `QCLSRC/RUNNBUW.clle`, `QCLSRC/RUNSVC.clle`, `QCLSRC/RUNCLM.clle`, `QCLSRC/DLYUPD.clle`
- Copybook — `QCPYSRC/POLDATA.cpy`
- DDS database — `QDDSSRC/POLMST.pf`, `QDDSSRC/POLMSTL1.lf`, `QDDSSRC/SVCPF.pf`, `QDDSSRC/CLMPF.pf`
- DDS display — `QDDSSRC/MNUDSPF.dspf`, `QDDSSRC/NBUWDSPF.dspf`, `QDDSSRC/SVCDSPF.dspf`, `QDDSSRC/CLMDSPF.dspf`
- DDS printer — `QDDSSRC/POLRPT.prtf`, `QDDSSRC/CLMRPT.prtf`
- Repository overview and build procedure — `README.md`
