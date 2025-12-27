# Decision Classification Logic

## Purpose

This document defines how the AI Decision Signal Digest maps observable collaboration signals to **exactly one** decision category.

The logic is:
- Deterministic
- Explainable
- Conservative by default

If a signal cannot be classified with confidence using the rules below, the system must **downgrade or remain silent**.

---

## Decision Categories (Immutable)

The digest classifies signals into one of four categories:

1. **Informational**
2. **Action Required**
3. **Decision Pending**
4. **Watchlist**

No additional categories are permitted.

---

## Core Design Principles

1. **One signal, one category**  
   Signals are never stacked or multi-labeled.

2. **Evidence over inference**  
   Classifications rely on observable inputs, not intent prediction.

3. **Downgrade on ambiguity**  
   When conditions are unclear, the system selects a lower-severity category.

4. **Explainability required**  
   Every classification must be justified using plain language and source references.

---

## Category Definitions and Rules

### 1. Informational

**Definition**  
Signals that provide situational awareness without implying urgency, action, or unresolved choice.

**Required conditions (all must be true)**
- No explicit questions detected
- No unresolved comment threads
- No permission or access changes that expand scope or risk
- No conflicting or rapid activity patterns

**Typical signals**
- Minor or moderate file edits
- New comments without replies
- File renames or moves without discussion
- Single-actor updates

**Rationale template**
> “Recent activity provides context but does not indicate a required action or unresolved decision.”

**Design intent**  
Most activity should fall here. This category prevents over-escalation.

---

### 2. Action Required

**Definition**  
Signals indicating a **clear next step** where:
- The task is known
- Ambiguity is low
- Ownership is reasonably implied by context

**Required conditions (at least one must be true)**
- Explicit question directed at a user or role
- Mention combined with an unresolved comment
- File shared or permissions changed with accompanying discussion
- Follow-up activity indicating expectation of response

**Disqualifiers (any one forces reclassification)**
- Conflicting viewpoints present
- Multiple unresolved alternatives discussed
- No clear owner implied

**Typical signals**
- “Can you review / approve / confirm?”
- Mentions followed by silence
- Threads where a single response would unblock progress

**Rationale template**
> “This activity suggests a clear next step that is currently pending.”

**Design intent**  
This category surfaces execution gaps, not decisions.

---

### 3. Decision Pending

**Definition**  
Signals indicating that a **choice must be made**, but:
- Inputs are conflicting, incomplete, or evolving
- No single action resolves the situation

**Required conditions (at least one must be true)**
- Multiple comments expressing differing viewpoints
- Explicit uncertainty (for example, “Which option should we take?”)
- Repeated edits reversing or revising prior changes
- Discussion followed by access or scope changes

**Disqualifiers**
- Clear task request with no alternatives
- Purely informational updates

**Typical signals**
- Design or strategy discussions
- Tradeoff debates
- Approval bottlenecks involving multiple stakeholders

**Rationale template**
> “Recent activity indicates unresolved alternatives or alignment is required before progress can continue.”

**Design intent**  
This is the highest cognitive-load category and should be used sparingly.

---

### 4. Watchlist

**Definition**  
Signals that do not require immediate action but have **elevated future impact or risk** if ignored.

**Required conditions (all must be true)**
- No explicit action request
- No active disagreement
- Potential impact inferred from metadata or context
- Low urgency now, higher consequence later

**Typical signals**
- Large permission expansions
- Sudden increase in collaborators
- Repeated minor edits without discussion
- Files with rising activity but unclear ownership

**Rationale template**
> “This activity does not require immediate action but may warrant monitoring due to potential downstream impact.”

**Design intent**  
This category preserves awareness without premature escalation.

---

## Classification Precedence (Internal Only)

When multiple category rules match, the system applies the following precedence:

1. Decision Pending
2. Action Required
3. Watchlist
4. Informational

This ordering ensures consistency and avoids under-classifying decision risk.

---

## Downgrade and Failure Rules

- If inputs are incomplete, downgrade the classification
- If signals conflict, downgrade the classification
- If source attribution is weak, downgrade or remain silent

**Silence is an acceptable outcome. Guessing is not.**

---

## What the System Never Does

The classification logic must never:
- Recommend decisions
- Assign owners or deadlines
- Summarize opinions
- Estimate urgency numerically
- Optimize for engagement

---

## Rationale

This logic mirrors how a senior analyst reasons from evidence:
- It privileges clarity over coverage
- It treats ambiguity as a signal, not a failure
- It avoids automation-first escalation

The goal is not to be clever.  
The goal is to be **trusted**.

---

## Status

This decision classification logic is intentionally complete.

Any changes require:
- Explicit versioning
- Updated disqualifiers
- Re-evaluation of downgrade behavior

