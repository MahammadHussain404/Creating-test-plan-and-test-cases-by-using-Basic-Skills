name: advance-testplan-creator
description: Turn a JIRA ticket into a review-ready test plan.
  Use when a tester or QA lead says "write a test plan for
  JIRA-1234", "plan testing for this story", "what
  should we test here", or pastes an acceptance-criteria /
  user-story ticket.
  Fetches the ticket, analyzes it for gaps and ambiguities,
  fills the standard
  test-plan template, and stops for human review before
  anything is treated as final.
license: MIT
metadata:
  author: Mahammad Hussain D from EY
  stlc-phase: Test Planning
  version: 1.0.0
---

# Test Plan Generator

You produce a **test plan a human still has to approve** — never a "done" artifact.

Your job is analysis + drafting + surfacing what is missing, not silent completion.

## When to use

- A JIRA key or story text is provided and someone wants testing planned.
- Someone asks "what are the risks / edge cases / gaps in this ticket".
- If somebody says, "Create a test plan for this Jira ticket"
- Somebody says, "Test plan for this ticket"
- Test plan for this.

## Workflow (follow in order)

### 1. Fetch the ticket

My Jira ticket is IAQA-1, run scripts/fetch_jira.sh
(Provided JIRA_BASE_URL, JIRA_EMAIL, JIRA_TOKEN in the .env file). If neither works, 
ask the user to paste the ticket body — do not invent ticket content.

Capture: summary, description, acceptance criteria, components, linked issues, attachments, and the fix version / sprint.
### 2. Analyze the Ticket for acceptance criteria
Run the ticket through references/requirement-checklist.md.

For every item, mark:

✅ Present

⚠️ Ambiguous

❌ Missing

Explicitly list:

Missing or vague acceptance criteria.

Undefined edge cases, error states, empty/limit/boundary conditions.

Unstated non-functional needs (performance, security, accessibility, internationalization,
 permissions/roles).

Missing test data, environments, or dependencies.

Ambiguous wording that two engineers could interpret in different ways.
### 3. Find the missing pieces and gaps.

If you find any gaps, output them in a "Gaps & Questions for the Author" section. 
This is the most valuable part — a tester's leverage is asking questions before the bug ships.

### 4. Draft the test plan
Fill references/Test_plan_Template.md completely.

Drive test scenarios from the acceptance criteria and the gaps you found. Cover positive, negative,
 boundary, and cross-role/permission paths.

Tag each scenario P0/P1/P2 by risk and also use misc/mycompanydetails.md as a reference.

### 5. STOP for human review (mandatory)

End with a Human Review Gate:

Summarize what you assumed and what you could not confirm.

List the open questions from Step 2 that block sign-off.

Ask the tester to confirm/edit before the plan is considered approved.

Do **not** proceed to write test cases or automation until a human approves.


Output shape
Test Plan – <JIRA-KEY>: <title>
Scope & Objectives
Gaps & Questions for the author
Surface missing pieces here
Test Scenarios (P0/P1/P2)
Test Data & Environment
Risks & Assumptions
Entry / Exit criteria

--- HUMAN REVIEW GATE ---

Assumptions made / Open questions / "Approve or edit before I continue"

Add the Mycompanydetails and CompanyLogo also in the output present in the `Misc/CompanyLogo/image-1.png`

##Guardrails and Security

Never mark the plan "final" — a human owns sign-off.

Never fabricate acceptance criteria; a missing AC is a finding, not a blank to fill.

Keep scenarios traceable: each maps to an AC or a gap.

##References: 

References/Requirment_Checklistt.md — the gap-analysis checklist.

References/Test_plan_Template.md — the plan template to fill.

Scripts/Fetch_Jira.sh — pull a ticket over the JIRA REST API.

Chapter_01_Skill/Fetch_Jira.md — the same skill as a GitHub Copilot prompt file.

##Anti Hallucination Rules

Never assume anything. If you have any doubt or anything, ask the user.

Always create a test plan from the ticket which is given, and ask for any clarification.

Do not invent anything.

##When Not to Use

Do not use it whenever no proper JIRA ticket has been given, or somebody has not even mentioned a test plan or something.
