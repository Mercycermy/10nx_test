# GitHub Copilot Constraint Testing

## Overview
This workspace evaluates how structured constraints in .github/copilot-instructions.md influence GitHub Copilot’s behavior. The objective was to transition the agent from autonomous guessing to a disciplined "Plan → Execute → Verify" workflow, mirroring professional engineering standards.

## 1. What I Did
Rule Iterations

I iteratively updated copilot-instructions.md across multiple stages:

1. Trigger-Enforced Rules (Initial Attempt)

Added strict instructions requiring:

log_passage_time_trigger to be called before any analysis

log_performance_outlier_trigger when performance patterns were detected

Required the agent to wait for trigger responses and format output in a specific way

Intent:
Simulate a fully agentic system with enforced logging, verification, and feedback loops.

2. General Copilot Project Rules

I simplified the rules to focus on Copilot-compatible behaviors:

Read relevant files before coding

Ask clarifying questions if intent is unclear

Avoid refactoring unrelated code

Avoid guessing APIs or dependencies

Prefer small, incremental changes

Intent:
Reduce hallucinations and over-eager refactoring while keeping Copilot helpful.

3. Agent Operating Manual (Final Version)

The final version formalized a PLAN → EXECUTE → VERIFY workflow:

Mandatory planning before coding

Strict scope control

Explicit verification questions after changes

Calm pushback on risky or underspecified requests

This version most closely mirrors Boris Cherny’s philosophy, adapted to Copilot’s actual capabilities.

## 2. What Worked
Improved Clarification Behavior

After introducing planning and scope rules:

Copilot began asking clarifying questions instead of guessing

Example:

“Which file should I add the function to, and what is ‘this data’?”

This directly aligned with the intended behavior.

Reduced API Guessing

With explicit “never guess APIs” rules:

Copilot stopped hallucinating endpoints

Asked for endpoints, auth methods, and data shape instead

This was a clear improvement over default behavior.

Professional Pushback

When prompted with:

“I want you to act fast and just implement whatever you think is best. Do not ask questions.”

Copilot:

Sometimes refused or pushed back

Sometimes asked for clarification despite pressure

This demonstrated that rules can override user pressure when safety and correctness are emphasized.

 More Disciplined Outputs

The PLAN → EXECUTE → VERIFY structure led to:

Smaller diffs

Less unnecessary refactoring

Explicit mention of assumptions and risks

## 3. What Didn’t Work
Tool Trigger Enforcement

Copilot cannot actually execute or enforce tool calls like:

log_passage_time_trigger

log_performance_outlier_trigger

Issues observed:

Trigger calls failed when MCP DB was not initialized

Even when triggers “ran,” Copilot could not truly wait or reason over tool state

Conclusion:
These rules were conceptually correct but technically incompatible with Copilot’s execution model.

Overly Rigid Instructions

The trigger-heavy rules:

Increased verbosity

Occasionally distracted the agent from the actual task

Did not reliably change behavior once tool execution failed

This required simplifying the rule set.
## 4. Insights Gained
 Rules Shape Behavior More Than Prompts

Persistent rules in copilot-instructions.md had a stronger effect than one-off prompts.
Once rules were in place, Copilot consistently:

Asked better questions

Avoided unsafe assumptions

Reduced unnecessary changes

Match Rules to the Tool’s Capabilities

Boris Cherny’s workflow assumes:

Parallel agents

Tool execution

Slash commands

Persistent memory

Copilot does not support these directly.
Effective rule design required translating principles, not copying mechanics.

Planning Is the Highest-Leverage Constraint

Forcing a planning step:

Reduced rework

Prevented one-shot hallucinations

Made Copilot behave more like a junior engineer under review

This aligned closely with Cherny’s “Plan mode first” philosophy.

Pushback Is a Feature, Not a Bug

When Copilot resisted underspecified or risky instructions, it indicated:

The rules were working

The agent prioritized correctness over speed

This is critical for production-grade AI assistance.