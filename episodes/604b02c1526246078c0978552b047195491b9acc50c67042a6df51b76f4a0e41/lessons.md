# Lessons: free-claude-code

Lineage `604b02c1526246078c0978552b047195491b9acc50c67042a6df51b76f4a0e41` on [https://github.com/alishahryar1/free-claude-code](https://github.com/alishahryar1/free-claude-code) at `74d4ccfde724f7416fa2a728d594d704dddc7c94`. 26 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target perf:aggregate_ir

- Attempts: 12 (3 accepted, 1 rejected, 8 no candidate)
- Best accepted effect: 3.76% (ratio 0.9624), episode `9b232f5e2458`
- Accepted effects: 3.76%, 1.19%, 1.03%
- Why candidates failed: no_improvement (1)
- Files changed by accepted candidates: `src/free_claude_code/core/anthropic/sse_aggregation.py`
- Files changed by rejected candidates: `src/free_claude_code/core/anthropic/sse_aggregation.py`
- Median model spend per attempt: 0.6647 USD (over 12 attempts with a recorded cost)

## Target perf:decode_ir

- Attempts: 14 (5 accepted, 9 no candidate)
- Best accepted effect: 13.58% (ratio 0.8642), episode `323e221229a9`
- Accepted effects: 13.58%, 2.49%, 1.64%, 1.23%, 1.13%
- Files changed by accepted candidates: `src/free_claude_code/core/anthropic/stream_contracts.py`, `src/free_claude_code/core/anthropic/streaming/decoder.py`
- Median model spend per attempt: 0.7544 USD (over 14 attempts with a recorded cost)

## In the agent's own words

The agent's journal entries on this lineage, newest first, verbatim. They are its own notes: claimed, not checked.

> I tried decode-path parsing/list-allocation changes and small framing updates in src/free_claude_code/core/anthropic/streaming/decoder.py and src/free_claude_code/core/anthropic/stream_contracts.py. No function name is recorded for these edits.
> Evaluation 1 was rejected: no_improvement, worst ratio 0.9997 vs required 0.9900. Evaluation 3 was rejected: worst ratio 1.0001. Evaluation 5 was rejected: worst ratio 1.0002. Builds were ok. Equivalence was identical outputs. Guards were ok.
> The decode_ir path did not move with these edits. The changes preserved outputs and stayed within guards, but the measured ratios stayed around 1.000.
> Try a different part of decode_ir. Use lineage_bench.py and lineage_equiv.py to isolate cost before editing again. No candidate was submitted.
>
> 2026-10-14, episode `21abfbbb3ba6` (no candidate)

> I tried a content_block_delta fast path in src/free_claude_code/core/anthropic/sse_aggregation.py. Edits touched lines 22-25, 97-132, 232-241, 46-57, 176-178, 263-276, 91-97, 189-209, 256-273, and 24-46.
> Evaluation 1 was rejected: aggregate_ir ratio 0.9961. Evaluation 3 was rejected: aggregate_ir ratio 0.9945. Evaluation 5 was rejected: aggregate_ir ratio 0.9957. Evaluation 7 was rejected: aggregate_ir ratio 0.9961. Builds were ok. Outputs were identical. Guards were ok.
> Best observed aggregate_ir ratio was 0.9945 against required 0.9900. decode_ir ratios were 1.0022, 1.0026, 1.0033, and 1.0029. The fast path preserved output but did not help enough.
> Try a different shape in sse_aggregation.py, especially around lines 91-132 and 176-209. Re-read stream_contracts.py and streaming/decoder.py before another aggregate_ir run. No candidate was submitted.
>
> 2026-10-14, episode `70ea3fed226c` (no candidate)

> I tried to reduce decode_ir in src/free_claude_code/core/anthropic/streaming/decoder.py and src/free_claude_code/core/anthropic/stream_contracts.py. I worked from parse_sse_text, AnthropicSSEDecoder, feed_frames, and SSEEvent. The largest edits replaced stream_contracts.py lines 81-136 with 67 lines and decoder.py lines 3-13 with 31 lines.
> Evaluation 1 was "outcome: rejected" with "no_improvement: worst ratio 0.9999 vs required 0.9900". Evaluation 3 was "outcome: rejected" with "worst ratio 0.9992". Evaluation 5 was "outcome: rejected" with "worst ratio 0.9991". Each run had "equivalence: identical outputs". The session ended with no candidate.
> The decode_ir changes did not help enough. aggregate_ir moved to ratio 1.0007, 1.0008, and 1.0015. Guard stayed ok, but the target stayed near parent.
> Next try should use lineage_bench.py and lineage_equiv.py on smaller decoder.py changes first. Check parse_sse_text and feed_frames separately.
>
> 2026-10-14, episode `2996d12b8968` (no candidate)

> Changed src/free_claude_code/core/anthropic/sse_aggregation.py in _sse_payload and aggregate_anthropic_sse_to_message. Tried reordering content_block_delta handling and fast parsing canonical content_block_delta frames.
> evaluation 1 was rejected: aggregate_ir ratio 1.0011. evaluation 3 was rejected: aggregate_ir ratio 0.9996. evaluation 5 was rejected: aggregate_ir ratio 0.9997. evaluation 7 was rejected: aggregate_ir ratio 0.9995. Builds were ok. Equivalence had identical outputs.
> The aggregate-side hot path did not move enough. The changes stayed correct, but the measured aggregate_ir ratios did not reach required 0.9900.
> Try a different part of the stream path next. Read streaming/decoder.py with lineage_bench.py and lineage_equiv.py before editing.
>
> 2026-10-14, episode `c6d43dce5c51` (no candidate)

> I tried a bounded decode-path fast-event parsing change. I edited src/free_claude_code/core/anthropic/streaming/decoder.py at lines 20-25, 3-7, 15-19, 3-4, and 45-56. I edited src/free_claude_code/core/anthropic/stream_contracts.py at lines 131-133, 18-19, and 80-138. The target path was AnthropicSSEDecoder, parse_sse_text, and feed_frames.
> Evaluation 1 was “outcome: rejected (equivalence_changed: candidate output differs from parent on seeded inputs)”. decode_ir ratio was 0.9971. Evaluation 3 was “outcome: rejected (no_improvement: worst ratio 0.9978 vs required 0.9900)”. Evaluation 5 had decode_ir ratio 0.9976. Evaluation 7 had decode_ir ratio 1.0004. Evaluations 3, 5, and 7 had identical outputs.
> The change preserved equivalence after evaluation 1, but it did not meet the required decode_ir improvement. aggregate_ir moved to 1.0009, 1.0010, and 1.0015 on the later runs.
> Next try a smaller parse_sse_text change in decoder.py and leave stream_contracts.py alone. Run perf decode_ir and equivalence before expanding it.
>
> 2026-10-13, episode `5bfc3ebb8a20` (no candidate)

## Licence

Code excerpts from https://github.com/alishahryar1/free-claude-code at 74d4ccfde724f7416fa2a728d594d704dddc7c94, copyright its authors; the licence was not determined (see the repository).
