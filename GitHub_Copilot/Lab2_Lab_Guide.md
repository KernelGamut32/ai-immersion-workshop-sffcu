# Lighthouse Member Services Lab Guide

*Lab 2  GitHub Copilot and Advanced Security*

SchoolsFirst Federal Credit Union workshop. Facilitator Allen Sanders. Duration 90 minutes. All people, accounts, balances, loans, and credentials in this package are fictional.

You will explain an unfamiliar service, create tests from independent business rules, correct its defects, and refactor it with an agent. You will then assess a cloud-agent pull request and inspect how security findings become reviewed fixes. The goal is working code you can explain and evidence you can defend.

### Two ways to participate

- Coders work in a local repository in desktop VS Code. Complete Quick_Start.md before class and keep the guide and Prompt_Cards.md beside the editor.

- Observers follow the timed track in this guide and the separate Observer_Prompt_Pack. Pair with a coder and make the same evidence and merge decisions.

### What you will leave with

- A documented module and a README explanation linked to the business rules.

- A test suite with meaningful financial boundaries and a corrected service.

- A reviewed refactor and a pull request, where repository access permits it.

- A cloud-agent issue and PR review record, or an explicitly identified worked-example review if cloud execution is unavailable.

### Start with the right evidence

The starter has deliberate defects. Its passing smoke test establishes that the app starts and responds at /health. It does not verify the database or payoff calculations. docs/payoff-rules.md defines the fictional product rules. The supplied implementation is something to test against those rules.

This local edition uses .NET 10 LTS, SDK 10.0.403 and runtime 10.0.14. Product guidance was checked November 1, 2026; local setup was reviewed November 3. Confirm the SDK with dotnet --version from the repository root. [G8, G12]


## The Session and Your Checkpoints

*14:15 to 15:45 local Pacific time*

| Time | Min | Activity | Evidence |
| --- | --- | --- | --- |
| 14:15 to 14:22 | 7 | Launch and cloud-agent issue | Account, repository, issue, and baseline |
| 14:22 to 14:37 | 15 | Explain and document | Evidence-backed explanation and documentation diff |
| 14:37 to 14:55 | 18 | Generate and run tests | Meaningful red cases, corrected implementation, green suite |
| 14:55 to 15:13 | 18 | Refactor with an agent | Passing regression suite and reviewed diff |
| 15:13 to 15:20 | 7 | Cloud-agent reveal and review | Issue acceptance evidence and PR decision |
| 15:20 to 15:40 | 20 | Advanced Security demonstration | Source trace, proposed fix, dependency and secret controls |
| 15:40 to 15:45 | 5 | Review and transfer | Saved work and a Day 2 pilot idea |

### Launch at minute two

The facilitator delegates the prepared input-validation issue at approximately 14:17 through GitHub. This remote task runs independently of your local VS Code session. Coders may delegate their own seeded issue when enabled. Record the actual issue and PR links; the issue number is not guaranteed to be 1. Completion time can vary. [G3]

### Keep moving

After one minute without progress, check the named source, current session, and permission context. At two minutes, ask for help and take the documented fallback. Exercise 1 ends at 14:37. Preserve at least 12 minutes for the refactor and 15 minutes for the security demonstration.

If a cloud PR or security scan is still running, inspect its real status and then review the prepared example. Record the route used. A local worked solution is not an observed cloud-agent result.


## Open the Repository and Establish a Baseline

*Launch 7 minutes*

1. Complete Quick_Start.md on this device before class. In desktop VS Code, choose File > Open Folder and select your local clone containing Lighthouse.sln. Confirm the assigned repository with git remote -v. [G8]

1. Confirm that desktop VS Code is signed into your approved GitHub account with Copilot access. Select a local workspace session and the model allowed by the facilitator. Use the session and agent controls actually shown. [G1, G2]

1. Open the VS Code terminal at the repository root. These commands work in Windows PowerShell and the macOS terminal.

```text
git status --short
dotnet --version
dotnet restore Lighthouse.sln --locked-mode
dotnet test Lighthouse.sln --no-restore
```

Read the command result and the number of tests actually executed. The starter payoff test file is intentionally empty. A green smoke test is a readiness checkpoint, not proof of correct payoff logic.

### Files to open

| Path | Why it matters |
| --- | --- |
| src/Lighthouse.Api/Services/LoanPayoffService.cs | Long method and calculation defects |
| src/Lighthouse.Api/Services/MemberService.cs | Duplicated validation and lookup query |
| docs/payoff-rules.md | Authoritative fictional product rules |
| tests/Lighthouse.Tests/LoanPayoffServiceTests.cs | Your test working area |
| .github/copilot-instructions.md | Repository instructions |
| CONTRIBUTING.md | Team conventions and review rules |

Keep the API bound to http://127.0.0.1:5080. Quick_Start.md provides the direct launch and sample-data checks. Use only the supplied fictional data.


## Choose Context and Control

*Use the labels present in your rehearsed environment*

| Work pattern | Useful task | What you inspect |
| --- | --- | --- |
| Completion | A small suggestion where you are typing | The proposed code before acceptance |
| Ask or read-only chat | Explain a symbol or compare code with rules | Sources, assumptions, and missing context |
| Bounded editing | Generate tests in selected files | Working set and each proposed change |
| Agent session | Refactor across files and run tools | Plan, permissions, commands, diff, and tests |
| Copilot cloud agent | Delegate an issue in a remote workflow | Issue scope, session progress, commits, PR, and checks |

Use Add Context, file selection, or the available # mention picker to reference the actual file or symbol. A literal #file: string is not a promise that your client has attached a source. Confirm that the attachment or context chip is visible. [G14]

### Four prompt ingredients

State the goal, the exact context, the constraints, and the evidence you expect back. Example: write boundary tests from the business rules, in the named test file, without changing production code, then report the actual results.

### Permissions

Tool approval depends on session permissions and organizational settings. You may not see a separate prompt for every build or test. Inspect commands and scope when prompted, and use the facilitator-approved permission level. A command finishing successfully does not establish that its output is correct. [G15]


## Exercise 1 Explain the Payoff Service

*14:22 to 14:30*

1. Open LoanPayoffService.cs and select CalculatePayoff. Attach docs/payoff-rules.md and the model definitions. Use a read-only question first.

```text
Explain CalculatePayoff in src/Lighthouse.Api/Services/LoanPayoffService.cs in plain language for a product owner. Read its model types and docs/payoff-rules.md first. Trace the inputs, validation, interest, grace-period fee, and final rounding. Distinguish what the code currently does from what the rules require. Cite the file and relevant symbol or rule. Do not edit files.
```

### A useful answer should identify

- Who supplies the loan, member, and payoff date, and what validation can reject.

- How the service calculates interest, determines the fee, and rounds the final result.

- Where current behavior disagrees with the rules, with file and symbol references.

### Follow the rounding

```text
Locate the final decimal.Round call in LoanPayoffService.CalculatePayoff. Compare ToEven and AwayFromZero using the rule's exact half-cent example. Calculate the unrounded interest and total with decimal arithmetic, then show both rounded outcomes. Explain which behavior the business rule requires. Do not change code yet.
```

Do not depend on a fixed line number from the plan. Find the actual decimal.Round call. The exact half-cent input gives you a concrete way to check the explanation.

Observer checkpoint: explain one rule in ordinary language and identify one assumption you would challenge. Coder checkpoint: open the cited code and confirm the answer describes what is actually present.


## Exercise 1 Document Without Hiding Defects

*14:30 to 14:37*

```text
Add XML documentation to the public members of LoanPayoffService using the actual implementation and model definitions. Keep each summary to one sentence. Document inputs, output, and exceptions actually thrown. Do not describe the known grace or rounding defects as approved business rules. Draft a README section named Loan payoff rules that links to docs/payoff-rules.md and distinguishes intended rules from current discrepancies. Change documentation only, then show the diff.
```

### Review the edit

1. Inspect the diff. Only documentation should change in this step.

1. Check that exception documentation names exceptions the method can actually throw and describes the triggering condition.

1. Check that the README links to the business rules. Avoid a description that makes the 10-day constant sound approved.

### Context challenge

Open .github/copilot-instructions.md. Identify the currency, naming, and review constraints. For an optional comparison, temporarily change only an explanation-style instruction, ask the same question, then restore it. Do not remove decimal, security, or test requirements. A different answer is possible, not guaranteed. [G2]

```text
Compare the implementation, docs/payoff-rules.md, and the proposed tests. List each disagreement, the authoritative rule, a concrete input that exposes it, and the expected output. Do not resolve a disagreement by changing the rules to match the code.
```

### Before moving on

Save the documentation changes. Ask a partner to name one claim that was supported by code and one that came from the business rules. The distinction matters when you generate tests in the next exercise.

Checkpoint: XML comments and a README section, with no unintended behavior change. If Copilot cannot edit, retain the reviewed draft and apply it manually or use the worked example during recovery.


## Exercise 2 Establish the Test Oracle

*14:37 to 14:41*

A test oracle is the independently justified result used to judge the code. Use the fictional product rules, not an output copied from the defective implementation. All dates below are in 2026. APR is a decimal fraction: 0.073 means 7.3 percent.

| Case | Principal | APR | As of | Due | Payoff | Expected total |
| --- | --- | --- | --- | --- | --- | --- |
| Standard interest | 1000.00 | 0.073 | Oct 1 | Nov 1 | Oct 11 | 1002.00 |
| Due date | 1000.00 | 0 | Oct 1 | Oct 10 | Oct 10 | 1000.00 |
| Day 10 | 1000.00 | 0 | Oct 1 | Oct 10 | Oct 20 | 1000.00 |
| Day 15 inclusive | 1000.00 | 0 | Oct 1 | Oct 10 | Oct 25 | 1000.00 |
| Day 16 | 1000.00 | 0 | Oct 1 | Oct 10 | Oct 26 | 1025.00 |
| Exact half cent | 100.00 | 0.01825 | Oct 1 | Oct 31 | Oct 2 | 100.01 |
| Zero principal | 0.00 | 0.073 | Oct 1 | Oct 10 | Nov 5 | 0.00 |

### The arithmetic behind the cases

Interest = principal x annual rate x elapsed days / 365. Use decimal arithmetic and retain full intermediate precision. For the standard case, 1000 x 0.073 x 10 / 365 = 2.00. The payoff is 1002.00.

The grace period includes day 15 after the due date. A positive-balance loan first incurs the fixed $25 fee on day 16. A zero-principal loan returns zero, including no fee. Round the final total once to two decimals using AwayFromZero.

For the midpoint case, 100 x 0.01825 x 1 / 365 = 0.005. The unrounded total is 100.005. The approved result is 100.01. ToEven would give 100.00. This is a separate issue from the grace boundary.

These rules are fictional training requirements. They do not describe actual SchoolsFirst loan products or financial calculations.


## Exercise 2 Generate Tests Before Fixing Code

*14:41 to 14:48*

1. Choose bounded editing or a constrained agent session. Attach LoanPayoffService.cs, its models, docs/payoff-rules.md, and LoanPayoffServiceTests.cs.

```text
Read docs/payoff-rules.md, the payoff models, and LoanPayoffService.cs. Write xUnit tests in tests/Lighthouse.Tests/LoanPayoffServiceTests.cs using the documented rules as the oracle. Use decimal literals and fixed DateOnly values. Cover the standard payoff, due date, day 10, day 15, day 16, zero principal, and the exact half-cent example. Use a Theory for the grace boundaries and names Method_Scenario_ExpectedResult. Assert exact monetary amounts, not just greater-than-zero. Do not change production code or weaken assertions to match the current implementation. Run the tests and report the actual result, count, and any failing case.
```

```text
dotnet test --filter FullyQualifiedName~LoanPayoffServiceTests
```

### Check the test quality

- Assertions use exact expected amounts and decimal literals such as 1000m.

- The grace Theory checks the final allowed day and the first fee day. Day 10 checks the stale constant too.

- The midpoint test is a true half-cent case with full intermediate precision.

- The tests use fixed dates, not the system clock, network, or real member records.

### If everything passes immediately

Inspect whether Copilot changed production code despite the instruction, copied current outputs into expected values, skipped boundary cases, or ran no matching tests. Passing tests are useful evidence only when they actually exercise the documented requirement. A correct implementation may also pass; inspect the diff before assuming the seeded defect remains.

Checkpoint: the failing case and its actual expected-versus-actual output are visible, or you can explain from the diff why the case already passes.


## Exercise 2 Correct the Three Behaviors

*14:48 to 14:55*

```text
Use the failing tests and docs/payoff-rules.md to make the smallest behavior corrections in LoanPayoffService. Check the grace length, inclusive last day, zero-balance treatment, and final rounding policy separately. Preserve decimal arithmetic and public signatures. Do not change expected test values or delete tests. Run dotnet test, explain each intentional behavior change, and show the exact diff.
```

| Check | Intended behavior | Why a partial fix fails |
| --- | --- | --- |
| Grace length | 15 days | Changing only the comparison leaves the stale 10-day constant. |
| Grace boundary | Fee only when days overdue > 15 | Changing only 10 to 15 still charges on day 15 if >= remains. |
| Rounding | Final total once with AwayFromZero | A green grace test does not exercise a half-cent result. |

```text
dotnet test
git diff --check
git diff -- src/Lighthouse.Api/Services/LoanPayoffService.cs tests/Lighthouse.Tests/LoanPayoffServiceTests.cs
```

### Preserve evidence

Keep the tests that exposed the defects. Do not turn them into assertions of the original behavior. Check the total test count and the result before committing the intentional fixes separately from the structural refactor.

If time is short, ask the facilitator for the tested recovery patch or compare with the worked solution. Save your current diff before applying anything. Use git apply --check first. A patch that does not apply cleanly is a signal to compare the intended change, not to force it over your work.

Checkpoint: the complete relevant suite passes, and you can explain the three deliberate changes. Record any case still failing before moving on.


## Exercise 3 Prepare a Controlled Refactor

*14:55 to 14:59*

The refactor begins from a service whose documented behavior already passes the tests. Bug correction and structural improvement are separate review decisions. Keep them separable in the history.

```text
git status --short
dotnet test
git switch -c refactor/payoff-service
```

If the branch already exists from an earlier attempt, inspect it and switch to it rather than creating a conflicting history. Save or commit your intentional documentation, tests, and bug fixes before starting the next edit. Use the facilitator's branch convention if one is supplied.

### Review the intended scope

- Extract calculation responsibilities from the long CalculatePayoff method.

- Share the duplicated member validation while preserving exception behavior.

- Read the SQLite connection from ConnectionStrings:Lighthouse without changing the data source or read-only mode.

- Preserve public signatures, the corrected rules, test assertions, and loopback-only operation.

### Keep the security demonstration separate

The member lookup has a deliberate unsafe SQL construction for the later security demonstration. This exercise focuses on the named refactor. If the agent discovers that flaw, have it report the issue without silently expanding the task. The facilitator uses a separate security copy so the demonstration remains reproducible.

Observer checkpoint: identify one proposed change that is structural and one that would change behavior. Ask which test would detect the difference.


## Exercise 3 Delegate and Inspect the Work

*14:59 to 15:08*

```text
First read .github/copilot-instructions.md, CONTRIBUTING.md, docs/payoff-rules.md, and the passing payoff tests. Propose a short plan, then refactor LoanPayoffService.CalculatePayoff to no more than 40 nonblank, noncomment lines by extracting validation, interest, fee, and rounding into clearly named helpers. Extract member validation shared with MemberService into one validator. Move the hard-coded SQLite connection string to the existing IConfiguration path, keeping the same database and loopback behavior. Preserve public signatures, decimal types, the corrected 15-day inclusive grace period, and final AwayFromZero rounding. Run the tests after each coherent change. Do not delete tests, change expected values, introduce new packages, fix unrelated security-demo flaws, push, or merge. Stop and explain if a behavior change is necessary. Finish with the changed files, commands run, results, and residual risks.
```

### Follow the actual execution

Read the plan and the proposed commands. If a permission prompt appears, verify the command and working directory. Terminal approvals vary with the session policy. Use the facilitator-approved permissions. [G15]

Optional prompt-file challenge: in a Local session, run /refactor-safely from .github/prompts. If that route is unavailable, paste the reviewed file text. Local prompt files do not run in the Copilot harness. [G16]

An agent may complete the refactor without a failing intermediate test, or it may need a correction. Do not manufacture a failure. If a test fails, examine the output and whether the agent fixes production code or weakens the test. Preserve the test oracle.

### End condition

The desired result is a short public method with understandable helpers, shared validation, configuration-based connection handling, and a green regression suite. A smaller line count alone does not establish a better design.

Checkpoint: you can identify the files changed, explain the new responsibilities, and locate the actual test output. Any skipped test or unexecuted command remains an explicit gap.


## Exercise 3 Review and Open a Pull Request

*15:08 to 15:13*

```text
Review the current diff against docs/payoff-rules.md and the tests. Separate intentional bug fixes from structural refactoring. Identify every behavior change, changed public contract, configuration default, validation exception, and test assertion change. Cite files and symbols. Do not edit files, commit, push, or merge. If the evidence is incomplete, say what you could not establish.
```

```text
git diff --check
git diff --stat
dotnet test
```

### Commit only the intended files

Review the working tree. Use git add -p for reviewed changes in tracked files; stage each reviewed new source file separately with git add -- followed by its path. Inspect git diff --cached. Keep generated files and unrelated changes out.

```text
git diff --cached --check
git commit -m "Refactor payoff service with preserved rules"
git push -u origin refactor/payoff-service
```

```text
Draft a concise pull request description from the actual diff and test output. Explain the problem, the change, resulting behavior, tests run with actual results, and remaining limitations. Separate the earlier bug-fix commit from this structural refactor. Do not claim a security scan, a code review, or a successful test that did not run.
```

On GitHub.com, choose Compare & pull request for the pushed branch, confirm the base and head, paste the reviewed description, and use the Create pull request dropdown to create a draft. Keep the PR unmerged for review.

At 15:13, stop for the reveal. Export reviewed staged changes with git diff --cached --output=../lighthouse-refactor-review.patch; stage reviewed new files first. After a commit, retain it and use git format-patch -1 --output-directory ../lighthouse-review if pushing is blocked. These Git options preserve patch encoding across shells. Record incomplete work.


## Cloud Agent Reveal and Review

*15:13 to 15:20*

In your browser, open the GitHub issue delegated at launch and inspect its progress. The remote task changes endpoint input validation; your local session refactors the service. Record whether the remote task uses a cloud sandbox or an Actions-based environment. The supplied setup workflow applies to the Actions-based route. [G3]

1. Read the issue acceptance criteria before reading the PR summary.

1. Open the changed files, commits, and reported checks. Compare the actual implementation and tests with every criterion.

1. Request Copilot code review if enabled for this repository. Read the comments and check their claims against the code. [G4]

```text
Review this pull request against the issue acceptance criteria and docs/payoff-rules.md. Focus on endpoint input validation, HTTP status and error shape, zero-balance behavior, deterministic tests, and unintended service changes. Identify missing evidence and possible regressions with file references. Treat passing checks and AI comments as inputs to human review. Do not merge the pull request.
```

### Human decision for this workshop

Keep a human reviewer responsible for the merge decision. Current Copilot approval capabilities can vary with configuration and preview settings; the workshop does not rely on an AI approval to satisfy the human review requirement. Verify the ruleset during rehearsal. [G4]

A cloud-agent PR may need workflow-run approval after inspection. The requester may need a separate human reviewer to meet required approvals. [G17] If the PR has not arrived, inspect its real status and use the labeled instructor-created endpoint solution. Record missing evidence before deciding.

Checkpoint: one criterion you verified, one concern or question, and a justified merge, request-changes, or wait decision.


## Advanced Security Observation Guide

*15:20 to 15:40  Facilitator led*

| Demonstration | Evidence to inspect | Decision to make |
| --- | --- | --- |
| Security overview | Repository coverage, alert state, and scope | Which repository or issue needs attention first? |
| CodeQL | Actual source-to-query data-flow evidence | Does this input reach an unsafe database operation? |
| Copilot Autofix | Proposed diff and verification results | Does the fix address the cause without a regression? |
| Dependency review | Affected version, advisory, and change in the PR | Does the proposed update resolve the relevant advisory? |
| Secret scanning and push protection | Observed alert or block and the tested pattern | What remediation or governed exception is appropriate? |
| Security campaign | Selected alert scope, owners, and agent-created work | Who verifies completion and the remaining risk? |

GitHub Code Security and GitHub Secret Protection are distinct products available for eligible Team and Enterprise Cloud organizations. Feature availability, permission, and usage arrangements require verification in the delivery organization. [G5]

### Training fixtures

The security copy contains an isolated vulnerable dependency fixture for advisory review. It is not required by the application runtime. The harmless training-marker route demonstrates a configured custom secret pattern. It does not prove recognition of an Azure provider token. A random provider-shaped key is not guaranteed to be blocked. [G6, G7]

Checkpoint for observers: choose the three features most useful to your role and explain their evidence, their limits, and the human action that remains.


## A Fix Still Needs Evidence

*Security review principles*

### SQL injection

Follow the request value into the lookup query. A correct repair uses a parameter for the value while preserving the intended search behavior. Review wildcard semantics, blank-input handling, result shape, and tests. A data-flow alert is stronger evidence than a matching keyword, but the actual GitHub alert still has to be generated and inspected.

### Dependency update

The demo fixture pins Newtonsoft.Json 12.0.3 for GHSA-5crp-9r3c-p9vr, also identified as CVE-2024-21907. The published affected range is below 13.0.1. Inspect the advisory and the proposed fixed version during rehearsal, and verify any other current advisories before using the fixture. The demonstration does not deserialize untrusted payloads. [G10]

### Secret handling

Never use a real credential to make a demonstration more convincing. For a blocked training push, remove the marker from the local commit before retrying. Do not choose a bypass merely to finish the lab. In a production exposure, revocation or rotation and incident handling matter; deleting a file alone does not invalidate a real credential. [G6]

### Agent-generated security changes

Autofix can propose a change, and agentic remediation can produce a PR when the feature is enabled. Review the actual diff, tests, scans, and permissions. A proposed fix is not the same event as a merged and verified remediation. [G7, G9]

Use the facilitator's Security Review Record to capture the actual alert, proposed fix, evidence, reviewer, and final state. If the live feature is unavailable, discuss the supplied source-review example and record that limitation.


## Code and Pull Request Review Checklist

*One page  Review the final version*

Reviewer: ____________________  Repository and PR: ____________________

Commit reviewed: ____________________  Route: live / worked example

### Correctness

[ ] Use the business rules and independent examples. Check exact amounts, dates, null or invalid inputs, and zero principal.

Evidence or correction: __________________________________________

### Security

[ ] Inspect SQL parameters, error responses, configuration, logs, input validation, and any secret or dependency finding.

Evidence or correction: __________________________________________

### Maintainability

[ ] Check names, method responsibilities, duplicated validation, documentation, and whether the tests explain the rules.

Evidence or correction: __________________________________________

### Behavior and evidence

[ ] Separate intended fixes from refactoring. Review the actual diff and test output. Count executed tests and check what is still untested.

Evidence or correction: __________________________________________

### Provenance and release

[ ] Review generated-code provenance and applicable license obligations under team policy. Verify branch, permissions, reviewer, and exact action before merge.

Evidence or correction: __________________________________________

### Decision

[ ] Request changes   [ ] Wait for evidence   [ ] Human approval for this version

Unresolved question and owner: _____________________________________

Actual tests and checks inspected: ___________________________________

Review the latest diff again after any revision. A green check, AI comment, or short method is not sufficient evidence by itself.


## Recovery Paths

*A useful fallback preserves the learning objective*

| Problem | Next action |
| --- | --- |
| Local SDK or restore is blocked | Pair at a prepared local station. Record the device or access issue for the administrator and continue the exercise. |
| Copilot access is missing | Confirm the assigned account and route with the facilitator. Use the prepared code and observer questions while access is checked. |
| Ask or Edit labels are absent | Use the rehearsed read-only or bounded-session equivalent. Confirm the sources and permitted actions. |
| Tests pass without exercising the defect | Check executed count, test inputs, changed source, and expected values. Add the exact day-15 and midpoint cases. |
| A recovery patch will not apply | Save your diff, run git apply --check, and compare the intended changes. Do not force the patch over learner work. |
| Refactor changes behavior | Stop, identify the changed rule or assertion, and restore the passing behavior before continuing. |
| Cloud PR or review is delayed | Show real status, then review the labeled worked example. No arrival time is guaranteed. |
| Autofix or campaign is unavailable | Use a genuinely rehearsed PR if available, or inspect the authored example and explain the missing live evidence. |
| Fake provider key is not blocked | Use the pretested harmless custom pattern or a genuine rehearsal capture. Do not use a real secret or claim a block occurred. |

Record the route actually completed: live edit, observed demonstration, or prepared example. All three can support review practice, but they establish different evidence about the environment.


## Observer Track and Day 2 Transfer

*A one page companion for noncoders*

| During | Your task |
| --- | --- |
| Launch | Read the issue. Identify the requested outcome and one missing constraint. |
| Explain | Compare the plain-language answer with the rules and a coder's explanation. |
| Tests | Predict the result for day 15, day 16, and the exact half-cent case. |
| Refactor | Choose the logging, async, or dependency-diagram card and inspect the demonstrated result. |
| PR reveal | Verify one acceptance criterion and vote merge, request changes, or wait with a reason. |
| Security demo | Choose three useful features and identify what each does and what remains a human decision. |
| Close | Name a bounded pilot, its owner, and two checks needed before release. |

### Your pilot idea

Task and beneficiary: _____________________________________________

Authorized repository or source: ___________________________________

Reviewer and required evidence: __________________________________

Success measure and stop condition: ________________________________

Use the separate Observer_Prompt_Pack for the detailed cards and worksheets. It includes a complete role-appropriate activity for every segment of the hour and a half.


## Reference Artifacts and Honest Evidence

*After the core exercises*

The facilitator package contains a worked solution, regression tests, independently computed payoff vectors, recovery patches, and verification records. Use these to inspect an outcome or recover time. They are instructor-created artifacts, not claims about a cloud-agent session that has already run.

### Compare your result with the reference

- Confirm the same input values, dates, rule version, and expected total before comparing test outputs.

- Compare method responsibilities and public contracts rather than requiring identical helper names.

- Read the endpoint task separately from the service refactor. One PR should not silently absorb the other task.

- Distinguish actual local build and test evidence from pending GitHub-hosted checks.

### Actual product screenshots

The package does not invent GitHub or VS Code screenshots. The readiness record lists the real views to capture in the delivery organization: context selection, a meaningful failing test, a green run, a diff, the agent session, a PR review, a CodeQL alert, an Autofix proposal, a dependency check, and a blocked training push.

### Before the lab ends

Save or commit the work you intend to retain and record incomplete items. Stop the local API with Ctrl+C and close completed local sessions. Retain repositories, branches, alerts, and review evidence under the workshop lifecycle. The facilitator handles any remaining remote agent tasks.

The next question is practical: which repeatable task can your team pilot with a clear test oracle and a named reviewer?


## Product References

*Product sources checked November 1 2026 and local setup November 3*

- **G1 VS Code Chat view and sessions:** https://code.visualstudio.com/docs/agents/run/chat-view

- **G2 VS Code custom instructions:** https://code.visualstudio.com/docs/agent-customization/custom-instructions

- **G3 Copilot on GitHub and cloud environments:** https://docs.github.com/en/copilot/concepts/copilot-surfaces/copilot-on-github

- **G4 Copilot code review:** https://docs.github.com/en/copilot/how-tos/use-copilot-agents/use-code-review

- **G5 GitHub Advanced Security:** https://docs.github.com/en/get-started/learning-about-github/about-github-advanced-security

- **G6 Secret scanning detection scope:** https://docs.github.com/en/code-security/reference/secret-security/secret-scanning-scope

- **G7 Resolve code scanning alerts:** https://docs.github.com/en/code-security/how-tos/manage-security-alerts/manage-code-scanning-alerts/resolve-alerts

- **G8 Local .NET development in VS Code:** https://code.visualstudio.com/docs/languages/dotnet

- **G9 Security campaigns:** https://docs.github.com/en/code-security/concepts/security-at-scale/about-security-campaigns


## Product References Continued

*Provider, context, and review details*

- **G10 Published Newtonsoft.Json advisory:** https://github.com/advisories/GHSA-5crp-9r3c-p9vr

- **G11 GitHub dependency review action:** https://github.com/actions/dependency-review-action

- **G12 .NET support policy:** https://dotnet.microsoft.com/en-us/platform/support/policy/dotnet-core

- **G13 Microsoft.Data.Sqlite async limitations:** https://learn.microsoft.com/en-us/dotnet/standard/data/sqlite/async

- **G14 VS Code chat context:** https://code.visualstudio.com/docs/chat/copilot-chat-context

- **G15 VS Code approvals and permissions:** https://code.visualstudio.com/docs/agents/run/approvals

- **G16 VS Code prompt files:** https://code.visualstudio.com/docs/agent-customization/prompt-files

- **G17 Review output from Copilot:** https://docs.github.com/en/copilot/how-tos/copilot-on-github/use-copilot-agents/review-copilot-output

The administrator validation matrix records current feature qualifications, usage distinctions, source details, and the delivery checks. Recheck the linked documentation within the week before delivery.
