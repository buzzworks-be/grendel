# Principles

**What Heimdall covers** — the concerns this instrument is built to
investigate. A principle names a way work can go wrong that is worth knowing
about; it does not assert that any repository holds it, and Heimdall does not
own it. Together they are the **frame of reference**: what observations are
taken against. Point a different frame at the same history and different things
become visible.

Principles are the *what* and change rarely. The rules in `rules/` are the
*how* — each derives from a principle and defines what can be looked for and
what would innocently explain it. A rule produces **observations about
adherence**: this is what the record shows, this is what it cannot show. Those
observations are how a principle may or may not turn out to have been upheld —
a question answered by the human reading the case file, never by Heimdall.

**Heimdall does not judge, and cannot determine a violation.** These principles
are its *frame of reference* — the concerns it is built to investigate, not
beliefs it holds — and the rules are how each concern is investigated. The word
*violation* names a **test outcome**: a rule's condition was met and no listed
exemption applied. It is never a conclusion about anyone's conduct. Whether a
principle was upheld, and what to do about it, is decided by the person reading
the case file.

The measurements that feed the rules live in `SIGNALS.md`.

Heimdall is not a security scanner and not a code-quality checker. It examines
how the work was produced — over a window of time, not per change — and reports
what the record shows. Good code arrived at badly still leaves a trace worth
reporting.

The unifying concern: **software is now produced faster than anyone can absorb
it.** Every principle below is a facet of that.

> **Status: candidate.** These are stated, not settled.
>
> P-1 to P-4 each have at least one **active** derived rule, though they are not
> equally strong — see the note on rule strength in `README.md`. P-5 to P-11
> have only **draft** rules (R-008 to R-016): their thresholds were chosen or
> swept on a handful of histories rather than calibrated, so they may be raised
> in a case file as observations and never as violations. That limit is real,
> not a formality: a draft rule must never be cited as though an increment
> failed it. Every principle has at least one derived rule; seven of them have
> only a draft.

---

## P-1. Work that outruns digestion is not done

An increment is finished when a person has absorbed it, not when it is
committed. Production that consistently exceeds the rate at which anyone could
read it accumulates surface that no one understands — and the cost is paid
later, by whoever must change it.

This is the spine. P-2 and P-3 are the shapes it takes.

**Rules out:** merging at a rate no reader could sustain; treating a merge as
evidence of review; measuring a period by what it produced rather than by what
was understood.

**Derived rules:** R-001 (a merged branch must have had time to be read),
R-004 (work landing on the trunk must not outrun a reader), R-006 (sustained
production stays within reach of a reader). Separate rules because the claims
differ: a branch has a review window, a trunk commit has none, and R-006 binds
inside the flow of production — including solo work, where the director is the
reader digestion cares about.

---

## P-2. Where the repository holds the reasoning, code arrives with it

Some repositories keep their specifications, decisions and requirements
alongside the code; some keep them elsewhere. Where they are kept alongside,
implementation landing without them is a gap in the process, not a matter of
taste — the reasoning existed at the moment of writing and was discarded.

The conditional is essential. A repository that deliberately keeps its reasoning
in another system is not violating this, and a rule derived from it must
establish the convention before applying it.

**Rules out:** implementation with no corresponding change to specification or
decision records, in a repository that otherwise maintains them; a specification
written only after the code it describes.

**Derived rules:** R-003 (substantial implementation arrives with the reasoning
behind it).

---

## P-3. A specification and its implementation landing together means no decision happened between them

Writing a spec and then building against it are two acts, and the gap between
them is where a human decides whether the spec is right. When both arrive in one
increment — a large document and broad code change in a single act — that gap
did not exist. Nothing was reviewed before it was built on.

This is most visible in machine-generated work, which can produce both at a
speed that leaves no room for a decision in between. The finding is not the
volume. It is the absence of the intermediate step the volume implies was
skipped.

**Rules out:** a large specification and its implementation in one increment;
documentation grown so fast that no reader could have kept pace, which becomes
documentation nobody reads and then documentation nobody maintains.

**Derived rules:** R-002 (a specification must have had an interval in which to
be decided on), R-005 (a living document keeps its claims about the code true —
the terminal stage of the decay this principle names).

---

## P-4. The history says truthfully what produced the work

Whether a change was written by a person, generated under direction, or
produced autonomously is part of the record of how the work went — and it is
the part most easily erased, since attribution is opt-in for the tool that
would carry it. A history that silently launders machine production into
apparent hand-work defeats every question this instrument exists to ask.

Stated carefully, because the obvious rule is wrong: absence of attribution
must never be treated as evidence of human authorship (S-3). The principle
binds only where machine involvement is demonstrated by something that cannot
be switched off — and then it demands the history admit what the physics
already show.

**Rules out:** stripping or suppressing machine attribution; work produced at
demonstrably machine pace with no declaration anywhere in the record.

**Derived rules:** R-007 (machine-paced work declares itself).

---

## P-5. Care is proportional to what is at stake

Not all change carries the same risk, and a process that treats it as though it
did is not disciplined — only uniform. Where more is at stake, more of whatever
the repository uses to be careful should be visible around the change: a longer
interval, recorded reasoning, review by someone who would know.

A flat process is either over-spending everywhere or under-spending where it
counts, and from outside those look identical. Both are worth knowing, and
neither is visible to a rule that applies one threshold to every path.

Stated with one hard constraint, because the obvious implementation is wrong.
**Stakes must be read from what the repository itself declares** — `CODEOWNERS`,
protected paths, a documented critical path, a security policy, a release
process that treats some components differently. Stakes *inferred* by Heimdall
from the code would be Heimdall holding an opinion about someone else's
architecture, which is the code-quality boundary this instrument does not
cross. Where a repository declares nothing, this principle has nothing to say,
and that silence is honest rather than a gap to be filled by guessing.

This principle is partly a correction to the ones above it. P-1, P-2 and P-3 are
indifferent to what a change touches, so their thresholds treat a typo fix and a
change to the authentication path alike. Until a rule derives from P-5, that
remains a caution to carry when reading their findings, not something the tables
can express.

**Rules out:** treating every change as equivalent where the repository itself
does not; reading uniform effort as evidence of discipline; and inferring what
matters in a repository that has not said.

**Derived rules:** R-008 (care must follow the gradient a repository declares
in its own ownership file) — **draft**. Revised once, after its first statement
proved unable to form a comparison: it compared declared paths against
undeclared ones, and a declaration worth comparing against covers everything
worth owning. It now compares levels *within* the declaration, using owner count
per pattern as the ordering the repository itself stated. Signal S-7 reports the
declaration and its coverage on any repository that has one, whether or not the
rule can run.

---

## P-6. Discipline belongs to the system, not to whoever is currently in it

A way of working is only as durable as the thing carrying it. Where recorded
reasoning, review and documentation hold because tooling, gates and conventions
require them, the practice survives the people who established it. Where they
hold because particular individuals happen to work that way, the practice leaves
when they do — and no single increment shows which of the two is true.

The concern is the **consistency of the process**, and it is a property of the
system. An organisation whose discipline depends on who picked up the task is
carrying a risk that every clean increment reading will miss, and the remedy is
a process one: move the discipline into the tooling so it stops depending on who
showed up.

Stated with a constraint that is not a softening. What may be said is whether
practice holds regardless of who is working, and at most by how much it varies.
**What must never be said is who differs** — not by name, not by handle, not by
an anonymised row, and not by any count from which a reader could work it out.
Identity carries no information for this finding: the finding is that the
process is consistent or that it is not, and knowing which contributor sits
where does not change it. Carrying identity is therefore pure cost, and this
principle is not a licence to compare people. Any rule derived from it inherits
these constraints and the contributor floor recorded with the project's
roadmap.

**Rules out:** mistaking discipline that is currently visible for discipline
that will persist; and any reading that resolves consistency into a comparison
of identifiable people.

**Derived rules:** R-009 (a practice the repository relies on must be carried by
something in the repository) — **draft**. Note what it does *not* do: the direct
reading of this principle, comparing how contributors work, is what the
principle forbids, so R-009 answers the same question from configuration
instead. It computes nothing per contributor, and therefore needs none of the
constraints above to be enforced by hand.

---

## P-7. Documentation must be readable in the part a reader needs

Documentation is read by someone who needs one answer, usually in a hurry,
usually knowing roughly what they are looking for. A corpus of a few enormous
documents can hold every fact and still fail that reader completely: finding the
answer costs reading the whole thing, so nobody reads it, and the document
becomes a place where facts are stored rather than a place where they are found.

The concern is **not length**. It is **addressability** — whether a reader can
reach the part that answers their question without loading the rest. Ten files
of two hundred lines beat one of two thousand only if they divide along the
lines a reader would actually look; split arbitrarily they are worse, because
the reader must now search ten places instead of one. Structure is what makes a
long document workable and its absence is what makes a short one useless.

Stated that way for a specific reason. A rule that capped document length would
be satisfied by `split`, which costs nothing and changes nothing — the failure
`README.md` warns about, where the cheapest evasion is theatre. Any rule under
this principle must be satisfiable *only* by making the material easier to
reach: headings that name what is beneath them, an index that stays true,
sections that can be read alone.

**Some documents are written for a model, and the economics invert.** A
repository now carries instruction files — `CLAUDE.md`, `AGENTS.md`, agent and
skill definitions, rule files — that are read by a model rather than a person,
and everything above stops applying to them:

- A model is handed the file **whole**. It does not navigate to the part it
  needs, so headings save it nothing and addressability is not the measure.
- **Length is the entire cost**, counted in tokens rather than minutes.
- For the always-loaded kind, that cost is paid **on every turn**, not once per
  question, and it is spent from the same budget as the work itself. A long
  instruction file does not merely take time to read; it displaces the thing
  being reasoned about.

The two are genuinely independent, not two views of one thing. A 597-line
document with thirty headings is comfortable for a person — small spans,
entry points everywhere — and can still be the most expensive file in the
repository for a model. Both facts are true and a case file should be able to
say both.

**Anomalies are read against the repository, not against a number.** A
six-thousand-line document in a corpus whose median is a hundred and twenty is
worth a look; the same document among specifications of that size is ordinary.
As in P-5, the standard is the target's own, and the useful unit is the one the
frame already uses elsewhere — reading time at a generous rate, so that a
document's cost is stated in the same currency as R-001's review windows.

This is a different concern from the ones above it, and worth keeping separate.
P-1 asks whether work arrived faster than it could be absorbed; a four-thousand
line document written slowly over a month never troubles it and is no more
readable for that. P-2 and P-3 ask whether reasoning is present and whether a
decision had an interval. P-4 asks whether the record is truthful. R-005 asks
whether a document's claims are still true. **None of them asks whether anybody
can read the thing**, and a repository can satisfy every one of them with a
documentation set nobody opens.

**Rules out:** treating documentation as complete because the facts are present
somewhere; measuring a corpus by volume produced; capping length without regard
to structure; and judging any document against an absolute size rather than
against its own repository.

**Measurement:** `bin/reading-weight` (signal S-6) reports both distributions
and their outliers, against the repository's own median.

**Derived rules:** R-010 (a document must offer a reader a way in partway) —
**draft**, and **for human-read documents only**. It measures the longest span
between headings rather than length, and exempts spans that are mostly table or
fenced code, because an inventory is entered by lookup and every row is its own
way in. The machine-read half of the principle has **no rule**: `reading-weight`
reports tokens and load frequency as a signal, and what a reasonable context
budget is depends on the harness, the model and the work — not on anything
visible in the repository. The comment-share half of S-6
has **no rule and should not acquire one**: it is where reading cost sits, not a
measure of merit, and as a score it would be the most gameable number in the
frame.

---

## P-8. Where the repository verifies its work, code arrives with its verification

Some repositories ship an executable check alongside the code it covers; some
verify elsewhere, or by other means, or not at all. Where the practice is to
ship it alongside, implementation landing without any is a departure from the
repository's own way of working — and what landed is behaviour asserted only by
whoever wrote it.

The conditional is as essential here as it is in P-2, and for the same reason. A
repository that keeps its suite elsewhere, verifies by construction, or has
decided tests are not how it works is not failing this principle, and a rule
derived from it must establish the convention from measured practice before
applying it.

**This is not a judgement about testing, and the line matters.** Whether a test
is any good, whether coverage is adequate, whether the right things are covered
— all code-quality questions, all outside what this instrument does. What is
visible from a repository is narrower and still worth having: whether
verification arrived at all, in a repository that normally ships it. The same
distinction P-2 draws between *reasoning was recorded* and *the reasoning was
sound*.

Kept apart from P-2 rather than folded into it, because the sentence a case file
must write differs. "The reasoning did not arrive" and "the verification did not
arrive" mean different things to a reader and imply different remedies, and a
single principle covering both would produce findings whose citation could not
say which had happened. The same argument that separated R-001 from R-004.

There is a reason this concern sharpens now rather than being a long-standing
one nobody bothered to state. DORA's 2025 research reports delivery stability
falling as AI adoption rises — change failure rate and rework rate both
worsening while individual output increases, as the delivery-metrics
assessment records. Generated code
arrives faster than the verification for it does, and a repository is where that
gap becomes visible.

**Rules out:** treating a merge as evidence that anything was verified; reading
the presence of a test file as evidence of adequate testing; and expecting tests
in a repository that has not established the practice.

**Derived rules:** R-011 (substantial implementation arrives with its
verification).

---

## P-9. A process that never revisits does not correct

Every principle above counts what is *present* in a range. None can see a
range that *removed* something. A window whose base is the very change that
fixed the repository's largest measured defect reads worse than one that
merely avoided new ones, because the tables show residue and never direction
of travel. That is a reporting artifact, and underneath it is a process
property: whether landed work is ever looked at again, and whether the
repository shows the shape of correction — a revert, a fix that names what it
fixed, rework concentrated where something went wrong — or only accretion.

The concern is not that correction happens. Correction happening is the
healthy shape, and a high rate of it is at least as consistent with a working
safety net as with poor work; a team that detects and backs out in an hour
looks worse by any naive count than one that ships the same defect and never
notices. The concern is the two shapes that show nothing was learned: work
backed out and then returned unchanged, and a file replaced and replaced again
inside one window because nobody understood it the first time.

Stated with a limit. Only some correction is visible: a `git revert` announces
itself, a fix that references its subject can be followed, but fix-forward
work that names nothing is indistinguishable from any other change, so a
repository that never reverts on principle reads as one that never corrects.
Every measure under this principle bounds correction from below.

**Rules out:** reading a clean window as health when the window merely added;
reading a window full of repair as unhealthy when repair is what it was; and
crediting a process for the absence of new defects when it has never shown it
can remove one.

**Derived rules:** R-012 (reverted work must not re-land unchanged) and R-013
(a file majority-replaced repeatedly in one window did not settle) — both
**draft**. R-012 is the harder fact: patch-id equality between what was
reverted and what came back, the same diff line for line. R-013 is stated
narrowly on purpose, because its ancestor — the retired *rework commit ratio*
— counted any touch of a previously-touched file and saturated at 84–98% on
every history tried; `bin/reversal` prints both figures side by side so a
reader can see that the narrower one discriminates where the old one did not.

---

## P-10. Work begun is work owed

Every principle above examines what **landed**. Nothing sees what was
*started*. A repository opening branches faster than it closes them, or
writing itself obligations faster than it discharges them, is producing
commitments faster than it retires them — the unifying concern applied to
intentions rather than to code — and a clean case file is entirely compatible
with the pile, because nothing in the frame looks at it.

Two carriers are visible from git, and they are weak in different ways. A
branch that never landed is a piece of begun work, but only where the
repository removes refs on landing; where it keeps them, survivors are
done-and-kept mixed with abandoned and nothing separates them, and where it
squashes, no surviving branch is reachable from the trunk whether it landed or
not. A marker in the tree — `TODO`, `FIXME`, whatever the repository writes —
is a recorded obligation, but its level says nothing: a repository that writes
none is not cleaner, it is silent. Only the repository's own trend carries
anything.

What neither can see is the likeliest case: work begun, abandoned, and deleted.
Both carriers are floors.

**Rules out:** treating merged work as the whole of the work; reading
throughput as progress without asking what was left behind to achieve it;
comparing one repository's obligations against another's; and mistaking a
repository that records no obligations for one that has none.

**Derived rules:** R-014 (unlanded work must not accumulate across consecutive
windows) and R-015 (recorded obligations must be discharged, not only
accumulated) — both **draft**, both **conditional** in the way R-003 and R-011
are. R-014 establishes the repository's ref-retention convention from the
landings it can see and stands down where refs are kept; R-015 establishes
that the repository uses markers at all and stands down where it does not. On
the three histories they were built against, each stood down once for the
right reason, which is the behaviour that matters most in a conditional rule.

---

## P-11. An increment must be separable to be reviewable

R-001 and R-004 are necessary-condition tests about **time**: the interval in
which to read did not exist. This is a different claim, and orthogonal to
them: that the *unit* could not be reviewed as one decision however much time
there was. A change reaching into many unrelated areas at once is several
decisions wearing one identifier; a reader must hold all of them, or approve
the lot on faith. Machine production makes wide, simultaneous, plausible
changes cheap in a way hand-work never did, and a small, slow, wide change
troubles nothing else in this frame.

The hard constraint, in the shape P-5 uses. **The partition must be the
repository's own** — its ownership file's patterns, its workspace manifest,
its directory layout. Areas Heimdall inferred from reading the code would be
an opinion about someone else's architecture, which is the line this
instrument does not cross. Where a repository declares no partition that can
be read, this principle has nothing to say.

Two things that look like width and are not. A change that arrives with its
tests and its documentation reaches into several top-level directories, and
that is R-003 and R-011 being satisfied, not spread; test, documentation and
tooling areas never count. And a change that is one edit applied everywhere
— a rename, a lint fix, a toolchain migration, a version bump — is wide by
construction and reviewable by reading the rule once; the first run on a
package workspace put four of those at the top of its list.

This principle has a property most of the frame lacks. `README.md` says to
prefer the framing whose cheapest evasion is the desired behaviour, and
usually that framing is unavailable. Here it is not: the cheapest way to
clear a rule under this principle is to split the change, which is the
practice the rule was written for.

**Rules out:** treating a merge as one decision because it is one commit;
judging width against an absolute rather than against the repository's own
distribution at that size; counting accompaniment as spread; and inferring
which areas are related in a repository that has not said.

**Derived rules:** R-016 (an increment must not span more declared areas than
the repository's own norm for its size) — **draft**, and **advisory**. Its
first criterion was a percentile of the repository's own distribution, which
flags a fixed share of any distribution and is a finding machine rather than a
test; its second collapsed onto the absolute floor wherever the median was one.
It now requires all three of an absolute floor, a multiple of the band's
median, and the band's 90th percentile, and the survivors on a 41-area
workspace were the landings a reader would want listed — including one whose
subject called itself work in progress.
