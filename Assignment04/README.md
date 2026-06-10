# Assignment 04: Build a Crew and Govern It

## Submission

This folder contains my Project 4 submission.

PDF file: `Maury_Sami_Project 4_Build a Crew and Govern It.pdf`

## Summary

For this project, I built a Sequential CrewAI workflow for a monthly complaints governance report validation process. The crew reviews a draft complaints risk report, compares it to source data, classifies risk, prepares an approval packet, and uses a human approval checkpoint before the final memo.

During the run, the Human Approval Checkpoint did not receive real human approval. The agent used placeholder approval fields, and the guardrail blocked the workflow after 3 retries. I documented that result in the governance audit.
