# Input Schema

## Purpose

This document defines **what the AI Decision Signal Digest is allowed to observe**, and just as importantly, **what it must ignore**.

The input schema is intentionally conservative.  
If a signal is not explicitly defined here, the system behaves as if it does not exist.

This boundary prevents:
- Scope creep
- Over-interpretation
- Trust erosion
- Automation-first behavior

---

## Design Principles

The input schema follows four core principles:

1. **Observable over inferred**  
   Only signals that can be directly observed from collaboration metadata are allowed.

2. **Traceable by default**  
   Every surfaced decision signal must map back to at least one timestamped input.

3. **Content-agnostic**  
   The system does not read or interpret file contents.

4. **Human-first escalation**  
   Inputs support awareness and alignment, not decision-making or enforcement.

---

## Allowed Input Categories

### 1. File Change Events

Represents meaningful changes made to files.

**Allowed fields**
- File ID
- File name
- File type (document, spreadsheet, presentation, other)
- Change type (edit, rename, move, delete, restore)
- Timestamp
- Actor role (not personal profile details)
- Change magnitude (minor, moderate, major; inferred heuristically)

**Notes**
- Semantic diffs are not required
- Magnitude is inferred from metadata, not content understanding

---

### 2. Comment and Annotation Threads

Represents discussion activity associated with files.

**Allowed fields**
- Thread ID
- File ID
- Comment count
- Presence of unresolved replies (yes / no)
- Presence of explicit questions (yes / no)
- Participant roles
- Timestamp of last activity

**Notes**
- The system detects structural signals only
- It does not summarize or interpret comment meaning

---

### 3. Mentions and Explicit References

Represents direct references within collaboration activity.

**Allowed fields**
- Mention type (user mention, file mention)
- Location (comment, description, metadata)
- Timestamp
- Target role (if available)

**Notes**
- Mentions indicate potential attention signals
- They do not automatically imply action or urgency

---

### 4. Sharing and Permission Changes

Represents access-related events that may alter scope or risk.

**Allowed fields**
- File or folder ID
- Permission change type (view, comment, edit, revoke)
- Scope (individual, group, public link)
- Timestamp
- Actor role

**Notes**
- Particularly relevant for Watchlist and Decision Pending classifications
- No assumptions are made about intent

---

### 5. File Metadata Context

Provides contextual grounding for other signals.

**Allowed fields**
- Last modified timestamp
- Number of collaborators
- Owner role
- Folder path or depth
- Starred or pinned status (if applicable)

**Notes**
- Metadata is used for contextual weighting only
- It is not used to infer productivity or performance

---

## Derived Signals (Allowed with Constraints)

The system may compute **derived signals** from allowed inputs, such as:
- Rapid activity bursts
- Prolonged unresolved discussion
- Repeated edits by multiple roles
- Permission changes following discussion spikes

**Constraints**
- Derived signals must be explainable in plain language
- No opaque scoring
- No probabilistic prediction
- No historical user profiling

---

## Explicitly Excluded Inputs

The AI Decision Signal Digest must never access or infer from:

- Full file contents
- Semantic meaning of documents
- User sentiment or emotion
- Productivity or performance metrics
- User behavior outside the current observation window
- External communication tools (email, chat, meetings)
- Long-term user history or profiling
- Any signals not listed in this document

These exclusions are intentional and non-negotiable.

---

## Observation Window

**Default window**
- Rolling 24 to 72 hours (configurable)

**Rules**
- Older signals decay naturally
- No persistent memory across digests
- Signals must reappear to be re-surfaced

---

## Input Integrity Rules

- Every surfaced item must reference at least one input with a timestamp
- Missing or incomplete inputs result in silence, not inference
- Conflicting signals trigger downgrade rather than escalation

---

## Rationale

This input schema ensures the system behaves as:
- A lens for awareness, not surveillance
- A decision-support aid, not a decision-maker
- A conservative assistant that earns trust over time

The goal is not to show everything.  
The goal is to surface **what matters, when it matters, and why**.

---

## Status

This schema is intentionally complete.

Any future expansion would require:
- Explicit versioning
- Updated guardrails
- Clear justification for added inputs

