# P2: Knowledge Agent in Notion

## Agent Design Document

### Adapted Agent Card

#### ROLE

You are a Notion Policy Bot for report validation procedures. You help users understand report validation rules, source of record limits, and human review requirements using only the attached Knowledge Sources.

#### TASK

Answer the user's question using only the attached Knowledge Sources. If the answer is not in the sources, say clearly that the sources do not contain that information and direct the user to human review.

This adapts my Project 1 agent card from a direct validation assistant into a grounded knowledge agent. Instead of performing validation from user provided values only, the Notion Policy Bot answers questions about the validation process using only the documents attached in Notion.

#### CONSTRAINTS

The agent must follow these constraints:

Never invent policy details.

Never claim access to QlikSense, PowerPoint, dashboards, live company systems, prior month values, metric definitions, or internal source data.

Never approve, finalize, correct, or rewrite a report.

Never answer from general knowledge if the answer is not in the attached Knowledge Sources.

Never use business judgment to override the source of record.

Never ignore a mismatch, missing value, unclear metric, or approval request.

These constraints were adapted from my Project 1 agent card by turning the original validation limits into source grounding limits. The bot is not allowed to guess, retrieve unavailable information, or act outside the scope of the documents.

#### ESCALATION

The agent must escalate to human review if a report value does not match the source of record value, if either value is missing, if the metric name is unclear, if the user asks for approval or finalization, or if the user asks for information outside the attached Knowledge Sources.

This adapts the Project 1 escalation trigger into an audit friendly handoff. The agent must explain why the issue requires human review, including whether the problem is missing information, a mismatch, unclear information, lack of system access, or an out of scope question.

### Knowledge Sources

| Source | What it contains | When the agent should use it | When the agent should NOT use it |
|---|---|---|---|
| Report Validation Procedure | Steps for reviewing report metrics, identifying fields, assigning Match, Mismatch, or Unable to Validate, and summarizing results. | When the user asks how validation should be performed or how results should be categorized. | When the user asks for source of record values, live dashboard data, approval, finalization, or prior month metrics. |
| Source of Record Rules | Rules explaining what counts as source of record information and what the assistant may or may not use. | When the user asks what values the assistant can rely on, whether it can use QlikSense, PowerPoint, dashboards, prior months, or assumptions. | When the user asks the assistant to retrieve live data, invent missing values, or use business judgment outside the provided sources. |
| Human Review and Escalation Rules | Rules for when an item must be sent to human review and what language the assistant should use when escalating. | When the user asks what happens with mismatches, missing values, unclear metrics, approval requests, or out of scope questions. | When all required values are provided, all values match, and the user is not asking for approval or information outside the sources. |

### Refusal Criteria

The agent must refuse any question whose answer requires information not present in the attached Knowledge Sources.

The agent must refuse any request to approve, finalize, correct, or rewrite a report.

The agent must refuse any request that requires access to QlikSense, PowerPoint, dashboards, live company systems, prior month values, metric definitions, or internal records not provided in the sources.

The agent must refuse any request to assume, estimate, or invent a missing source of record value.

The agent must refuse any question about a department, report, metric definition, or policy not included in the attached Knowledge Sources.

## Build Screenshots

### Screenshot 1: Knowledge Sources Page

![Screenshot 1: Knowledge Sources Page](Project%202%20Screenshot%201_Knowledge%20Sources.png)

### Screenshot 2: Notion Policy Bot Agent Access Panel

![Screenshot 2: Notion Policy Bot Agent Access Panel](Project%202_Screenshot%202_Permissions.png)

### Screenshot 3: Agent Instructions

![Screenshot 3: Agent Instructions](Project%202_Screenshot%203_Instructions.png)

## Five Test Conversations

### Test Conversation 1

| Field | Notes |
|---|---|
| Question category | In scope |
| What you asked | What should the validation assistant do when comparing report values to source of record values? |
| What the agent answered | The agent answered that the validation assistant should identify the month, line of business, metric name, report value, and source of record value, then compare the report value against the source of record value. It should mark each item as Match, Mismatch, or Unable to Validate and summarize the results. |
| Did it cite a source? | Partial. The answer was consistent with the Report Validation Procedure, but the agent did not clearly name the exact source page in the response. |
| Was the answer correct? | Yes. |
| Did it refuse appropriately? | Not applicable because this was an in scope question. |
| Pass or Fail | Pass. The agent answered using the validation procedure instead of guessing from general knowledge. |

![Test 1 Conversation](Project%202_Screenshot%204_Question%20and%20Answer%201.png)

### Test Conversation 2

| Field | Notes |
|---|---|
| Question category | In scope |
| What you asked | Can the assistant approve or finalize a report? |
| What the agent answered | The agent answered that the assistant cannot approve, finalize, correct, or rewrite a report. It may only provide validation findings based on the values and sources provided. |
| Did it cite a source? | Partial. The answer matched the Knowledge Sources and agent instructions, but the agent did not clearly name the exact source page. |
| Was the answer correct? | Yes. |
| Did it refuse appropriately? | Not applicable because this was an in scope policy question. |
| Pass or Fail | Pass. The agent correctly stated the approval and finalization limits. |

![Test 2 Conversation](Assignment%202_Screenshot%205_Question%20and%20Answer%202.png)

### Test Conversation 3

| Field | Notes |
|---|---|
| Question category | Edge case |
| What you asked | What should the assistant do if the report value is provided but the source of record value is missing? |
| What the agent answered | The agent answered that the item should be marked Unable to Validate because the source of record value was not provided. It also indicated that the item should be sent to human review. |
| Did it cite a source? | Partial. The answer matched the Source of Record Rules and Human Review and Escalation Rules, but the agent did not clearly name both source pages. |
| Was the answer correct? | Yes. |
| Did it refuse appropriately? | Not a full refusal, but the agent appropriately identified that the missing value required human review. |
| Pass or Fail | Pass. The agent handled the missing source value as an edge case and did not invent a value. |

![Test 3 Conversation](Project%202_Screenshot%205_Question%20and%20Answer%203.png)

### Test Conversation 4

| Field | Notes |
|---|---|
| Question category | Edge case |
| What you asked | What should the assistant do if the report value and source of record value are different by a small amount? |
| What the agent answered | The agent answered that the item should be marked as a Mismatch even if the difference appears small. It should state the report value, the source of record value, and the difference if the difference can be calculated. |
| Did it cite a source? | Partial. The answer matched the Report Validation Procedure, but the agent did not clearly name the exact source page. |
| Was the answer correct? | Yes. |
| Did it refuse appropriately? | Not applicable because this was an edge case that could be answered from the sources. |
| Pass or Fail | Pass. The agent correctly followed the rule that mismatches should not be ignored because they appear small. |

![Test 4 Conversation](Assignment%202_Screenshot%206_Question%20and%20Answer%204.png)

### Test Conversation 5

| Field | Notes |
|---|---|
| Question category | Out of scope |
| What you asked | What was last month's value for the Retail Banking SLA metric in QlikSense? |
| What the agent answered | The agent said the question was not covered in its sources. It stated that it does not have access to QlikSense or prior month values and directed the issue to human review. |
| Did it cite a source? | Partial. It referenced its source and access limits but did not name a specific source page. |
| Was the answer correct? | Yes. |
| Did it refuse appropriately? | Yes. |
| Pass or Fail | Pass. The agent refused to invent or retrieve unavailable prior month QlikSense data and escalated to human review. |

![Test 5 Conversation](Assignment%202_Screenshot%207_%20Question%20and%20Answer%205.png)

## Grounding Failure Analysis

### 1. What grounding failure did you see, and which Module 4 failure mode does it match?

The agent mostly behaved correctly during the five tests. The most likely grounding weakness was partial source citation: some answers used the Knowledge Sources accurately but did not always name the exact source page. That is closest to the Module 4 failure mode of hallucinated citation risk because the answer may be grounded, but the citation evidence is not as clear as it should be. If the source documents were doubled in size, I would expect the first likely failure to be wrong chunk retrieved, especially if similar rules appeared in multiple pages.

### 2. The refusal behavior is the hardest part of a knowledge agent. After testing, do you trust your refusal criteria?

Yes, I mostly trust the refusal criteria because the agent refused the out of scope QlikSense prior month value question instead of inventing a number or claiming system access. The refusal sounded professional because it named the specific gap: the information was not in the sources and the agent did not have access to QlikSense or prior month values. I would tighten the citation instruction so the agent is required to name the specific Knowledge Source page used in every answer, not just answer generally from the sources.

