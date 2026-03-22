# Methodology

The thinking behind Claude Forge, distilled from building and testing agent systems on real projects.

## Core Insight

The most valuable data for building effective agents isn't the codebase — it's the PR review history. Reading 40+ reviews teaches agents more about what actually gets accepted than reading every source file.

## Five Principles

### 1. Ground in Reality

Every agent prompt must contain patterns from the actual project, not generic best practices. A Go test-writer agent for Project A writes differently than one for Project B, because the projects use different assertion patterns, fixture styles, and conventions.

**How the forge does this:** It reads real source files and test files before writing any agent prompt, and includes exact code snippets as templates.

### 2. Model the Reviewer

The lead reviewer is the quality gate. Their patterns — what they always ask, what they care about, what they let slide — are the most important input to the system.

**How the forge does this:** It extracts comments from every PR the lead reviewer has touched, ranks patterns by frequency, and builds a challenger agent that simulates their review style using their actual words.

### 3. Verify, Don't Trust

AI agents are confident but often wrong. Research agents must prove their own findings before passing data downstream.

**How the forge does this:** Research agents are required to cross-reference from 2+ sources, manually compute algorithm proofs, and WebFetch every URL they cite. Unverified findings are flagged, not hidden.

### 4. Iterate Based on Testing

The first version of every agent is wrong. You discover this by testing on real work, not by reading the prompt.

**How the forge does this:** After creating agents, it offers a test run. Issues found during testing feed back into agent improvements. False positives in the challenger get added to "Things NOT to flag."

### 5. Human Checkpoints

Agents should never auto-submit work. The orchestrator pauses at critical decisions: after research (confirm findings), after building (confirm approach), and before submission (confirm readiness).

**How the forge does this:** The orchestrator template includes explicit pause points where the user must approve before the pipeline continues.

## The Agent Archetypes

Through testing, we've found that effective agent teams follow this pattern:

| Role | Purpose | Key Design Decision |
|------|---------|-------------------|
| **Researcher** | Gather verified information | Must self-verify with proofs |
| **Builder** | Create code matching project patterns | Must read reference code first |
| **Reviewer** | Check against project standards | Based on actual linter config and test conventions |
| **Challenger** | Simulate lead reviewer | Built from their real review comments, ranked by frequency |
| **Submitter** | Create polished deliverables | Matches format from real accepted PRs |
| **Orchestrator** | Chain everything with checkpoints | Human approval at critical gates |

## What Doesn't Work

Lessons from testing:

- **Generic agents** that don't reference the actual project produce generic output that fails review
- **Skipping PR analysis** means the challenger can't predict what the reviewer will ask
- **Ignoring false positives** wastes time — the challenger must know what NOT to flag
- **Auto-submitting** without human review creates trust issues
- **Single-source research** produces errors that propagate through the entire pipeline
