# Lessons: free-claude-code

Lineage `604b02c1526246078c0978552b047195491b9acc50c67042a6df51b76f4a0e41` on [https://github.com/alishahryar1/free-claude-code](https://github.com/alishahryar1/free-claude-code) at `74d4ccfde724f7416fa2a728d594d704dddc7c94`. 15 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target perf:aggregate_ir

- Attempts: 8 (2 accepted, 1 rejected, 5 no candidate)
- Best accepted effect: 3.76% (ratio 0.9624), episode `9b232f5e2458`
- Accepted effects: 3.76%, 1.03%
- Why candidates failed: no_improvement (1)
- Files changed by accepted candidates: `src/free_claude_code/core/anthropic/sse_aggregation.py`
- Files changed by rejected candidates: `src/free_claude_code/core/anthropic/sse_aggregation.py`
- Median model spend per attempt: 0.5940 USD (over 8 attempts with a recorded cost)

## Target perf:decode_ir

- Attempts: 7 (4 accepted, 3 no candidate)
- Best accepted effect: 13.58% (ratio 0.8642), episode `323e221229a9`
- Accepted effects: 13.58%, 2.49%, 1.64%, 1.23%
- Files changed by accepted candidates: `src/free_claude_code/core/anthropic/stream_contracts.py`, `src/free_claude_code/core/anthropic/streaming/decoder.py`
- Median model spend per attempt: 0.5036 USD (over 7 attempts with a recorded cost)

## In the agent's own words

The agent's journal entries on this lineage, newest first, verbatim. They are its own notes: claimed, not checked.

> I tried to reduce aggregate_ir in src/free_claude_code/core/anthropic/sse_aggregation.py. I changed aggregate_anthropic_sse_to_message and helpers. Edits touched lines 95-131, 100-120, 37-42, 72-77, 22-47, then rewrote lines 1-240 as 209 lines.
> evaluation 1 was rejected: worst ratio 0.9996 vs required 0.9900. evaluation 3 was rejected: worst ratio 0.9997 vs required 0.9900. evaluation 5 was rejected: worst ratio 0.9997 vs required 0.9900. evaluation 7 was rejected: worst ratio 1.0002 vs required 0.9900. All had build ok and identical outputs. The session stopped without submitting a change.
> The code stayed equivalent. The edits did not help. The target metric did not clear the required ratio, and decode_ir stayed at 1.0000, 1.0001, 1.0003, then 1.0009.
> Next try should isolate _sse_payload, parse_sse, and AnthropicSSED... before changing aggregate_anthropic_sse_to_message again.
>
> 2026-10-13, episode `5534779bd764` (no candidate)

> I tried bounded decode-path reductions in decoder framing and fast SSE event parsing. I edited src/free_claude_code/core/anthropic/streaming/decoder.py and src/free_claude_code/core/anthropic/stream_contracts.py, including AnthropicSSEDecoder.feed and parse_sse_text.
> evaluation 1 was rejected with decode_ir ratio 0.9991. evaluation 3 was rejected with decode_ir ratio 0.9991. evaluation 5 was rejected with decode_ir ratio 0.9968. evaluation 7 was rejected with decode_ir ratio 0.9961. The required ratio was 0.9900. Builds were ok. Equivalence was identical outputs. Guards were ok.
> I now believe these bounded changes did not help enough. The best measured decode_ir ratio was 0.9961. aggregate_ir stayed near 1.0000 and did not show useful movement.
> Next try a different decode_ir path. Re-read lineage_bench.py and lineage_equiv.py before changing more parser code.
>
> 2026-10-13, episode `5bc22d6ac3a6` (no candidate)

> Changed src/free_claude_code/core/anthropic/sse_aggregation.py for aggregate_ir. The session facts record line edits, not a function name. Read stream_contracts.py, streaming/decoder.py, lineage_bench.py, and lineage_equiv.py.
> evaluation 1 was rejected: no_improvement: worst ratio 0.9996 vs required 0.9900. evaluation 3 was rejected at ratio 0.9987. evaluation 5 was rejected at ratio 0.9994. evaluation 7 was rejected at ratio 0.9938. Each build was ok. Equivalence was identical outputs. Guard was ok.
> The best measured aggregate_ir change was parent 1548971772, candidate 1539408272, ratio 0.9938. decode_ir moved to parent 1525836749, candidate 1534283872, ratio 1.0055. The changes did not help enough. No candidate was submitted.
> Try next inside src/free_claude_code/core/anthropic/sse_aggregation.py. Keep the output contract identical. Measure aggregate_ir and decode_ir again before any submission.
>
> 2026-10-13, episode `8859ceb4af79` (no candidate)

> Changed src/free_claude_code/core/anthropic/stream_contracts.py in parse_sse_text and parse_sse_lines, and src/free_claude_code/core/anthropic/streaming/decoder.py in AnthropicSSEDecoder. Tried avoiding per-frame parse list allocation, reducing boundary scans, and avoiding joins for complete chunk-local frames.
> Own sandbox evaluations had build ok and identical outputs. evaluation 1 was rejected: decode_ir ratio 0.9992, aggregate_ir ratio 1.0005. evaluation 3 was rejected: decode_ir ratio 0.9984, aggregate_ir ratio 1.0002. evaluation 5 was rejected: decode_ir ratio 0.9964, aggregate_ir ratio 0.9990. evaluation 7 was rejected: decode_ir ratio 0.9955, aggregate_ir ratio 0.9984.
> The code kept equivalence but did not meet the required 0.9900 decode_ir ratio. The session stopped without submitting a change.
> Try a narrower change in AnthropicSSEDecoder. Measure boundary handling separately from parse_sse_text. Stop if decode_ir stays above ratio 0.9900.
>
> 2026-10-13, episode `c53eda6f029c` (no candidate)

> Changed src/free_claude_code/core/anthropic/sse_aggregation.py in aggregate_anthropic_sse_to_message. Tried SSE payload detection and ignored-frame handling changes.
> Own sandbox evaluation 1: “outcome: rejected (no_improvement: worst ratio 1.0038 vs required 0.9900)”. Evaluation 3: “worst ratio 0.9997 vs required 0.9900”. Evaluation 5: “worst ratio 0.9989 vs required 0.9900”. Evaluation 7: “worst ratio 0.9991 vs required 0.9900”. Builds were ok. Equivalence was identical outputs. Guards were ok.
> I now believe these small aggregation-path changes did not help enough. decode_ir ratios were 1.0008, 1.0000, 0.9997, and 1.0002.
> Try a different path in src/free_claude_code/core/anthropic/sse_aggregation.py. Read lineage_bench.py and lineage_equiv.py again before editing. No candidate was submitted.
>
> 2026-10-13, episode `31973c8ddd5a` (no candidate)

## Licence

Code excerpts from https://github.com/alishahryar1/free-claude-code at 74d4ccfde724f7416fa2a728d594d704dddc7c94, copyright its authors; the licence was not determined (see the repository).
