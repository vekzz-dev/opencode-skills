# Review Calibration

A finding must survive three questions. If any answer is no, drop it.

1. **What happens?** A concrete behavior: wrong output, crash, exposed data, or a specific change that now costs extra work.
2. **When?** A realistic input or a plausible near-term change. "Someone might later need…" is not when.
3. **Is the fix cheaper than the cost?** If the cleanup costs more than the problem it prevents, it is noise.

## Noise versus justified

| Requested change | Verdict | Reason |
|---|---|---|
| Extract a working 8-line function into three helpers | Noise | No behavior, defect, or change cost |
| Introduce an interface for its only implementation | Noise | Speculative second use; adds indirection now |
| Replace a two-branch `if` with a strategy pattern | Noise | The branch count is the design; the pattern adds files |
| Add a repository layer over one ORM call | Noise | Nothing to isolate, nothing to swap |
| Rename `data` to `payload` | Noise | Preference; no ambiguity reported |
| Split a coherent 80-line module by "concerns" | Noise (usually) | Separation without a cost is churn |
| `catch (e) { return null }` hiding a failure | Worth fixing | Callers cannot distinguish "empty" from "broken"; a real error disappears |
| Same validation rule in three handlers, already different | Worth fixing | Users get accepted in one path and rejected in another |
| Query inside a loop over user-sized input | Worth fixing | Cost grows with data; measurable at a few hundred rows |
| Shared mutable default or module-level state | Blocker | Values leak between calls; results depend on call order |
| SQL built by string concatenation with input | Blocker | Injection; full data exposure |
| Timestamps stored without timezone | Worth fixing | Ordering and reporting break across servers |
| Missing index on a column filtered on every request | Worth fixing | Full scan becomes the bottleneck as the table grows |
| Business rule duplicated in backend and frontend, already diverged | Worth fixing | One side is wrong today, and users see the mismatch |

## Phrasing: observation, not judgment

| Instead of | Write |
|---|---|
| "This is bad practice." | "When `items` is empty, `total / 0` throws and the request returns 500." |
| "You should use a repository pattern." | "The same query is built in four handlers; when the column changes, all four must change — one is already stale." |
| "SRP violation." | "This class charges the card and retries email; when the email call throws, the charge is already committed and the caller sees failure." |
| "Consider extracting this." | Omit unless the extraction prevents a named change cost. |

Name a principle only after the concrete consequence, and only once. The consequence teaches; the principle labels it.

## Worked example: good enough

```text
Verdict: Good enough for the current scope. Ship it.

Not reviewed
- The CLI parses args but I did not run it; argument handling looks consistent
  with the tests in cli_test.py.

Lesson
- The single `parse_config` function keeps validation and defaults together;
  that is fine while the format stays small. Revisit when a second format
  appears, not before.
```

No blockers, no padding. The lesson is optional and states when the decision would change.

## Worked example: one blocker, one cost

```text
Verdict: Needs work before it handles real uploads — one blocker, one cost
that will grow.

Blockers
- api/upload.py:42 — `filename` from the multipart form is joined directly
  onto `UPLOAD_DIR` — a request with `../../etc/passwd` writes outside the
  upload directory — join with `os.path.basename` and resolve, then verify
  the result stays under `UPLOAD_DIR`.

Worth fixing
- api/upload.py:57 — the size check runs after the file is fully written to
  disk — a 5 GB upload fills the disk before being rejected — check
  `content_length` and stream with a cap instead.

Not reviewed
- Concurrent uploads: I did not test two requests writing the same filename.
```

Two findings, each with a trigger and a minimal fix. The rewrite is a few lines, not a new module.

## Scope discipline

- Review the files the user pointed at, plus what is needed to judge them.
- Do not list pre-existing problems outside the requested scope; mention one only when it blocks the reviewed code.
- If the code is untested in a path that matters, say so under `Not reviewed` instead of demanding tests for everything.
- The review ends with the verdict. Do not append a roadmap, a maturity score, or a refactor plan.
