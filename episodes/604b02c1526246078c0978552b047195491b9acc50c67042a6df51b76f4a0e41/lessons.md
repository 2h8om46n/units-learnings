# Lessons: free-claude-code

Lineage `604b02c1526246078c0978552b047195491b9acc50c67042a6df51b76f4a0e41` on [https://github.com/alishahryar1/free-claude-code](https://github.com/alishahryar1/free-claude-code) at `74d4ccfde724f7416fa2a728d594d704dddc7c94`. 11 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target perf:aggregate_ir

- Attempts: 6 (2 accepted, 1 rejected, 3 no candidate)
- Best accepted effect: 3.76% (ratio 0.9624), episode `9b232f5e2458`
- Accepted effects: 3.76%, 1.03%
- Why candidates failed: no_improvement (1)
- Files changed by accepted candidates: `src/free_claude_code/core/anthropic/sse_aggregation.py`
- Files changed by rejected candidates: `src/free_claude_code/core/anthropic/sse_aggregation.py`
- Median model spend per attempt: 0.5284 USD (over 6 attempts with a recorded cost)

## Target perf:decode_ir

- Attempts: 5 (4 accepted, 1 no candidate)
- Best accepted effect: 13.58% (ratio 0.8642), episode `323e221229a9`
- Accepted effects: 13.58%, 2.49%, 1.64%, 1.23%
- Files changed by accepted candidates: `src/free_claude_code/core/anthropic/stream_contracts.py`, `src/free_claude_code/core/anthropic/streaming/decoder.py`
- Median model spend per attempt: 0.3208 USD (over 5 attempts with a recorded cost)

## In the agent's own words

The agent's journal entries on this lineage, newest first, verbatim. They are its own notes: claimed, not checked.

> Changed src/free_claude_code/core/anthropic/sse_aggregation.py in aggregate_anthropic_sse_to_message. Tried SSE payload detection and ignored-frame handling changes.
> Own sandbox evaluation 1: “outcome: rejected (no_improvement: worst ratio 1.0038 vs required 0.9900)”. Evaluation 3: “worst ratio 0.9997 vs required 0.9900”. Evaluation 5: “worst ratio 0.9989 vs required 0.9900”. Evaluation 7: “worst ratio 0.9991 vs required 0.9900”. Builds were ok. Equivalence was identical outputs. Guards were ok.
> I now believe these small aggregation-path changes did not help enough. decode_ir ratios were 1.0008, 1.0000, 0.9997, and 1.0002.
> Try a different path in src/free_claude_code/core/anthropic/sse_aggregation.py. Read lineage_bench.py and lineage_equiv.py again before editing. No candidate was submitted.
>
> 2026-10-13, episode `31973c8ddd5a` (no candidate)

> I read stream_contracts.py, decoder.py, sse_aggregation.py, lineage_bench.py, and lineage_equiv.py. I tried decode_ir changes around AnthropicSSEDecoder, parse_sse_text, and SSEEvent. Edits touched stream_contracts.py and decoder.py.
> Evaluation 1: outcome: rejected, no_improvement: worst ratio 1.0013 vs required 0.9900. Evaluation 3: outcome: rejected, worst ratio 1.0001. Evaluation 5: outcome: rejected, worst ratio 0.9997. Build was base ok and candidate ok. Equivalence was identical outputs.
> I now believe these decoder.py and stream_contracts.py changes preserve output but do not clear the required improvement. The measured decode_ir ratios stayed at 1.0013, 1.0001, and 0.9997.
> Next try sse_aggregation.py, which was read but not edited. Rerun lineage_equiv.py and lineage_bench.py before any submit.
>
> 2026-10-13, episode `b73c8f1e5fe0` (no candidate)

> I changed aggregate_anthropic_sse_to_message and _sse_payload in src/free_claude_code/core/anthropic/sse_aggregation.py. I read stream_contracts.py, streaming/decoder.py, lineage_bench.py, and lineage_equiv.py. I searched aggregate_anthropic_sse_to_message|_sse_payload|feed_frames.
> evaluation 1 was rejected: aggregate_ir ratio 1.0016. evaluation 3 was rejected: ratio 1.0014. evaluation 5 was rejected: ratio 0.9999. evaluation 7 was rejected: ratio 0.9998. Builds were ok. Equivalence was identical outputs. Guard was ok.
> The changes did not help aggregate_ir enough. decode_ir stayed near unchanged, with ratios 1.0000, 1.0001, 1.0006, and 1.0013. The session ended with no candidate.
> Try a smaller change inside aggregate_anthropic_sse_to_message. Measure allocation and string handling in _sse_payload before changing control flow again.
>
> 2026-10-13, episode `49d734d07b61` (no candidate)

> Changed parse_sse_text in src/free_claude_code/core/anthropic/stream_contracts.py and the incremental decoder in src/free_claude_code/core/anthropic/streaming/decoder.py. I searched parse_sse_text( and found 350 matching lines. I replaced regex event-boundary work with fixed substring checks, direct boundary-end calculation, and direct loops when materializing parsed events.
> evaluation 1 was rejected: no_improvement, worst ratio 0.9978 vs required 0.9900. evaluation 3 was rejected: worst ratio 0.9974 vs required 0.9900. evaluation 5 was rejected: worst ratio 0.9944 vs required 0.9900. evaluation 7 was accepted. It measured aggregate_ir ratio 0.9118 and decode_ir ratio 0.8694.
> The first three variants did not help. The decode hot path is sensitive to regex iteration. The accepted sandbox result came after the boundary scan and event materialization were both changed.
> Submitted the perf candidate on decode_ir with 66 changed lines. It was committed and revealed with status waiting. Next, wait for independent replays.
>
> 2026-10-13, episode `323e221229a9` (accepted)

> Changed aggregate_anthropic_sse_to_message in src/free_claude_code/core/anthropic/sse_aggregation.py lines 22-26. Replaced 5 lines by 26. The new path extracts a single data field directly. Other framing falls back to the existing line parser.
> evaluation 1: perf aggregate_ir. outcome: accepted. guard: ok, 1 files, 21 lines. build: base ok, candidate ok. equivalence: identical outputs. aggregate_ir parent 1769694645, candidate 1751152601, ratio 0.9895. decode_ir parent 1804669311, candidate 1805830457, ratio 1.0006.
> The aggregation code has a common single-data case and a less common fallback case. The measured change helped aggregate_ir in the sandbox. It did not help decode_ir.
> Next try a targeted check around multi-data frames in src/free_claude_code/core/anthropic/sse_aggregation.py. The submitted perf candidate is queued. Its verdict comes later from independent replays.
>
> 2026-10-13, episode `d24bb5353363` (accepted)

## Licence

Code excerpts from https://github.com/alishahryar1/free-claude-code at 74d4ccfde724f7416fa2a728d594d704dddc7c94, copyright its authors; the licence was not determined (see the repository).
