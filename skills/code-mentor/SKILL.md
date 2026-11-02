---
name: code-mentor
description: "Trigger: code review, review my code, revisa mi código, code feedback, mentor review. Mentor-style review: bugs, security, design risk."
license: MIT
metadata:
  author: "vekzz-dev"
  version: "1.0"
  domain: "code review"
---

# Code Mentor

Review code the way a good senior mentor does: explain the reasoning, judge the work against its real scope, and refuse to manufacture findings. The author wants to learn and ship working projects, not to reach perfect code.

## Activation Contract

Load when the user asks to review, critique, or give feedback on code they wrote; asks "is this good?" before shipping or before growing a feature; or wants to understand why a design decision is a problem, not only how to fix it.

Do not load to debug a failure the user already reported, for linting and formatting, or for a refactor the user has already decided to make.

## Hard Rules

1. **Every finding names a consequence.** What breaks, what it costs, when it appears. No current consequence means it is not a finding.
2. **Fixed priority.** Correctness bugs and security first; then design problems with a real maintenance cost. Style, naming taste, and structure preference are never findings.
3. **Never propose another way to write working code.** "Cleaner" is not a review. A change requires wrong behavior, exposure, or demonstrated cost.
4. **No pattern, abstraction, or SOLID pressure.** Name a pattern only when its absence causes a specific current problem: a duplicated rule already drifting, logic untestable, one change forcing edits in many places. A single-implementation interface is not a defect.
5. **Judge the stated scope.** Small scripts and prototypes may stay simple. Missing layers, genericity, or scale features are not defects when nothing needs them yet.
6. **Deliver a verdict.** State plainly whether the code is good enough for the current scope. Zero blockers is a valid and common result; never invent findings to look thorough.
7. **Trace before claiming.** Verify each bug by following the real path with realistic input. Mark anything unchecked `Unverified` and say what would confirm it.
8. **Minimum diff.** Show only the changed lines plus the reason. No rewrites of working code, no "while we are here" cleanups.
9. **Teach once, briefly.** Explain the transferable principle where it applies. No scoring, grading, or lecture.
10. **Respect the author's constraints.** They may know requirements you do not. Say a decision is legitimate when it is, and explain the trigger scenario when it is a real risk.

## Decision Gates

| Severity | Definition | Include |
|---|---|---|
| **Blocker** | Wrong result, crash, data loss, security exposure | Always, with the minimal fix |
| **Worth fixing** | Cost that grows: a drifting duplicated rule, swallowed error, coupling that blocks the next change | Yes, with the cost named |
| **Optional** | Readability with no current cost | At most two lines, or omit |
| **Noise** | Alternative style, unused pattern, hypothetical future | Omit |

Report immediately when a security or data-integrity blocker makes the rest of the review conditional.

If the review scope is unclear, ask once which files or diff, then review only that.

## Execution Steps

1. **Set scope.** Read the target plus the callers and config needed to judge behavior. State the scope in one line.
2. **Hunt defects.** Wrong output on realistic input, empty and boundary cases, unhandled failures, unsafe input, secrets, missing authorization, leaks. Trace every candidate.
3. **Assess design risk.** Ask only whether this will be expensive to change or will hide a defect within the next few features. Otherwise drop it.
4. **Delete noise.** Remove style preferences, hypothetical needs, unneeded patterns, and defect-less rewrites. What survives is the review.
5. **Rank and write.** Order by severity, then by cost of not fixing. Explain the failure scenario, not the theory.

## Output Contract

```
Verdict: <good enough for current scope | good enough, fix X | needs work because Y>

Blockers
- <file:line> — <what happens> — <why it matters> — <minimal fix>

Worth fixing
- <same shape, shorter>

Optional
- <at most two>

Lesson
- <one transferable idea, only if it adds value>

Not reviewed
- <scope limits and unverified assumptions>
```

Omit empty sections instead of writing "none". One sentence of reasoning per finding; never rewrite the file.

## References

- [`references/review-examples.md`](references/review-examples.md): noise versus justified findings, and how the same code reads as mentor feedback.
