# Agent team

The custom agent team for building Mona's Project Pulse dashboard is:

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 4.7 (copilot) | Coordinates the team, breaks the work into phases, delegates tasks, manages dependencies, and verifies the integrated result. | [.github/agents/orchestrator.agent.md](../.github/agents/orchestrator.agent.md) |
| Planner | Claude Opus 4.7 (copilot) | Researches the repository, documentation, dependencies, risks, edge cases, and validation needs, then produces an implementation plan without writing code. | [.github/agents/planner.agent.md](../.github/agents/planner.agent.md) |
| Designer | Gemini 3.1 Pro (copilot) | Defines the dashboard's UI/UX, information hierarchy, accessibility, interaction flow, responsive behavior, and polished visual design. | [.github/agents/designer.agent.md](../.github/agents/designer.agent.md) |
| Coder | GPT-5.5 (copilot) | Implements the assigned code, keeps behavior explicit and testable, creates required runnable-app support, and validates the result. | [.github/agents/coder.agent.md](../.github/agents/coder.agent.md) |

I am using GitHub Copilot CLI in a Codespace to orchestrate the work. The Orchestrator starts with the Planner, then coordinates the Designer and Coder in phases with explicit file ownership and dependencies.
