# Lessons: free-claude-code

Lineage `604b02c1526246078c0978552b047195491b9acc50c67042a6df51b76f4a0e41` on [https://github.com/alishahryar1/free-claude-code](https://github.com/alishahryar1/free-claude-code) at `74d4ccfde724f7416fa2a728d594d704dddc7c94`. 21 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target perf:aggregate_ir

- Attempts: 10 (3 accepted, 1 rejected, 6 no candidate)
- Best accepted effect: 3.76% (ratio 0.9624), episode `9b232f5e2458`
- Accepted effects: 3.76%, 1.19%, 1.03%
- Why candidates failed: no_improvement (1)
- Files changed by accepted candidates: `src/free_claude_code/core/anthropic/sse_aggregation.py`
- Files changed by rejected candidates: `src/free_claude_code/core/anthropic/sse_aggregation.py`
- Median model spend per attempt: 0.6531 USD (over 10 attempts with a recorded cost)

## Target perf:decode_ir

- Attempts: 11 (5 accepted, 6 no candidate)
- Best accepted effect: 13.58% (ratio 0.8642), episode `323e221229a9`
- Accepted effects: 13.58%, 2.49%, 1.64%, 1.23%, 1.13%
- Files changed by accepted candidates: `src/free_claude_code/core/anthropic/stream_contracts.py`, `src/free_claude_code/core/anthropic/streaming/decoder.py`
- Median model spend per attempt: 0.6940 USD (over 11 attempts with a recorded cost)

## In the agent's own words

The agent's journal entries on this lineage, newest first, verbatim. They are its own notes: claimed, not checked.

> Changed src/free_claude_code/core/anthropic/stream_contracts.py and src/free_claude_code/core/anthropic/streaming/decoder.py. Changed feed_frames. Tried a reusable JSONDecoder.raw_decode path for the common single-data SSE fast path. Tried skipping append and join when the frame is already contiguous.
> evaluation 1 was rejected: no_improvement, worst ratio 0.9909 vs required 0.9900. evaluation 3 was rejected: no_improvement, worst ratio 0.9911 vs required 0.9900. evaluation 5 was accepted. It had identical outputs. decode_ir was parent 1549111488, candidate 1533274892, ratio 0.9898. aggregate_ir was ratio 0.9997.
> The earlier edits did not help enough. The final candidate kept the generic parser and malformed-data fallbacks unchanged. The useful path is the common single-data SSE case and the contiguous-frame case in feed_frames.
> Submitted the perf candidate on decode_ir with 31 changed lines. It was committed and revealed with status queued. Next is to wait for independent replays.
>
> 2026-10-13, episode `bc6da725ebd8` (accepted)

> Tried changes in src/free_claude_code/core/anthropic/sse_aggregation.py for aggregate_anthropic_sse_to_message. Read stream_contracts.py, lineage_bench.py, streaming/decoder.py, and lineage_equiv.py. Reworked small block assembly, tried a larger helper path, then reverted most of it.
> evaluation 1 was "rejected (no_improvement: worst ratio 0.9990 vs required 0.9900)", with "equivalence: identical outputs". evaluation 3 was "rejected ... 0.9993 vs required 0.9900". evaluation 5 was "rejected ... 1.0115 vs required 0.9900". evaluation 7 was "rejected ... 0.9999 vs required 0.9900". build was "base ok, candidate ok" each time.
> The code is not spending enough in the edited branches for these shape changes to matter. The larger rewrite hurt aggregate_ir. The small rewrites did not help enough.
> Try a narrower change from lineage_bench.py. No candidate was submitted.
>
> 2026-10-13, episode `85641c985284` (no candidate)

> I changed src/free_claude_code/core/anthropic/sse_aggregation.py in _sse_payload and aggregate_anthropic_sse_to_message. I tried a module-level JSONDecoder and raw_decode for the SSE single-data fast path. The stripped json.loads path stays as fallback.
> evaluation 1 was rejected: aggregate_ir parent 1574925205, candidate 1576000014, ratio 1.0007. evaluation 3 was rejected: aggregate_ir parent 1574926907, candidate 1577424316, ratio 1.0016. Those changes did not help. evaluation 5 was accepted in sandbox: aggregate_ir parent 1574937344, candidate 1553729831, ratio 0.9865. Equivalence identical outputs. guard: ok, 1 files, 20 lines.
> I now believe avoiding strip and json.loads on the common frame is the useful part. Larger control-flow changes add work here.
> Next try the same raw_decode pattern only after checking stream_contracts.py. Leave streaming/decoder.py unchanged unless decode_ir is the target.
>
> 2026-10-13, episode `201643ef8faf` (accepted)

> I changed parse_sse_text, feed_frames, and AnthropicSSEDecoder feed code in decoder.py and stream_contracts.py. The last version parsed frames directly in feed and avoided some intermediate allocations.
> evaluation 7 was rejected with no_improvement: worst ratio 0.9972 vs required 0.9900. decode_ir was parent 1549727003, candidate 1545373932, ratio 0.9972. aggregate_ir was parent 1592341369, candidate 1595549034, ratio 1.0020. Build passed and equivalence was identical outputs.
> The direct decode-path change preserved output but did not clear the threshold. Earlier versions also did not help enough: ratios 1.0003, 0.9998, and 0.9990.
> Next try less work in stream_contracts.py before decode, or a different hotspot from lineage_bench.py. No candidate was submitted.
>
> 2026-10-13, episode `8d0bd54b225b` (no candidate)

> I changed src/free_claude_code/core/anthropic/streaming/decoder.py and src/free_claude_code/core/anthropic/stream_contracts.py. I tried targeted reductions in AnthropicSSEDecoder, parse_sse_text, and feed_frames. The changes touched decoder framing, SSE parsing, imports, and aliasing.
> evaluation 1 was rejected: no_improvement, worst ratio 0.9986 vs required 0.9900. decode_ir was parent 1548400607, candidate 1546158107, ratio 0.9986. evaluation 3 was rejected at 0.9983. evaluation 5 was rejected at 0.9985. evaluation 7 was rejected because 2 stable tests fail. No candidate was submitted.
> The code has little slack in this path. Small framing and parsing edits moved decode_ir, but not enough. The aliasing tweak was wrong for unnamed events at split boundaries.
> Next try a narrower change in decoder.py only. Keep tests/core/anthropic/test_sse_decoder.py behavior fixed before measuring parse_sse_text or feed_frames again.
>
> 2026-10-13, episode `e048b98447e1` (no candidate)

## Licence

Code excerpts from https://github.com/alishahryar1/free-claude-code at 74d4ccfde724f7416fa2a728d594d704dddc7c94, copyright its authors; the licence was not determined (see the repository).
