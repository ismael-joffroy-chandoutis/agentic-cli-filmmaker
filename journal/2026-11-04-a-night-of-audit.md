[English](2026-11-04-a-night-of-audit.md) · [Français](2026-11-04-une-nuit-d-audit.fr.md)

# 4 November 2026: a night of audit

Logbook. A Claude Code system running across several machines for an artist and filmmaker: what was measured, what broke, what was fixed, what is still open.

## What a session loads before the first word

Measured on a real session log: before the person types anything, a session loads about 322 KB of text, 85,000 to 95,000 tokens. Half of it is the list of specialised agents (167 KB for 387 descriptions). Then come the global instructions file (49.5 KB), the memory index, the list of 571 skills and the names of 631 tools loaded on demand.

Anthropic's documentation recommends an instructions file under 200 lines, and turning any rule that allows no exception into a hook. The proposal: keep every rule, move long procedures into files read on demand, cut each agent description to 200 characters. Estimated gain: from 90,000 to 35,000 tokens per session. This is an estimate, to be measured once applied.

## The go that armed itself

Nothing goes to a human without an explicit "go envoi <name>" message. A hook blocks every sending command (mail, messaging apps) until a human message arms it, for thirty minutes and one message.

The audit found that the arming pattern accepted the bare word "go". The hook's log showed 23 wrongful armings, three of them on the sentence "don't launch anything without a go" (in French, "ne lance rien sans go"). The gate opened on sentences that asked precisely for it to stay shut.

Fix: the pattern now requires "go envoi" followed by a name, excludes negations ("without a go", "wait for my go") and passes a bench of twelve sentences that must or must not arm it. Twelve out of twelve.

## Silent failures

The silent-failure register holds 105 entries since August: automations that did nothing while nothing said so. One question finds them: if it does nothing, what do I see?

This night added several. On the Mac mini server, the macOS task manager frozen since the evening before: a watchdog meant to run every two minutes showed 4,821 runs at 01:20 and still 4,821 at 01:33. A macOS kernel memory leak: 9.49 GB of the 11.6 GB of wired memory in a single zone (`data.kalloc.1024`), while all programs together use less than 2. Only a reboot gives it back. And while this logbook was being written, the external work disk unmounted for a few seconds: a file written at that moment was lost without any error, found back at its original size. It had to be redone and its content checked.

The method and eighteen anonymised cases are published in [proof-gate](https://github.com/ismael-joffroy-chandoutis/proof-gate).

## Agnostic

Three layers come apart: the model, the harness (Claude Code or another one), the files and tools. Of 332 skills, 323 already follow the open Agent Skills format, readable by other tools. The main system stays on Claude Code, the one place where remote control, Artifacts and connectors work together. What no other harness runs as is: the sending gate. It is the most expensive part to port.

## Sessions becoming bots

6,799 conversations in five months, 2,245 in October alone. Counting the subjects that come back tells which sessions deserve to become permanent bots, each with its own small interface, on the model of [Inboxt](https://github.com/ismael-joffroy-chandoutis/inboxt), the inbox published today. The rule stays the same: the bot prepares, the person decides.

## Open

- Apply the slimmer instructions file, then measure the real gain.
- Put the twelve-sentence bench into the daily automatic check.
- Reboot the Mac mini server to clear the kernel leak, and watch whether it comes back.
