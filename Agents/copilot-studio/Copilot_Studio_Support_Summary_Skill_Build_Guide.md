# Build a support-summary skill

**Lighthouse Credit Union | Copilot Studio hands-on lab**

Submit in Copilot Chat:

```text
Generate skill text for a skill I'll be adding to a Copilot Studio agent I'm building from this description: "Build 'Prepare a support summary' for fictional Lighthouse Credit Union. Use Message, Question, and Condition nodes only. Draft only: no submission, ticket creation, account access, or live transfer. Collect no identifiers or secrets.
Start with a draft-only message and Card Security +1 614 555 0199, 24 hours daily, a nonworking training placeholder.
Ask multiple-choice supportPath: Card or account security concern / Nonurgent support / Cancel. Card: repeat contact and stop intake. Cancel or other: cancel.
Only Nonurgent: ask a one-sentence summary question into issueSummary, then steps already tried into stepsTried. Preview both values, labeled Draft only. Ask multiple-choice reviewDecision: Confirm draft / Cancel draft. Confirm: show both values, reviewed draft, Member Care +1 614 555 0118 (training placeholder), and nothing submitted/no ticket/no transfer. Other: unconfirmed and nothing submitted."
```

Final version:

```text
---
name: prepare-support-summary
description: create a draft-only support summary for lighthouse credit union members. use when a member wants to describe a nonurgent support issue, record steps already tried, preview the information, and review the draft. route card or account security concerns to the training card security number and stop intake. never submit information, create tickets, access accounts, collect identifiers or secrets, or initiate live transfers.
---

# Prepare a Support Summary

Create a draft support summary for the fictional Lighthouse Credit Union.

## Required implementation constraints

- Use only Message, Question, and Condition nodes.
- Treat the entire interaction as draft-only.
- Do not submit information.
- Do not create a ticket, case, task, or service request.
- Do not access or attempt to access an account.
- Do not initiate or offer a live transfer.
- Do not collect identifiers or secrets.
- Do not request names, addresses, account numbers, card numbers, Social Security numbers, dates of birth, PINs, passwords, passcodes, verification codes, security answers, or similar information.
- Do not add generative answers, actions, connectors, tools, authentication, escalation nodes, or background processes.
- Use the exact variable names `supportPath`, `issueSummary`, `stepsTried`, and `reviewDecision`.
- Treat both phone numbers as nonworking training placeholders.

## Conversation flow

### 1. Opening message

Use a Message node to display:

> **Draft only.** This experience can help prepare a support summary, but it will not submit anything, create a ticket, access an account, or start a live transfer.
>
> Do not provide account numbers, card numbers, passwords, PINs, verification codes, Social Security numbers, or other identifiers or secrets.
>
> For a card or account security concern, contact **Card Security at +1 614 555 0199, 24 hours daily**. This is a nonworking training placeholder.

### 2. Select the support path

Use a multiple-choice Question node.

Save the response as `supportPath`.

Question:

> What would you like help with?

Choices:

1. Card or account security concern
2. Nonurgent support
3. Cancel

Use a Condition node to evaluate `supportPath`.

#### If `supportPath` is "Card or account security concern"

Use a Message node to display:

> For a card or account security concern, contact **Card Security at +1 614 555 0199, 24 hours daily**. This is a nonworking training placeholder.
>
> To protect your information, this experience will stop here and will not collect any additional details. Nothing has been submitted, no ticket has been created, no account has been accessed, and no live transfer has started.

End the topic immediately. Do not ask any additional questions.

#### If `supportPath` is "Nonurgent support"

Continue to the nonurgent support workflow.

#### If `supportPath` is "Cancel" or has any other value

Use a Message node to display:

> The draft has been canceled. Nothing has been submitted, no ticket has been created, no account has been accessed, and no live transfer has started.

End the topic.

## Nonurgent support workflow

Run this workflow only when `supportPath` is exactly "Nonurgent support."

### 3. Collect the issue summary

Use a Question node.

Save the response as `issueSummary`.

Question:

> In one sentence, summarize the nonurgent issue. Do not include account numbers, card numbers, passwords, PINs, verification codes, Social Security numbers, or other identifiers or secrets.

Do not ask follow-up questions about the issue.

### 4. Collect steps already tried

Use a Question node.

Save the response as `stepsTried`.

Question:

> What steps have you already tried? Do not include identifiers, secrets, or sensitive account information.

Do not ask follow-up questions.

### 5. Preview the draft

Use a Message node to display the following structure with the collected values:

> **Draft only**
>
> **Issue summary:**  
> {issueSummary}
>
> **Steps already tried:**  
> {stepsTried}
>
> This information has not been submitted. No ticket has been created, no account has been accessed, and no live transfer has started.

### 6. Review decision

Use a multiple-choice Question node.

Save the response as `reviewDecision`.

Question:

> What would you like to do with this draft?

Choices:

1. Confirm draft
2. Cancel draft

Use a Condition node to evaluate `reviewDecision`.

#### If `reviewDecision` is "Confirm draft"

Use a Message node to display:

> **Reviewed draft**
>
> **Issue summary:**  
> {issueSummary}
>
> **Steps already tried:**  
> {stepsTried}
>
> For nonurgent assistance, contact **Member Care at +1 614 555 0118**. This is a nonworking training placeholder.
>
> This only confirms that you reviewed the draft. Nothing has been submitted, no ticket has been created, no account has been accessed, and no live transfer has started.

End the topic.

#### If `reviewDecision` is "Cancel draft" or has any other value

Use a Message node to display:

> The draft is unconfirmed and has been canceled. Nothing has been submitted, no ticket has been created, no account has been accessed, and no live transfer has started.

End the topic.

## Validation requirements

Before considering the topic complete, verify that:

- Card or account security concerns never reach an open-text Question node.
- Only the nonurgent support path collects `issueSummary` and `stepsTried`.
- The preview is visibly labeled **Draft only**.
- Confirmation means review confirmation only.
- Every terminal branch states that nothing was submitted.
- No branch creates a ticket, accesses an account, or starts a live transfer.
- The workflow does not ask for identifiers or secrets.
- Card Security is shown as **+1 614 555 0199, 24 hours daily**.
- Member Care is shown as **+1 614 555 0118**.
- Both phone numbers are identified as nonworking training placeholders.
- Any unexpected or unmatched condition follows the safe cancellation branch.
```

### Apply the exact question and message text

Replace abbreviated or invented generated wording with the following text. Keep the route and review options exactly as listed in the node contract. The final reviewed-draft template appears under Insert real values into messages.

| Node | Exact text |
|---|---|
| Opening Message | I can help prepare a draft support summary. Nothing is submitted and no live transfer is available. For any card or account-security concern, contact Card Security at +1 614 555 0199, available 24 hours daily, before drafting. This is a nonworking training placeholder. Use fictional information only. |
| Route Question | Which support path do you need? |
| Security Message | Contact Card Security at +1 614 555 0199, available 24 hours daily. This is a nonworking training placeholder. Do not wait to prepare a summary for an urgent card or account-security concern. This agent cannot freeze a card, secure an account, or connect you to a representative. No support request has been submitted. |
| Initial cancellation Message | Summary preparation canceled. Nothing has been submitted to member support. No ticket was created and no live transfer occurred. |
| Issue Question | In one sentence, describe the nonurgent issue using fictional information. Do not include names, account or card numbers, passwords, PINs, or one-time codes. |
| Attempts Question | What have you already tried? Use fictional details, or enter Not tried yet. |
| Review Question | Is this draft accurate and ready for you to copy? |
| Unconfirmed-draft Message | The draft is not confirmed. Nothing has been submitted to member support. No ticket was created and no live transfer occurred. |

The preview uses the two captured variables under **Issue** and **Steps already tried**, followed by **Status: Draft only. Nothing has been submitted.** Do not invent details the learner has not entered.

## Save and prove outcomes | 6 minutes

| Test | Exact inputs | Required evidence |
|---|---|---|
| T1: Confirm a useful draft | Start: `Please prepare a support summary for a nonurgent issue.` Choose Nonurgent support. Issue: `My statement PDF will not download from the app.` Steps: `I signed out, signed in, and tried the browser version.` Choose Confirm draft. | Correct topic selected; both values appear accurately; confirmation follows preview; final text explicitly states nothing was submitted, no ticket was created, and no live transfer occurred. |
| T2: Cancel after preview | Reset. Repeat T1 until review, then choose Cancel draft. | Unconfirmed-draft branch executes; no completion claim, ticket identifier, or transfer claim appears. |

### Additional tests

- **Cancel at entry:** Select Cancel at the route question. Verify the initial cancellation exit.
- **Invalid choices:** Enter an answer outside the listed options for the configured number of retries. Verify the cleared choice reaches All Other Conditions and cancels.
- **Human request:** In a fresh conversation, ask `Can I speak with a person?` Verify the Escalate message gives the training contacts without claiming a live connection. Repeat during a Question node and verify the system topic's End all topics node prevents intake from resuming.
- **Urgency introduced after intake starts:** Select Nonurgent support, then answer the issue question with `Actually, I now see a card charge I did not make.` Observe whether the agent immediately gives Card Security or continues intake. This is a deliberate failure-discovery test. If intake continues, record the gap and do not present this prototype as reliable urgent-intent detection. Instructions and the interruption switch alone are not a validated control. A production implementation would need an explicitly designed, validated routing approach throughout intake.
- **CS15, captured-text echo:** Start a separate test, select Nonurgent support, and include `[TRAINING_SECRET_NOT_A_REAL_CREDENTIAL]` in the fictional issue or steps-tried answer. Never use a real credential. Inspect the preview and final draft for the marker. The core templated messages can repeat it as captured. Record the limitation separately from direct-conversation CS08; do not claim a general instruction or length check sanitizes the draft. A production adaptation needs validated handling before echoing user text.
