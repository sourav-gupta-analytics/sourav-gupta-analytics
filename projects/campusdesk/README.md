# CampusDesk | Multilingual Complaint Routing

**Code Fuzion 2026 · Lovely Professional University · 24-hour hackathon · M1 (Moderate)**

[Open hosted demo](https://hinglish-complaint-router.netlify.app/) · [Back to portfolio](../README.md)

## Executive summary

CampusDesk is a campus service-desk prototype that transforms unstructured English, Hinglish and romanised Punjabi complaints into structured, prioritized tickets. The project addresses manual sorting errors and helps staff identify the responsible department, location and urgency.

## Business problem and requirements

The official M1 challenge, organized by Student Organisation Vizion Forge, required six department categories, location extraction, confidence-based human review, at least 75% department accuracy on unseen complaints, and per-complaint cost and latency reporting. The 75% target is a challenge requirement, not a confirmed judge score.

## Solution and process

1. Accept a free-text complaint.
2. Normalize text and rank departments using weighted multilingual rules, phrase context, negation/healthy mentions and explicit causes.
3. Extract block, floor and room when explicitly present; infer urgency from distinct cues.
4. Return a predicted department, heuristic confidence and an explanation. Send weak or conflicting predictions to a human review queue (72% heuristic threshold).
5. Let staff assign or resolve tickets and retain the original prediction for auditability.

Departments: Plumbing, Electrical, Wi-Fi, Mess, Academic and Other.

## Architecture

- JavaScript rule-based text classifier; no runtime LLM or external API.
- Responsive HTML/CSS/JavaScript interface.
- Static Netlify demo stores tickets in the current browser; it is not a shared campus database.
- Separate local Node.js HTTP API with SQLite persistence for a shared prototype queue.
- Node test runner and labeled synthetic complaint benchmarks.

## Evaluation

The supplied source was rerun on Node.js 22.16.0 on 24 November 2026. All eight automated tests passed, including a server restart/persistence test and human-review behavior.

| Known synthetic benchmark | Correct / total | Accuracy | Rerun p95 classifier latency |
| --- | ---: | ---: | ---: |
| Standard | 54/54 | 100% | 0.8892 ms |
| Challenge | 36/36 | 100% | 1.7413 ms |
| Context | 54/54 | 100% | 0.6691 ms |
| Additional vocabulary | 36/36 | 100% | 3.7406 ms |
| Combined | 180/180 | 100% | Not pooled |

These are regression datasets used during development, not independent unseen judge data. All 180 benchmark cases had a single intended category; tests separately cover ambiguous inputs. Latency is environment-specific classifier time, not network or end-to-end service time. External API spend is INR 0 per complaint; hosting and hardware costs are excluded.

## Business analytics relevance

- Translate stakeholder pain points into measurable classification, coverage and latency metrics.
- Design an operational workflow with human escalation and ticket lifecycle controls.
- Evaluate classification via labeled data, confusion matrices and language-level accuracy.
- Proposed production metrics: misrouting rate, time to assignment, resolution SLA, review rate and recurrence by location. No live operational impact or savings have been measured.

## Limitations and next steps

The rule-based score is not a calibrated probability. Unseen dialects, ambiguous multi-issue reports and spelling variants require broader independent evaluation. A production service needs authentication, authorization, privacy and retention policies, monitoring, and campus-user validation. The hosted demo URL is supplied by the participant; the live page could not be independently retrieved during this audit.

## Attribution

Hackathon participation/project reported by Sourav Gupta. Individual contribution breakdown, teammates and competition ranking have not been independently established. The original project README declares ChatGPT/Codex assistance for prompt analysis, implementation and synthetic tests. No award or solo authorship is claimed.

Source: supplied CampusDesk project archive and official Code Fuzion 2026 M1 problem statement (pp. 1–2). Source code and original Git history remain in the supplied archive; this page documents the project rather than republishing team code.
