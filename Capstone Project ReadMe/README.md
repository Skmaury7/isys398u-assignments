# Capstone Project: Complaint Governance Report Validation Agent

## Project Overview

For my capstone project, I built a Complaint Governance Report Validation Agent using Relevance AI.

The purpose of the agent is to perform a first pass review of a synthetic monthly complaint governance report against synthetic source data. The agent checks for numeric mismatches, missing source support, unsupported conclusions, incomplete trend explanations, and escalation concerns.

The agent returns a structured findings table, an overall result, a human review required decision, and a short explanation.

This project uses synthetic data only. No real customer data, company data, confidential complaints, or production systems were used.

## Tool Used

Tool: Relevance AI

Relevance AI was not one of the tools used or demonstrated in this course. I chose it because it allows users to build an AI worker with a defined role, instructions, task flow, and structured output.

This meets the tool discovery requirement because the tool is new to the course and is agentic.

## What the Agent Does

The agent reviews two inputs:

1. Synthetic source data
2. Synthetic draft complaint report language

The agent compares the draft report to the source data and flags issues such as:

1. Report values that do not match source values
2. Claims that are not backed up by the data
3. Missing source support
4. Complaint trends that are minimized
5. Items that should go to human review

## Files in This Folder

| File | Description |
|---|---|
| CAPSTONE_Written_Brief.pdf | Final written brief for the capstone project |
| CAPSTONE_Written_Brief.docx | Editable version of the written brief |
| capstone_demo_recording.mp4 | Recorded demo of the working agent, if uploaded directly |
| README.md | Summary of the capstone project |

## Demo Recording

The demo recording shows the working agent in Relevance AI.

In the demo, the source data shows that complaint volume increased from 100 to 160 and that there were 2 high priority pain points. The draft report incorrectly says complaint volume was stable, high priority pain points were zero, and no escalation was required.

The agent correctly returns:

1. Overall Result: Fail
2. Human Review Required: Yes
3. A structured findings table explaining the issues

Demo video: Added the video file link to the folder and here: https://drive.google.com/file/d/1IQsV7QfmSnKEPF4iDzfJFZUB-cjcGreL/view?usp=sharing

## Required Course Concept Connections

### 1. Agent Card

The written brief includes a full Agent Card with the agent’s purpose, role, inputs, task steps, constraints, output format, escalation trigger, and success metric.

### 2. Evaluation

The written brief includes three documented test runs. The agent was scored on correctness, completeness, safety, and fit.

The three test cases were:

1. Governance risk scenario
2. Mostly clean report scenario
3. Missing source data scenario

### 3. Risk and Governance

The written brief discusses the main risks for this build, including data leakage, prompt injection, tool abuse, over permissioning, false confidence, and incorrect escalation decisions.

The agent was intentionally kept narrow. It does not connect to real workplace systems or confidential data.

### 4. Business Case

The business case is a faster and more consistent first pass review. The agent would not replace a complaints governance analyst, but it could help catch errors earlier, save review time, and create a clearer review trail.

## Honest Limits

This agent is not production ready. It was tested only on a small set of synthetic data. Before it could be used with real data, it would need company security approval, access controls, logging, clearer materiality rules, prompt injection testing, and a larger evaluation set.

## Rubric Alignment

| Rubric Area | How This Project Meets It |
|---|---|
| Tool discovery and novelty | Uses Relevance AI, which was not used or demonstrated in class |
| The build | Working complaint governance report validation agent |
| Presentation and demo | Demo shows the agent running on a synthetic governance risk test case |
| Written brief | Includes all seven required sections |
| Honest limits and reflection | Names real limits and next steps for the build |

## Final Submission

This folder contains the written brief and demo materials for the capstone project.
