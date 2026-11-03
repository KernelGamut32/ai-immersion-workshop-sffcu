# Lab 2 quick start

**Session:** 14:15 to 15:45, local Pacific time. **Scenario:** Lighthouse Member Services, a fictional credit-union API. **Facilitator:** Allen Sanders. **Edition:** local VS Code and .NET 10, November 3, 2026.

## Before class

Complete these steps on the computer you will use for the lab. Ask the facilitator for a prepared partner station if your device cannot run the approved tools.

1. Install desktop **VS Code**, **Git**, the **C# extension**, and the **.NET 10 SDK 10.0.403** selected for this package. Use the organization's approved installation process. The SDK provides build and test tools. C# Dev Kit is optional under the organization's policy.
2. Sign into VS Code with the approved GitHub account and verify Copilot access. Confirm that a local workspace session can read an attached file and use the permitted editing tools. Copilot and GitHub still need network access.
3. Open your assigned private repository on GitHub and verify its owner and name. Use VS Code's **Git: Clone** command with that repository's URL, then open the cloned folder containing `Lighthouse.sln`. Use the learner starter as your starting point. Confirm your Git author identity and authenticated repository access through the organization's normal Git setup.
4. Run the baseline commands below from that folder. They work in Windows PowerShell and the macOS terminal. Package restore needs access to the approved NuGet feed.

```text
git remote -v
git status --short
dotnet --version
dotnet restore Lighthouse.sln --locked-mode
dotnet test Lighthouse.sln --no-restore
```

The rehearsed SDK is **10.0.403**; `global.json` controls SDK selection. Do not change that file to work around a missing SDK. The untouched starter executes **one smoke test**, which checks `/health`. It does not verify the database or the payoff rules. The payoff test file is intentionally empty.

5. Verify the supplied database by starting the API:

```text
dotnet run --project src/Lighthouse.Api --no-launch-profile
```

Keep that terminal running. Open these addresses in a browser:

- `http://127.0.0.1:5080/health`
- `http://127.0.0.1:5080/api/loans/L0001/payoff?payoffDate=2026-10-31`

The first confirms an API response. The second must return the seeded loan result, confirming a database read. Its calculation is deliberately imperfect until Exercise 2; a response is not evidence of correct financial rules. The database file is already included, so no database service or seeding command is required. Stop the API with **Ctrl+C**. Keep its loopback binding and use only the fictional records.

6. Report the actual SDK, starter test count, database response, and Copilot readiness to the facilitator. Leave the starter source unchanged before class.

Setup reference: [Using .NET in VS Code](https://code.visualstudio.com/docs/languages/dotnet). The administrator runbook contains installation and account preparation details.

## During the lab

Open `docs/payoff-rules.md`, `src/Lighthouse.Api/Services/LoanPayoffService.cs`, and `tests/Lighthouse.Tests/LoanPayoffServiceTests.cs`. Keep the Lab Guide and `Prompt_Cards.md` beside the local editor.

Explain and document first. Generate tests from the business rules, observe the defects, correct the behavior, and refactor from a passing baseline. Record the actual tests, diff, and review decision in `03_Templates/Learner_Review_Record.md`.

At **15:13**, move to the GitHub cloud-agent PR reveal even if your own push is still pending. The remote endpoint task is separate from your local service refactor. Use the actual issue and PR links supplied for the session.

## Observers

Open `Observer_Prompt_Pack.pdf` and pair at a prepared local coder station. Compare the explanation with the rules, predict day-15/day-16 and midpoint results, inspect the delegated GitHub issue, and make a justified PR decision. Choose three security features and record the evidence and human decision for each. You have an activity in every segment.

## If something is blocked

After two minutes, ask for help and use the guide's fallback. Preserve your work. For an SDK, restore, or device problem, continue at a prepared partner station while the administrator investigates. A source-review example can support review practice but does not establish that a live cloud task or security scan ran. Use the route field in the review record to say what you actually completed.

## Finish

Save or commit your intentional work, record incomplete items, submit the brief feedback, and complete the Day 2 pilot card. Stop the local API with Ctrl+C and close completed local sessions. Retain repositories and evidence according to the agreed lifecycle. The facilitator checks any remaining remote agent task.
