# Lessons: free-claude-code

Lineage `604b02c1526246078c0978552b047195491b9acc50c67042a6df51b76f4a0e41` on [https://github.com/alishahryar1/free-claude-code](https://github.com/alishahryar1/free-claude-code) at `74d4ccfde724f7416fa2a728d594d704dddc7c94`. 6 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target perf:aggregate_ir

- Attempts: 3 (1 accepted, 1 rejected, 1 no candidate)
- Best accepted effect: 3.76% (ratio 0.9624), episode `9b232f5e2458`
- Why candidates failed: no_improvement (1)
- Files changed by accepted candidates: `src/free_claude_code/core/anthropic/sse_aggregation.py`
- Files changed by rejected candidates: `src/free_claude_code/core/anthropic/sse_aggregation.py`
- Median model spend per attempt: 0.4820 USD (over 3 attempts with a recorded cost)

## Target perf:decode_ir

- Attempts: 3 (3 accepted)
- Best accepted effect: 2.49% (ratio 0.9751), episode `710c5e2617d0`
- Accepted effects: 2.49%, 1.64%, 1.23%
- Files changed by accepted candidates: `src/free_claude_code/core/anthropic/stream_contracts.py`, `src/free_claude_code/core/anthropic/streaming/decoder.py`
- Median model spend per attempt: 0.3029 USD (over 3 attempts with a recorded cost)

## In the agent's own words

The agent's journal entries on this lineage, newest first, verbatim. They are its own notes: claimed, not checked.

> I read sse_aggregation.py, stream_contracts.py, decoder.py, and lineage_bench.py. I edited aggregate_anthropic_sse_to_message in src/free_claude_code/core/anthropic/sse_aggregation.py. I also edited src/free_claude_code/core/anthropic/streaming/decoder.py lines 38-51. I tried fast-path parser changes for aggregate_ir.
> evaluation 1: outcome: rejected, ratio 0.9903 vs required 0.9900. evaluation 3: outcome: rejected, ratio 0.9903 vs required 0.9900. evaluation 5: outcome: rejected, ratio 0.9907 vs required 0.9900. evaluation 7: outcome: rejected, ratio 0.9904 vs required 0.9900. Each build was base ok, candidate ok. Equivalence was identical outputs.
> I now believe the aggregate parser path has small measurable movement, but these changes did not clear the required 1% improvement. decode_ir moved to 1.0006, 1.0007, or 1.0009 in these runs, so the parser work also touched guarded cost.
> Next try should inspect aggregate_anthropic_sse_to_message again and isolate aggregate_ir work from decoder.py changes. The session stopped without submitting a change.
>
> 2026-10-13, episode `77cfa3290222` (no candidate)

> Changed src/free_claude_code/core/anthropic/streaming/decoder.py lines 18-26 and src/free_claude_code/core/anthropic/stream_contracts.py lines 77-103. The change added decode-path fast paths for common complete SSE frames with optional keepalive comments and CRLF framing, kept the generic parser fallback, and skipped generator construction when a fed chunk produces no complete frames.
> evaluation 1 was rejected: no_improvement, worst ratio 0.9980 vs required 0.9900. build: base ok, candidate ok. equivalence: identical outputs. aggregate_ir ratio 1.0000. decode_ir ratio 0.9980. evaluation 3 was accepted in the sandbox. guard: ok, 2 files, 32 lines. build: base ok, candidate ok. equivalence: identical outputs. aggregate_ir ratio 1.0004. decode_ir ratio 0.9836.
> The first decoder edit did not help enough. The later edit helped decode_ir in the sandbox while keeping outputs identical.
> Next, wait for independent replay on the submitted perf candidate. If it holds, inspect the same decode path for more complete-frame cases and no-frame chunks.
>
> 2026-10-13, episode `dd8c1f41ced6` (accepted)

> I changed src/free_claude_code/core/anthropic/sse_aggregation.py. I tried direct complete-frame handling in the aggregation loop and a fast path for the common single data-field SSE frame. I kept fallback handling for multi-line data fields.
> Evaluation 1 was rejected, aggregate_ir ratio 0.9995. Evaluation 3 was rejected, aggregate_ir ratio 0.9907. Evaluation 5 was rejected, aggregate_ir ratio 0.9922. Evaluation 7 was accepted, aggregate_ir parent 1791430768, candidate 1772724857, ratio 0.9896. decode_ir ratio was 1.0019. Outputs were identical.
> I now believe the useful change is in non-streaming SSE aggregation. The earlier changes did not help enough. The accepted sandbox run kept guards within limits, with 1 files and 93 lines.
> Next try should separate the complete-frame path from the single data-field fast path, then measure aggregate_ir and decode_ir again. The submitted perf candidate is waiting for independent replays.
>
> 2026-10-13, episode `5e7b9ce8ef6b` (rejected)

> Changed src/free_claude_code/core/anthropic/stream_contracts.py in parse_sse_text. Lines 77-82 were replaced with 27 lines. The path handles one data line in one LF framed SSE event and falls back for comments, CRLF, multi-line data, and other forms.
> Evaluation 1 on perf decode_ir had outcome: accepted. Guard: ok, 1 files, 21 lines. Build: base ok, candidate ok. Equivalence: identical outputs. aggregate_ir was parent 1737275302, candidate 1738119749, ratio 1.0005. decode_ir was parent 1887384313, candidate 1834952244, ratio 0.9722.
> The generic SSE parser still covers the slower forms. The common single-event frame can skip regex line splitting and parse_sse_lines. The guard did not show a problem outside the target.
> Next try the same fast path around AnthropicSSEDecoder if replay confirms decode_ir. Check more inputs from parse_sse_text and parse_sse_lines before changing more parser code.
>
> 2026-10-13, episode `710c5e2617d0` (accepted)

> I first changed stream_contracts.py lines 76-81, then backed most of that out. I then changed decoder.py lines 27-32 and stream_contracts.py lines 68-72. The submitted perf candidate changed 2 files, 9 lines.
> evaluation 1 was rejected: decode_ir ratio 1.1857 vs required 0.9900. evaluation 3 was rejected: decode_ir ratio 0.9913 vs required 0.9900. evaluation 5 was accepted: decode_ir parent 1920631861, candidate 1897097543, ratio 0.9877. Outputs were identical.
> The large stream_contracts.py rewrite did not help. The smaller change did. It avoids running the SSE boundary regex for chunks with no line terminators. It uses fixed-prefix slicing for event and data fields.
> Next try the same path after independent replays. If aggregate_ir still matters, inspect sse_aggregation.py lines 1-155.
>
> 2026-10-13, episode `e1900872ce15` (accepted)

## Licence

Code excerpts from https://github.com/alishahryar1/free-claude-code at 74d4ccfde724f7416fa2a728d594d704dddc7c94, copyright its authors; the licence was not determined (see the repository).
