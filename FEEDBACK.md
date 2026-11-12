# Client review: Larkspur disruption-care agent

**To:** Room 5  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-12 09:46

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. agent.py build_tools(): search_alternatives description is "search for alternative flights", 30 characters, up from 6.**

Claude picks the tool from that string alone. It says nothing about the pnr being the only input, or that origin, destination, date and cabin are derived from the booking, or what a result looks like. The sibling descriptions are 216 to 445 characters. hold_seat at 73 and send_confirmation at 71 are the other short ones.

Run python3 run.py --show-tools and paste the search_alternatives entry, then rewrite it and paste the new text next to the old.

**2. readout-trace.json: the committed run called lookup_booking, get_flight_status, check_policy, then stopped at 4 API turns and 3 tool calls.**

search_alternatives never appears in the tools-called list for that run. The agent gave a customer-facing answer without searching for a single alternative flight. A larger model reading the same 30-character description has the same information to go on.

Run python3 run.py K7PQ2M --trace and paste the tool sequence, then say whether search_alternatives was called and why.

**3. agent.py: TONE_ADDENDUM is 0 characters, and PITCH.md names R8KD3F as still getting a calm, routine resolution.**

Your Lever line is intelligence and your Next line is abusive customers, so the tone gap is your own stated target. The system prompt is SYSTEM_PROMPT plus an empty string, and the model has no instruction for threats or abuse. A model upgrade changes nothing in that concatenation.

Run python3 verify.py 4.1 and paste the output, which is the baseline for the tone fix.

**4. There is no evals/cases.json, and PITCH.md Number and Safety check are both still TODO.**

Nothing in the repository yet shows a before and after on any metric. The only measured number is one trace: 12683 tokens in, 1083 out, 10.9s, on one booking. Gates 1.2 to 2.2 are banked in readout.html. 3.1 and 4.1 are not.

Run python3 bench.py --label before --stage 1 --runs 3 now, make your change, run it again as after, and paste the python3 bench.py --compare before after output.

**5. readout-trace.json shows 0 cache read, 0 written, hit ratio None, and agent.py contains no cache_control.**

Every turn resends runtime_preamble() + SYSTEM_PROMPT + TONE_ADDENDUM and all nine tool schemas, plus any MCP tools. check_policy alone is 445 characters. The 12683 input tokens over 4 turns include that prefix four times, uncached.

Run python3 run.py --tool-tax and paste the output, so the per-turn schema cost is a number before anyone argues about model price.

## Your four answers

Four of the lines in your PITCH.md are answers to me rather than to your
verifier, and somebody on your side wrote them between our sessions. I read
those beside the code, not instead of it. Where an answer is carrying a number,
I have said whether the repository backs it.

| My question | Your answer | My read |
| --- | --- | --- |
| **What it costs** | TODO from bench-after: tokens and modelled cost per resolved conversation (one trace on K7PQ2M was 12,683 in / 1,083 out tokens over 4 turns) | **Thin.** The answer is a TODO that carries one trace, "12,683 in / 1,083 out tokens over 4 turns", which matches readout-trace.json but covers a single booking. No bench-after.json exists, so there is no per-conversation figure or modelled cost yet. |
| **When it is wrong** | It can be wrong on tone (R8KD3F) and on group bookings (next_available_day answers for one passenger). Today it also caps at 8 tool calls and then hands to a human. | **Answered.** R8KD3F and the single-passenger next_available_day are specific, and TONE_ADDENDUM at 0 characters backs the tone claim. The "caps at 8 tool calls" claim matches MAX_TOOL_CALLS = 8, but evals/cases.json would be needed to show how often either failure happens. |
| **Who runs it** | Larkspur's service team runs it, with a human agent taking refunds and anything the agent hands off. | **Thin.** "Larkspur's service team" is a department with no named owner or on-call role. Nothing in TEAM.md or the repo identifies who receives the escalate_to_human queue or who reviews tone failures. |
| **What you left out** | Refund processing, group and partner bookings beyond a lookup, and any tone gating until TONE_ADDENDUM is written. | **Answered.** The list is specific and consistent with the code: TONE_ADDENDUM is empty and escalate_to_human covers refunds and groups. Gate 4.1 is not banked, which matches "any tone gating until TONE_ADDENDUM is written". |

All four answered. Bring the artifact behind each one to our next meeting, not the sentence.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (228 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- `readout.html (evidence block)`
- `PITCH.md (4 of 4 answers to Priya)`
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `pod_sync.py`, `run.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
