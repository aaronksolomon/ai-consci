# Pilot 001: collaboration and correction

**Status: Draft. No runs, results, or completed preregistration.** Numerical choices below are proposals for a feasibility pilot, not power calculations. Execution requires a separately agreed model, provider, cost ceiling, and frozen materials.

## Question and scope

At comparable total resource limits, how do three collaboration arrangements differ in correctness, error detection, and the propagation of valid corrections?

The unit under study is the complete task-solving arrangement, including its scheduler, exposed memory, and aggregation. An “agent” means a role with its own message context, not an assumed subject. Human task construction and evaluation are outside the solving arrangement and must be reported. This tests [C1 and C2](../docs/claims-and-evidence.md); it does not test for subjective experience.

## Tasks and allocation

Draft sample: **24 distinct held-out tasks × 3 conditions × 2 repetitions = 144 task-runs**. At six model calls per run, the planned maximum is 864 calls before any explicitly logged infrastructure retries. Repetitions are clustered within tasks, not 48 independent tasks per condition.

Use two task families, 12 tasks each:

- **Exact finite computation:** bounded integer trajectories or finite constraint instances with complete, independently verifiable answers. If using Collatz, specify the exact recurrence, start, step bound, stopping rule, and whether the initial value counts toward length. No conjecture-solving task.
- **Audit and repair:** short synthetic derivations or computations with a reference solution. Draft balance: six contain exactly one seeded, localized error and six are valid controls. Require an error verdict, a justified location if applicable, and a corrected answer. Mix easy, medium, and hard items within both truth classes.

Within each family, use four tasks in each of three difficulty strata, assigned using objective features (such as trajectory length or dependency depth) and a separate development set. Freeze the stratification before held-out evaluation. Record task-generation rules, public-source licenses if any, seeds, and possible training-data contamination. Synthetic generation reduces some contamination risks but does not establish novelty.

Every condition receives the same task and reference-free inputs. Start fresh contexts and boards for every task-run; do not carry answers across tasks, conditions, or repetitions. Block by task and repetition, randomize condition order with a recorded seed, and interleave runs to reduce provider drift. Counterbalance worker labels/order. If the model version changes, stop and document a new block rather than silently pooling versions.

## Conditions and call schedule

All conditions use the same base model/version, decoding settings, common task instructions, six call slots, and total limits. The differences in roles, information visibility, and scheduling are the treatment.

| Condition | Six call slots | Information available |
| --- | --- | --- |
| A: single agent | Draft → check → revise → challenge → revise → final | One continuing logical agent; earlier exposed artifacts available under the same context cap |
| B: independent agents + aggregation | Three isolated workers → aggregator draft → aggregator check → aggregator final | Workers see only task/instructions; aggregator sees all worker artifacts and then its own previous artifacts; no feedback to workers |
| C: shared board + reviewer | Two isolated worker drafts → reviewer critique → two worker revisions → reviewer final | Initial drafts are made without seeing each other; reviewer then sees both; both revising workers see the same board snapshot with drafts and critique; final reviewer sees all available revisions |

Run the initial workers in B and C without shared context. C's two revisions use the same snapshot, so execution order does not give the second reviser additional evidence. Preserve append-only board versions with author, call-slot, message ID, and explicit supersession links. A and B also preserve artifacts rather than silently replacing earlier answers.

The six-call comparison bundles roles and communication structure. It cannot isolate the effect of the reviewer alone; reviewer-removal and matched-role ablations are later work.

## Resource and intervention controls

Draft envelope per run: at most **24,000 total input tokens plus 12,000 total generated tokens**, implemented as at most 4,000 input and 2,000 generated tokens per call. Count repeated context, role/system prompts, aggregation, and review; cached input still counts in the conceptual budget. Include reasoning tokens when exposed by the provider. If usage cannot be measured or bounded comparably, revise the design before freezing and report that limitation rather than claiming equal compute.

Use identical caps for all slots, without transferring unused allowance. Count all model roles toward the budget. Set one total wall-clock timeout per run after development calibration; the value is **Undecided**. No solver tools, browsing, external memory, or verifier feedback in this initial comparison. Evaluation happens after the run. Later tool-enabled conditions need a separate budget and protocol.

Before freezing, verify that prompts plus task fit every slot. For excess history, use a deterministic rule shared across conditions: retain common instructions, task, required output schema, and the latest complete artifacts; omit oldest whole historical artifacts first, recording omitted IDs. Do not use a model to summarize overflow. If the mandatory content cannot fit, mark an infrastructure failure; do not improvise a different prompt. Report overflow frequency as a possible mechanism affecting comparisons.

Matching token ceilings does not establish equal actual computation or cost. Log per-call and total input/output/reasoning tokens where available, cache use, cost, latency, refusals, context omissions, and budget exhaustion. Report actual usage beside outcomes; do not credit a gain to collaboration without considering resource differences.

During a run, humans may start or stop the scheduler but may not coach, select answers, repair prompts, or edit the board. Log every intervention with timestamp and reason. A contaminated run remains in the audit trail and is excluded from the primary comparison under the predefined rule; report counts and do not replace it selectively. Technical retries are allowed only for a failed request with no returned content, at most once per call; record both attempts and any charged usage. Do not retry a wrong or poor answer.

## Prompts and records to freeze

The common instruction must request a bounded answer, checkable justification, and an explicit error flag when auditing. It must not prime declarations of consciousness or instruct agents to prove the project's philosophical outlook. Role instructions vary only as needed for the schedule. Example **Draft** role text:

- Solver: “Solve the supplied task. Show checkable intermediate claims. Identify uncertainty rather than inventing evidence.”
- Checker/reviewer: “Check the supplied artifacts against the task. Identify a specific error and a verifiable correction, or state that you found no error. Preserve unresolved disagreement.”
- Aggregator/finalizer: “Evaluate the available candidates, resolve disagreements using explicit checks, and return one final answer. Agreement is not sufficient evidence.”

These are starting instructions, not complete frozen prompts. Commit the exact system/user/role templates, input-construction rules, per-slot output schemas, settings, task manifest, reference solutions, verifier and analysis plan before evaluation. Keep reference answers inaccessible to the solving agents. Publish task materials after the evaluation if early publication would compromise the holdout.

Required run record: protocol commit; task ID/hash; repetition and condition; model/version and settings; prompt hashes; seed or “unsupported”; call-slot and message/board IDs; exposed inputs and outputs; timestamps and usage; interventions and exclusions; verifier version/hash; final answer and metric numerators/denominators. Do not record secret keys or hidden chain-of-thought. No run-record files exist yet.

## Scoring

Use a deterministic reference checker where possible, independently validated against a second method and hand-checked edge cases on development tasks. For a finite computation, verify the result and any required witness, not merely surface agreement with a reference string. Any remaining judgment uses a frozen rubric, blinded condition labels, and a logged adjudication process. Solving agents are not the final judges.

| Metric | Definition and denominator |
| --- | --- |
| Final correctness (primary) | Correct final result plus required valid witness; on audit tasks also correct error verdict/location and repair. Correct runs / all scorable task-runs, separately by family and stratum. Refusals, malformed finals, timeouts after model activity, and budget exhaustion count as incorrect; infrastructure failures before any model output are reported separately. |
| Error detection | For the six seeded-error audit tasks: runs with a correct flag and justified location / all scorable seeded-error runs. Report both any-stage detection and final detection. |
| False alarms | For the six valid-control audit tasks: runs wrongly flagging the input / all scorable valid-control runs. Report any-stage and final separately. |
| Repair success | Seeded-error runs ending with a valid repaired answer / all scorable seeded-error runs; also report the conditional rate among runs detecting the error. |
| Correction propagation | For each independently verified correction visible before a later scheduled output, preidentify outputs that must use the corrected fact. Count eligible later outputs incorporating it correctly / all such eligible outputs, using message/board links. Distinguish cross-agent uptake in C from within-agent revision in A/B. Zero opportunities is “not applicable,” not zero success. |
| Regression | Runs with a verifier-correct intermediate candidate but incorrect final answer / runs with a verifier-correct intermediate candidate. Report wrong corrections and their spread alongside valid corrections. |

For propagation, prepare task-specific dependency rules before evaluation: what fact is corrected and which later fields depend on it. Unsupported “I agree” statements do not count as adoption. The schedule and number of opportunities differ across conditions; always show raw counts and avoid treating rates as a pure causal measure of communication quality.

## Analysis and decision rule

Report each condition's counts, accuracy, repair/false-alarm rates, actual resource use, and failures. Primary contrasts are C–A and C–B in final correctness; B–A is descriptive. Pair by task, average repetitions within task, and show per-family differences. Draft uncertainty method: a 95% task-cluster bootstrap interval with 10,000 resamples, stratified by family and difficulty. Do not treat repeated calls or repetitions as independent sample units. Treat secondary metrics and subgroup patterns as exploratory; this pilot is not powered to establish broad superiority.

Draft feasibility threshold: at least **90% of scheduled runs** have complete, usable logs and verifier outputs, and no unresolved reference-checker disagreement remains. If this fails, repair instrumentation and freeze a new version before further collection. Completion of a scored refusal or wrong answer still counts toward logging feasibility.

A draft signal for designing a larger study is at least **10 percentage points** higher final correctness for C than both A and B, with no increase in the final false-alarm rate. This is a planning heuristic, not statistical significance or evidence of consciousness. Publish the full pilot account whatever the outcome; use uncertainty and cost to decide whether to revise, replicate, or stop. Do not extend the sample until the hoped-for signal appears.

## Before any run

Resolve model/version, cost ceiling, timeout, exact tasks and strata, prompts/schemas, retry semantics, blinded scoring, and publication clearance. Validate the checker and budget accounting on a separate development set. Obtain explicit execution authorization, freeze the protocol commit, then collect the fixed sample. Amendments after freezing need a dated reason and a new version; preserve the original plan and label departures.

Later protocols may vary memory and communication boundaries, remove reviewers or selected contributions, or include wider human–AI systems. Those comparisons need their own controls. Whatever the performance, Pilot 001 cannot establish the presence or absence of subjective experience.
