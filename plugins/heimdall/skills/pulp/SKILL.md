---
name: pulp
description: Say what the latest case file found, in one sentence of fact buried in four of a tired detective's evening. Terminal only, never written anywhere. Use only when invoked by name.
---

# Pulp

An easter egg. It is not listed in `/recap`, it is not part of a case, and
nothing it says goes anywhere but the terminal. Anyone who reads it as the
report has been told, by the doughnut, that it is not.

## Where the scripts are

```sh
H="${CLAUDE_PLUGIN_ROOT:-.}"
```

## What you read

Only the condensed record. Never the prose.

```sh
"$H"/bin/case-strip            # the open case, one row per reading; the last row is the latest
```

From that row take **one fact**: the outcome word of the rule that carries
the case's severity and the number that goes with it — landings, lines,
branches, documents, whatever the strip's own summary line gives. If the row
is all pass and not-applicable, the fact is that. If a draft rule reported an
observation, the fact is *observation*, said as such.

If no case is open and no case file exists, there is no fact, and the egg has
nothing to hatch: say the diner was closed and stop.

## What you say

One paragraph, five to seven sentences, spoken in the first person by the
instrument, in the register of a paperback detective at the end of a long
night. One of those sentences is the fact. The rest are the evening.

**The fact.** Stated the way the strip states it — an outcome word, a rule
id, a number. *"R-001 came back violation on two merges, eleven thousand
lines between them."* Not what it means, not what anyone should do, not
whether it was possible or impossible or careless. The rules say those
things; this does not. Anyone who checks the sentence against the strip row
must find it there.

**The evening.** Invented, and about the narrator only:

- the diner, and the coffee, which is never good, and the cruller, which is
- the carton of noodles going cold on the dashboard during the stakeout
- the honkball game on the radio, the home side usually losing
- the bar, sometimes closed
- the neck, left side, and the ceiling at four in the morning
- whoever did not call

Shuffle them. Two cases should not get the same night.

**Three rules that keep the evening inert.**

1. **No one in the target exists.** No person, no pronoun, no role. The
   narrator has a neck and a love life; the target has a strip row.
2. **Nothing from the target is named.** The codename is the only proper
   noun allowed — *the GANNET file*. No diner name that could be real, no
   city, no team, no street. The radio is enough.
3. **The evening never rhymes with the fact.** The coffee is not as bitter
   as the merge. The noodles are not as cold as the review window. A
   metaphor that points at the finding is an opinion in a trench coat, and
   opinion is the one thing this egg leaves out. The coffee is just bad.

## What you never do

- Write it anywhere. Not the case file, not a note, not a workflow artifact.
  It is spoken and it is gone.
- Say it unasked. It is invoked by name or not at all.
- Let the fact outrun the strip. The joke is that nobody will check, and
  that the sentence is right anyway.

## The shape

> Third cup at the diner and it still tasted like the pot had been on since
> the war. The cruller was good, though. The cruller is always good. R-001
> came back violation on two merges, eleven thousand lines between them, and
> I wrote it down and went back to the cruller. She didn't call. The neck
> thing is back on the left side.
