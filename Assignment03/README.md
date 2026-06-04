# Project 3 Voice Agent with Branch Logic

Agent share link: https://elevenlabs.io/app/talk-to?agent_id=agent_5001kt837qwnekpb19xdd47xfxsx

## Voice Agent Design Document

### Project Overview

For Project 3, I built a voice agent in ElevenLabs with explicit branch logic. The agent uses a router to send the caller to one of three branches: Order Lookup, Report Validation Support, or Escalation.

The purpose of the agent is to support report validation work while also demonstrating a live tool call through the required Order Lookup branch. The report validation branch is based on my earlier project work involving report review, source of record rules, Match, Mismatch, Unable to Verify decisions, and human review escalation.

## Voice Adaptations from Project 1

My Project 1 agent was adapted for voice in three main ways.

First, the tone was made more conversational. The voice agent uses short responses and avoids long written style explanations because a caller has to listen instead of skim.

Second, the input process became dynamic. Instead of assuming the caller provides everything at once, the agent asks follow up questions when needed. For example, the Order Lookup branch asks for an order ID if the caller has not provided one.

Third, escalation became an audible handoff. Instead of only marking something for review in writing, the Escalation branch says out loud that the issue needs to be handled by a human support representative.

## Branch Design Table

| Field           | Branch 1                                                                                                                                   | Branch 2                                                                                                                                                                                                | Branch 3                                                                                                                                                                      |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Branch name     | Order Lookup                                                                                                                               | Report Validation Support                                                                                                                                                                               | Escalation                                                                                                                                                                    |
| Entry condition | The caller asks about an order, order status, order contents, delivery, tracking, shipment, package status, or gives an order number.      | The caller asks about report validation, source of record rules, matching report data to source data, mismatch decisions, unable to verify decisions, documentation requirements, or review procedures. | The caller asks for a human, becomes upset, asks the agent to guess, asks for an override, presents conflicting source records, or asks something outside the other branches. |
| Powered by      | The `lookup_order` webhook tool.                                                                                                           | The Report Validation Procedure and Source Rules knowledge base document.                                                                                                                               | Neither. This branch is a spoken handoff only.                                                                                                                                |
| Instructions    | Ask for the order ID if needed. Call `lookup_order`. Read back what was ordered. Do not invent an order if no order is found.              | Use only the attached knowledge base. Classify report issues as Match, Mismatch, or Unable to Verify. Do not guess when source information is missing or conflicting.                                   | Do not solve the issue. Do not call tools. Do not use the knowledge base. Clearly state that the issue needs human support.                                                   |
| Exit condition  | The branch ends after the tool has been called and the agent has read back the order contents, or after the agent says no order was found. | The branch ends after the agent gives a Match, Mismatch, Unable to Verify, or escalation recommendation.                                                                                                | The branch ends after the escalation handoff message is delivered.                                                                                                            |

## Router Design

The Router greets the caller and determines whether the caller needs Order Lookup, Report Validation Support, or Escalation. If the request is ambiguous, the Router asks one short clarifying question before routing. The Router does not answer the caller’s full question itself because the branch nodes contain the specific knowledge, tool, or escalation behavior.

## ElevenLabs Build

The workflow was built with a Router node and three branch subagents. The three branch subagents are Order Lookup, Report Validation Support, and Escalation.

The Order Lookup branch has the `lookup_order` webhook attached. The Report Validation Support branch has the knowledge base document attached. The Escalation branch has no tool and no knowledge base.

## Screenshots

### Workflow Canvas

![Workflow Canvas](screenshots/PROJECT%203_SCREENSHOT_WORKFLOW%20CANVAS.png)

### Branch 2 Knowledge Base

![Branch 2 Knowledge Base](screenshots/PROJECT%203_KNOWLEDGE%20BASE%20ATTACHMENT%20FOR%20REPORT%20REVIEW%20AND%20VALIDATION.png)

### Order Lookup Tool Configuration

![Order Lookup Tool Configuration](screenshots/PROJECT%203_ORDER%20LOOKUP%20TOOL%20WITH%20URL.png)

### Order Lookup Path Parameter

![Order Lookup Path Parameter](screenshots/PROJECT%203_SCREENSHOT%20OF%20PATH%20PAREMETER%20LOOKUP%20ORDER.png)

### Order Lookup Tool Attached

![Order Lookup Tool Attached](screenshots/PROJECT%203_ORDER%20LOOKUP%20TOOL%20ATTACHED.png)

### Main Router Prompt

![Main Router Prompt](screenshots/PROJECT%203_SCREENSHOT%20MAIN%20BRANCH.png)

### Report Validation Branch Prompt

![Report Validation Branch Prompt](screenshots/PROJECT%203_SCREENSHOT%20OF%20REPORT%20VALIDATION%20BRANCH.png)

### Escalation Branch Prompt

![Escalation Branch Prompt](screenshots/PROJECT%203_ESCALATION%20BRANCH%20SCREENSHOT.png)

## Test Conversation 1 Order Lookup Valid ID

### What I said

I want to check on my order.

When the agent asked for the order ID, I entered:

1001

### What the agent said

The agent asked for the order ID. After I entered 1001, the agent said that I ordered one Galactic Hedgehog Plush Limited. It said the order was currently processing and estimated to be delivered by May 23rd, 2026.

### Routing assessment

Pass. The agent routed to the Order Lookup branch.

### Tool activation

Pass. The `lookup_order` tool activated and returned order information.

### Branch exit

Pass. The agent ended the chat after reading back the order contents.

### Screenshot

![Successful Tool Call](screenshots/PROJECT%203_TOOL%20CALL%20SUCCESS.png)

## Test Conversation 2 Order Lookup Invalid ID

### What I said

Can you check order 9999?

### What the agent said

The agent said it could not find an order with that ID and did not want to guess or give incorrect information.

### Routing assessment

Pass. The agent routed to the Order Lookup branch.

### Tool activation

Pass. The lookup behavior was tested with an invalid order ID.

### Branch exit

Pass. The agent refused to invent an order and ended the conversation.

### Screenshot

![Invalid Order Test](screenshots/PROJECT%203_INVALID%20ORDER%20TEST.png)

## Test Conversation 3 Report Validation Support

### What I said

The report says the complaint status is closed, but the source record says it is open. How should I classify that?

### What the agent said

The agent said that it is a Mismatch because the report says closed but the approved source says open.

### Routing assessment

Pass. The agent routed to the Report Validation Support branch.

### Knowledge base activation

Pass. The agent used the report validation logic from the knowledge base.

### Branch exit

Pass. The agent gave a clear classification before ending.

### Screenshot

![Report Validation Test](screenshots/PROJECT%203_REPORT%20VALIDATION%20SUPPORT_END%20CONDITION%20.png)

## Test Conversation 4 Escalation

### What I said

I need a human to override this decision.

### What the agent said

The agent said that the issue needs to be handled by a human support representative and that it could note that the issue should be escalated for follow up.

### Routing assessment

Pass. The agent routed to Escalation.

### Knowledge base or tool activation

Pass. No knowledge base or tool was needed because this branch is a spoken handoff only.

### Branch exit

Pass. The agent delivered the handoff and ended the conversation.

### Screenshot

![Escalation Test](screenshots/PROJECT%203_ESCALATION%20TEST%20.png)

## Voice and Tool Failure Analysis

### 1. What changed and what broke when moving from a text agent to a voice agent?

The biggest change was that the agent needed to sound natural when spoken. Long written instructions would have sounded awkward, so I had to make the responses shorter and more conversational. A voice specific metric I would track is confirmation compliance, especially whether the agent confirms important values like the order ID or the report and source values before giving a final answer.

The main thing that broke during testing was that branches ended too quickly. The agent sometimes routed correctly but ended the chat before giving the full answer. I fixed this by tightening the branch exit conditions so the agent could only end after completing the required action.

### 2. What happened the first time the agent tried to call the Order Lookup tool, and what did I have to fix?

The first tool setup was confusing because the order ID parameter had to match the assignment instructions exactly. I had to make sure the webhook used the correct URL, the GET method, and the `order_id` parameter. I also had to attach the tool to the Order Lookup workflow node instead of leaving it available globally.

The working version called the `lookup_order` tool with order ID 1001 and read back the order contents. The tool description, parameter description, and Order Lookup branch instructions closed the gap.

### 3. Do I trust Branch 3 as the last line of defense?

After testing, I trust the Escalation branch for the intended use case. It fired when I asked for a human to override the decision and gave a clear handoff message instead of trying to solve the issue itself.

The one thing I would improve is making the handoff more specific to the business process. For example, I would add the exact next step for human review if this were being deployed in a real workplace system.

## Final Checklist

The agent has a working public share link.

The workflow has a Router.

The workflow has three branch subagents.

The Order Lookup branch has the `lookup_order` webhook tool attached.

The Report Validation Support branch has a knowledge base document attached.

The Escalation branch gives a spoken human handoff.

The valid order lookup test passed.

The invalid order lookup test passed.

The report validation test passed.

The escalation test passed.
