# Agent team

To build Mona's Project Pulse dashboard, I will use GitHub Copilot CLI in a Codespace to orchestrate this custom agent team:

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 4.7 (copilot) | Coordinates the workflow, delegates scoped work to specialists, manages dependencies, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository and relevant documentation, then creates an implementation plan with file assignments, dependencies, edge cases, and validation expectations. | `.github/agents/planner.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements the assigned application logic and support files, following repository patterns and validating the changes. | `.github/agents/coder.agent.md` |
| **Designer** | Gemini 3.1 Pro (copilot) | Shapes the dashboard's UI/UX, accessibility, information hierarchy, interactions, visual styling, and responsive behavior. | `.github/agents/designer.agent.md` |

The Orchestrator first asks the Planner for a repository-informed plan, then divides the work into scoped phases. The Designer defines the dashboard experience and visual direction, while the Coder implements the assigned application logic and supporting files. The Orchestrator sequences or runs independent tasks in parallel as appropriate, integrates the results, and verifies that Project Pulse works as a cohesive dashboard.
