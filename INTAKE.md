# Task intake: from a user request to SCAM work

## 1. Purpose

The user should be able to describe a goal in ordinary language and add:

```text
Работай по SCAM.
```

The agent, not the user, turns that request into a bounded SCAM unit of work.
The user reviews a short preview of the agent's understanding, corrects it if
needed, and approves the start.

Intake is not implementation and is not a miniature project audit. Its job is to
identify the kind of work, the next independently acceptable outcome, its
boundaries, evidence and external authority.

## 2. When intake is required

Use intake before any non-trivial repository or external-state work:

- design or research that produces a project decision;
- diagnosis of a defect or incident;
- implementation, refactoring, migration or test work;
- review of code, evidence or a completed task;
- deploy, release, publish or another external operation;
- continuation of unfinished work in a new chat.

A direct answer, explanation, translation, status report or read-only lookup that
does not create a project artifact or change state does not need a Task Contract.
Answer it directly. Do not create process documentation merely because SCAM was
mentioned.

## 3. Bounded orientation

Before the preview, read only enough to ground the task:

1. project and SCAM entrypoints;
2. current Project Context;
3. an active Task Contract or Handoff, if one exists;
4. the directly affected contract, code and focused tests;
5. Git/workspace state needed to protect existing changes.

Do not install dependencies, rebuild the project, run the full test suite,
redesign the toolchain or read all historical documentation during intake.
Record an environment fact only when it materially affects the proposed owning
gate.

Separate every statement into one of these sources:

- explicit user requirement;
- verified project fact;
- accepted project decision;
- agent assumption or recommendation.

An assumption never becomes a requirement merely because it is convenient.

## 4. Classify the request

Choose exactly one active task type.

| Type | Use when | Result | Default mutation boundary |
| --- | --- | --- | --- |
| `DESIGN` | a material product or architecture choice is still open | decision/design document and implementation decomposition | code read-only |
| `DIAGNOSIS` | a symptom is known but its first cause is not proven | reproduction, localized cause and evidence | code read-only except a minimal diagnostic test if necessary |
| `IMPLEMENTATION` | the intended behavior and important decisions are known | tested change in code/config/docs | paths declared by the Task Contract |
| `REVIEW` | a concrete contract, diff and evidence must be evaluated | findings and `ACCEPT` or `CHANGES_REQUIRED` | read-only |
| `OPERATION` | the outcome changes an environment or external system | preflight, execution, post-check and recovery record | only the named target and authorized action |
| `CONTINUATION` | an existing unfinished task moves to another chat/agent | verified baseline and the next step of the same contract | inherited from the existing contract |

Classification rules:

- unknown desired design → `DESIGN`, not implementation;
- known desired behavior but unknown defect cause → `DIAGNOSIS` before a fix;
- known cause and accepted expected behavior → `IMPLEMENTATION`;
- completed implementation awaiting independent evaluation → `REVIEW`;
- deploy/publish/merge is `OPERATION`, even when implementation preceded it;
- an existing active Task Contract takes precedence over creating a duplicate
  `CONTINUATION` task.

If a request mixes several types, decompose it in dependency order:

```text
design → diagnosis (when needed) → implementation → review → operation
```

Create and activate only the first independently acceptable task. Put later work
in a short proposed decomposition, not into the current acceptance.

## 5. Determine whether the request must be split

Split before implementation when any of these is true:

- outcomes can be accepted or released independently;
- different outcomes require different owning gates;
- one part is read-only and another mutates code or an external system;
- a material choice must be made before implementation can be specified;
- deploy/publish/merge is bundled with building the change;
- review is bundled with fixing everything the review might discover;
- several services are mentioned but no single cross-service contract defines
  one observable result.

Do not split mechanical changes that are inseparable parts of one behavior, such
as contract → provider → consumer → integration test.

For a broad milestone, show:

1. the milestone outcome in one sentence;
2. the ordered task decomposition;
3. the one task proposed as active now.

Do not create one Task Contract containing the entire roadmap.

## 6. Draft the Task Contract

Reuse the project's existing task identifier and location conventions. If none
exist, use `TASK-<YYYYMMDD>-<short-slug>` and store the contract under the
project's agent-context tasks directory.

Fill `templates/TASK.md` yourself. The user must not be asked to complete the
template.

During intake the contract has status `DRAFT`. Only the Task Contract itself
may be created or edited before approval; product code, configuration and
external state remain unchanged.

Type-specific acceptance:

### DESIGN

- the decision to make and constraints are explicit;
- realistic options are compared;
- one recommendation is argued;
- affected public contracts, risks and open questions are named;
- implementation is decomposed into later tasks;
- no implementation is performed.

### DIAGNOSIS

- the symptom is reproduced or explicitly marked not reproducible;
- the first supported cause is localized with evidence;
- affected contract and blast radius are named;
- competing hypotheses are rejected by evidence;
- a minimal fix scope is proposed separately;
- the fix is not implemented unless a new implementation task is approved.

### IMPLEMENTATION

Acceptance normally covers:

- the main observable behavior;
- an error or boundary case;
- compatibility or a named intentional break;
- the smallest owning gate that proves the outcome;
- package/integration/acceptance evidence when a lower-level test is insufficient.

### REVIEW

The contract names the base, head/diff, originating Task Contract and evidence.
Findings use only `BLOCKER`, `FOLLOW_UP` or `REJECTED`. The result is
`ACCEPT` or `CHANGES_REQUIRED`; review does not silently repair the code.

### OPERATION

The contract names:

- exact target/environment and source artifact/commit/digest;
- preflight and stop conditions;
- authorized write action;
- success checks;
- recovery or rollback boundary;
- evidence to preserve.

Never infer authority for deploy, publish, merge, production write, credentials
or destructive cleanup from permission to edit code.

### CONTINUATION

Do not invent a new acceptance. Verify the existing Task Contract, Handoff,
branch/commit and worktree. If they differ, report the delta before continuing.

## 7. Show the SCAM Preview

Before implementation, show one compact preview:

```text
SCAM Preview
Type: <TYPE>
Task: <ID — name>
Outcome: <one observable result>
In scope: <short list>
Out of scope: <short list>
Acceptance / owning gate: <essential checks and command/level>
External actions: <allowed; approval required>
Assumptions or decisions: <only material items>
Split: <none, or ordered later tasks>
```

The preview summarizes the draft; it does not dump the whole template into chat.

Ask no more than three questions, and only when different answers would
materially change the result, public contract, safety or external authority.
For every question propose a recommended default and explain its consequence.
Do not ask about information that the repository can answer.

End with one explicit request:

```text
Подтверди превью или поправь конкретный пункт. После подтверждения acceptance
замораживается и я начинаю работу.
```

If the user already said that the stated acceptance is frozen and explicitly
authorized starting without another confirmation, create the contract as
`FROZEN`, show the preview as a notice and proceed.

## 8. Freeze and start

After user approval:

1. apply any requested corrections;
2. set Task Contract status to `FROZEN`;
3. record the approval reference succinctly;
4. begin the normal METHOD.md implementation cycle.

After the first product edit, the agent cannot change acceptance. A new finding
becomes:

- `BLOCKER` only when it violates the frozen contract or a governing invariant;
- `FOLLOW_UP` when useful but independently acceptable;
- `SPLIT` when the approved task cannot safely remain one unit.

Changing scope requires a new human decision, not a rewritten history.

## 9. Anti-patterns

Do not:

- make the user translate their request into the SCAM template;
- spend the task proving that the local Node/npm version differs before defining
  the product outcome;
- call setup, baseline or documentation the completed user outcome;
- turn a roadmap into one implementation task;
- add requirements discovered by the agent after acceptance was frozen;
- use a full repository audit as intake;
- ask the user to approve facts that can be read from canonical project sources;
- claim `PASS` for a command or external run that was not executed;
- begin implementation while a material design decision remains hidden in an
  assumption.

The quality test for intake is simple: after reading only the preview, the user
can say whether the agent is solving the right problem, at the right boundary,
with the right proof and authority.
