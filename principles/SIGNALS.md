# Signals

What can actually be measured from a repository's history, what each measurement
suggests, and — the part that matters — how each one lies.

A signal is not a rule. Nothing here is a violation. These feed the rules in
`rules/` once best practices are defined; until then they are readings, and a
reading is reported as shape, never as deviation.

Every signal below must remain stated so that it would make sense pointed at any
repository.

---

## S-1. Review latency — merge lag against branch tip

**Measure:** the interval between the newest commit on a branch and the merge
commit that lands it.

**Suggests:** whether a merge could have been preceded by a read. A branch
merged within a minute of its last commit was not reviewed in that minute; the
physical time did not exist. This is the sharpest available evidence for the
difference between a human in the loop and a human on the merge button.

**How it lies — two ways, both real:**

1. **The branch may have been reviewed hours earlier.** A pull request sits
   open, gets read, receives one final fixup commit, and merges immediately
   after. Lag reads near zero; review happened. Measuring lag alone will
   systematically accuse a healthy workflow.
2. **Lag is not review.** A long gap means only that time passed. Nobody was
   necessarily reading during it.

**Read it with:** the branch's full lifetime (first commit to merge) and its
size. Short lifetime plus large size plus near-zero lag is the combination that
carries signal; lag alone is close to worthless.

---

## S-2. Digest rate — volume against elapsed time

**Measure:** lines changed per minute across runs of closely spaced commits, and
the share of a window's churn arriving inside such runs.

**Suggests:** whether production outran absorption. Careful reading of unfamiliar
code runs at a low, human, roughly knowable rate. Sustained output far above it
means the output was not read as it was produced — arithmetic, not judgement.

**How it lies:** it conflates production with the *opportunity* to absorb. A
person may read attentively while a machine writes. The measure bounds
absorption from above; it never demonstrates that nobody read.

**Read it as:** a ceiling on what could have been digested, not a claim about
what was.

**Derived rule:** R-006, which binds the ceiling over sustained runs of
commits — including in solo work, where R-001 and R-004 cannot reach.

---

## S-3. Machine attribution

**Measure:** commit trailers naming an AI co-author, committer identities that
differ from author identities, bot accounts, generation markers in commit
bodies.

**Suggests:** which share of the work was machine-produced.

**How it lies — badly, and this is the important one.** Attribution is opt-in
and trivially removed. A tool can be configured not to write the trailer, and
then entirely machine-generated history is indistinguishable from hand-written.
**Presence is strong evidence; absence is no evidence at all.** A signal that
can be switched off by the thing it measures must never carry a rule on its own.

**Read it as:** a floor on machine involvement. Never infer human authorship
from a missing trailer. Where attribution matters to a finding, corroborate with
signals that cannot be opted out of — S-2 rate, S-4 shape.

**Derived rule:** R-007 — which fires only on the conjunction of machine pace
and absent declaration, exactly because pace cannot be switched off and
attribution can. The warning above still governs: absence alone is never a
finding, and R-007's text repeats it.

---

## S-4. Dumping — concentration of a window's output

**Measure:** churn per document file; the largest single commit's share of a
window; whether specification and implementation changed in the same increment.

**Suggests:** whether the work arrived as a sequence of decisions or as one act.
A large specification and broad code change landing together indicates no point
at which the specification could have been reviewed before being built on.

**How it lies:** a genuine one-off — an initial import, a vendored dependency, a
generated client — looks identical. Establish what the increment contains before
reading its shape.

---

## S-5. Documentation drift

**Measure:** documentation churn against code churn; and documents left
untouched while the code they describe keeps moving.

**Suggests:** rot. Documentation produced faster than it can be read is
documentation that will not be read, and then not maintained. Both the growth
rate and the staleness are visible; the reading is not.

**How it lies:** the relationship between a document and the code it describes
is inferred from paths and timing, not from content. A document may legitimately
outlive the churn beneath it.

**Derived rule:** R-005, which narrows the inference to the checkable core — a
document that names a path is claiming it exists and is current.

---

## S-6. Reading weight — where the cost of reading sits

**Measure:** for code files, lines split into code, comment and blank, and the
comment share per file, against the repository's own median. For documents,
total length and — the figure that matters — the **longest unbroken span between
headings**, which is what a reader must cross to reach one answer. Both are
quoted in reading time at the same generous rate R-001 uses.
`bin/reading-weight` computes it.

It also separates the documents written for a **model** — `CLAUDE.md`,
`AGENTS.md`, agent, skill and rule files — and reports those in tokens rather
than spans, split by whether they load on every turn or on demand.

**Suggests:** where a repository's reading cost actually lives, and what is out
of line with its own norm. For agent files it suggests something different
again: a standing charge against the context budget that the work itself has to
share. Observed on real history — a 597-line file with thirty headings, ideal by
the human measure, was simultaneously the most expensive file in that repository
for a model. A file that is 80% comment is mostly prose and costs
prose to read; a 600-line document with four headings costs more to use than a
5,000-line one sectioned every eighty lines, and ranking by length gets that
backwards.

**How it lies:** in more ways than most of these, and none of them subtle.

*The comment share is not a quality score and must never be reported as one.*
It cannot distinguish careful explanation from commented-out code, a licence
header, or generated narration — a short file with a licence banner scores like
a well-documented one, which is why the outlier list has a length floor. Read it
as weight, never as merit; used as a score it is the most trivially gameable
number in this catalogue.

*The lexing is approximate.* Comment syntax is a vendored table for common
languages, not a lexer: markers inside string literals, nested block comments,
heredocs and regex literals are not modelled. Python docstrings count as
comments, which is right for reading weight and wrong if you wanted comments
strictly. Trailing comments on code lines count as code, deliberately — the line
must be read for its logic either way. Never quote one file's ratio as exact.

*Structure detection is convention-bound.* Headings are recognised in the
markdown and rst styles; a document organised some other way reads as
unstructured when it is not. And any heading counts as an entry point, which
over-credits a document with many shallow headings that do not actually
partition meaning.

*The token count is an estimate and the audience is a guess.* Tokens are
approximated at four characters each — the standard rough heuristic, wrong by a
noticeable margin for code, tables and non-English text, and wrong in a
model-specific way regardless. And whether a file is written for a model is
decided by path convention (`CLAUDE.md`, `.claude/`, `.cursor/`, `SKILL.md` and
friends); a repository with its own convention will have its instruction files
counted as prose, and a human-read document living under one of those paths
counted as machine-read. Which files load *every turn* is convention too, and
harness-specific.

*The distribution excludes what it could not read.* Generated, vendored and
unknown-syntax files are skipped, so a repository whose weight lives in a
language the table does not carry reads as lighter than it is. The count of
skipped files is printed for that reason.

**Derived rule:** R-010, from the document half only — the longest prose span
between headings, exempting spans that are mostly table or fenced code. The
comment-share half has no rule and should not acquire one, for the reason above:
it is weight, not merit.

---

## Method notes

**Absolute figures mean little.** Every signal is read against the same window
one period earlier — the repository's own history is the only honest yardstick.
`bin/window-facts --baseline` does that comparison.

**A dated, stated convention change is a window boundary.** Where the repository
records the moment its practice changed — an accepted decision record adopting a
branching model, a documented policy shift — its history has regimes, and a
window that straddles the boundary averages across them, producing a reading of
neither. Split at the boundary and read each side against its own kind. The same
caution applies to trends across the boundary: a change in the numbers may be
the convention taking effect, or the work changing character at the same moment
(a project winding down produces small careful increments because the increments
are small), and the split alone cannot say which.

**Window edges cut act-grouping.** Signals that group consecutive commits into
increments by time gap (`--act-gap`) will split a run that straddles a window
edge into two, so a windowed reading and a full-history reading can disagree by
an increment or two at the margins. Neither is wrong; compare like with like.

**The script counts; the agent interprets.** A process reading is mostly
numbers, and one wrong figure discredits the document.

**Retired: rework commit ratio.** The fraction of commits touching a
previously-touched file was measured at 84–98% across every real window tried.
At any scale most commits touch something an earlier commit touched, so it
reported that iteration exists rather than what it cost. Gross churn against net
churn carries the same concern honestly.

**Tested once, not established: fast work predicting later rework.** The
hypothesis that burst-produced files are rewritten more afterwards was tested on
one repository over one period. Naively it appeared to hold; the apparent effect
was largely a confound, because bursts are disproportionately where
documentation is produced, and documentation and source have different natural
rework rates. Stratified by file kind, the effect survived weakly for source
files and vanished for documentation, on a small sample. **Not strong enough to
treat as a leading indicator.** Worth re-testing on more history before any rule
depends on it — and worth remembering as the shape such a test takes: a
correlation that dissolves once you ask what kind of files it is made of.

---

## S-7. Declared stakes — which paths a repository singles out

**Measure:** whether the repository declares, in-tree and path-scoped, that
some paths need particular people — `CODEOWNERS` at any of its three valid
locations, or `OWNERS` files by placement — and if so, how many patterns it
carries, whether any is a catch-all, and what share of the files changed in a
window those patterns cover. `bin/stakes-proportionality` computes it.

**Suggests:** where a project believes its own risk sits, stated by the project
rather than inferred by this instrument. That distinction is the whole value: a
declaration is the target's own standard, and reading stakes off the code
instead would be an opinion about someone else's architecture. Coverage is the
informative half — seven patterns naming seven distinct owner sets says
something quite different from one line reading `* @org/team`, and a
declaration covering 87% of changed files says the project treats nearly
everything as owned.

**How it lies:**

*Absence is not indifference.* Six of seven repositories measured had no
declaration of any kind, including a mature workspace with careful review
practice. `CODEOWNERS` is a review-routing feature of one hosting platform;
not using it says the team does not route reviews that way, not that nobody
cares where it counts.

*Presence is not enforcement.* The file declares an intent. Whether the named
owners actually reviewed anything is on the hosting platform, outside git and
outside this instrument entirely.

*Currency is unverifiable.* An owner set can name people who left years ago,
and the file reads identically either way.

**Signal and rule, not signal instead of rule.** R-008 derives from the same
measurement and asks a narrower question: whether the paths a repository marks
most heavily get at least the review window of the paths it marks least. That
rule needs a populated gradient and often has none. This signal has no such
requirement — it reports the declaration and its coverage on any repository that
has one, including every repository where the rule returns not-applicable, which
so far is all of them. Report the shape; leave the scoring to R-008.
