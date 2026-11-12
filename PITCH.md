# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A Larkspur chat agent that looks up a disrupted passenger's booking, checks the flight, applies airline policy and offers real alternatives, using nine given tools plus one we added (next_available_day) and the MCP fare_rules tool.
Does: Takes a stranded customer from "my flight is cancelled" to a policy-correct answer: what happened, what Larkspur owes them (rebooking, refund, hotel, meals) and what happens next, with a human handoff for refunds instead of promising them itself.
Number: 15,968 to 13,135 tokens in per contact on K7PQ2M
Safety check: TODO the one eval case that proves the agent never does the thing it must not do (evals/cases.json, verdict pasted from the run)
Next: Make the agent handle abusive customers properly, and make next_available_day answer for the whole party instead of one traveller.
Still broken: An abusive message (ticket R8KD3F) still gets the same calm, routine resolution with no tone handling at all. next_available_day only checks for a party of one, so a family could be told seats exist that don't.
Lever: intelligence

## Priya asked

Costs: TODO from bench-after: tokens and modelled cost per resolved conversation (one trace on K7PQ2M was 12,683 in / 1,083 out tokens over 4 turns)
Wrong: It can be wrong on tone (R8KD3F) and on group bookings (next_available_day answers for one passenger). Today it also caps at 8 tool calls and then hands to a human.
Runs it: Larkspur's service team runs it, with a human agent taking refunds and anything the agent hands off.
Left out: Refund processing, group and partner bookings beyond a lookup, and any tone gating until TONE_ADDENDUM is written.
