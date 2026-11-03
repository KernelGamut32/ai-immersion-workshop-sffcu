# Lighthouse Member Services Lab Guide

*Lab 2  GitHub Copilot and Advanced Security*

You will explain an unfamiliar service, create tests from independent business rules, correct its defects, and refactor it with an agent. You will then assess a cloud-agent pull request and inspect how security findings become reviewed fixes. The goal is working code you can explain and evidence you can defend.

### What you will leave with

- A documented module and a README explanation linked to the business rules.

- A test suite with meaningful financial boundaries and a corrected service.

- A reviewed refactor and a pull request, where repository access permits it.

- A cloud-agent issue and PR review record, or an explicitly identified worked-example review if cloud execution is unavailable.

### Start with the right evidence

The starter has deliberate defects. Its passing smoke test establishes that the app starts and responds at /health. It does not verify the database or payoff calculations. docs/payoff-rules.md defines the fictional product rules. The supplied implementation is something to test against those rules.

This local edition uses .NET 10 LTS, SDK 10.0.403 and runtime 10.0.14. Product guidance was checked November 1, 2026; local setup was reviewed November 3. Confirm the SDK with dotnet --version from the repository root. [G8, G12]

## Exercise 1 Explain the Payoff Service

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

## Exercise 1 Document Without Hiding Defects

```text
Add XML documentation to the public members of LoanPayoffService using the actual implementation and model definitions. Keep each summary to one sentence. Document inputs, output, and exceptions actually thrown. Do not describe the known grace or rounding defects as approved business rules. Draft a README section named Loan payoff rules that links to docs/payoff-rules.md and distinguishes intended rules from current discrepancies. Change documentation only, then show the diff.
```

### Review the edit

1. Inspect the diff. Only documentation should change in this step.

1. Check that exception documentation names exceptions the method can actually throw and describes the triggering condition.

1. Check that the README links to the business rules. Avoid a description that makes the 10-day constant sound approved.

### Context challenge

```text
Compare the implementation, docs/payoff-rules.md, and the proposed tests. List each disagreement, the authoritative rule, a concrete input that exposes it, and the expected output. Do not resolve a disagreement by changing the rules to match the code.
```

### Before moving on

Checkpoint: XML comments and a README section, with no unintended behavior change. If Copilot cannot edit, retain the reviewed draft and apply it manually or use the worked example during recovery.

## Exercise 2 Establish the Test Oracle

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

## Exercise 3 Delegate and Inspect the Work

```text
First read .github/copilot-instructions.md, CONTRIBUTING.md, docs/payoff-rules.md, and the passing payoff tests. Propose a short plan, then refactor LoanPayoffService.CalculatePayoff to no more than 40 nonblank, noncomment lines by extracting validation, interest, fee, and rounding into clearly named helpers. Extract member validation shared with MemberService into one validator. Move the hard-coded SQLite connection string to the existing IConfiguration path, keeping the same database and loopback behavior. Preserve public signatures, decimal types, the corrected 15-day inclusive grace period, and final AwayFromZero rounding. Run the tests after each coherent change. Do not delete tests, change expected values, introduce new packages, fix unrelated security-demo flaws, push, or merge. Stop and explain if a behavior change is necessary. Finish with the changed files, commands run, results, and residual risks.
```

### Follow the actual execution

### End condition

The desired result is a short public method with understandable helpers, shared validation, configuration-based connection handling, and a green regression suite. A smaller line count alone does not establish a better design.

Checkpoint: you can identify the files changed, explain the new responsibilities, and locate the actual test output. Any skipped test or unexecuted command remains an explicit gap.

## Exercise 3 Review and Open a Pull Request

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

## Review

1. Read the issue acceptance criteria before reading the PR summary.

1. Open the changed files, commits, and reported checks. Compare the actual implementation and tests with every criterion.

1. Request Copilot code review if enabled for this repository. Read the comments and check their claims against the code.

```text
Review this pull request against the issue acceptance criteria and docs/payoff-rules.md. Focus on endpoint input validation, HTTP status and error shape, zero-balance behavior, deterministic tests, and unintended service changes. Identify missing evidence and possible regressions with file references. Treat passing checks and AI comments as inputs to human review. Do not merge the pull request.
```

### Human decision for this workshop

Keep a human reviewer responsible for the merge decision.
