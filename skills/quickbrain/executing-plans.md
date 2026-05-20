---
name: quickbrain-executing-plans
description: Use when you have a written implementation plan to execute step-by-step with review checkpoints. Part of the quick_brain fork.
---

# quickbrain-executing-plans

> Part of the **quick_brain** fork. See `README.md` for context.

## Overview

Load plan, review critically, execute all tasks, report when complete.

**Announce at start:** "I'm using the quickbrain-executing-plans skill to implement this plan."

**Note:** Execution quality is significantly higher on a platform with subagent support. If subagents are available, prefer `@subagent-driven-development.md` (in this directory) instead of this skill.

## The Process

### Step 1: Load and Review Plan
1. Read plan file
2. Review critically — identify any questions or concerns about the plan
3. If concerns: raise them with the user before starting
4. If no concerns: create a TodoWrite list and proceed

### Step 2: Execute Tasks

For each task:
1. Mark as in_progress
2. Follow each step exactly (plan has bite-sized steps)
3. Run verifications as specified
4. Mark as completed

### Step 3: Complete Development

After all tasks complete and verified:
- Verify all tests pass
- Summarize what was implemented
- Confirm the work matches the plan (and therefore the original brainstorming discussion)

## When to Stop and Ask for Help

**STOP executing immediately when:**
- Hit a blocker (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**
- User updates the plan based on your feedback
- Fundamental approach needs rethinking

**Don't force through blockers** — stop and ask.

## Remember
- Review plan critically first
- Follow plan steps exactly
- Don't skip verifications
- Stop when blocked, don't guess
- Never start implementation on main/master branch without explicit user consent

## Integration

**Related skills in this fork:**
- `@writing-plans.md` — creates the plan this skill executes
- `@subagent-driven-development.md` — alternative execution mode using subagents (recommended when available)
