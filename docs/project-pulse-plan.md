# Project Pulse Implementation Plan

## Summary

Build a small, static Project Pulse dashboard that helps contributors scan project owners, status, recent activity, priority or risk, and a brief project summary. Follow the requirements in `.github/project-pulse-brief.md` and the workflow expectations in `.github/steps/3-step.md`.

The repository has an `app/` directory, but it contains no app files yet. `.vscode/` contains `tasks.json`, but no `launch.json`. The dashboard scaffold therefore needs to be created; no framework or existing app pattern is present to extend. Keep the implementation lightweight and static.

## Ordered implementation steps

### 1. Confirm requirements and agree on the design/data contract

- **Designer:** Recommend the information hierarchy, card layout, status and priority presentation, responsive behavior, and accessibility approach. Provide guidance without editing implementation files.
- **Coder:** Review the brief and this plan; agree with Designer on the dashboard’s content structure and the data fields the UI will display.
- **File assignments:** No files created or edited.
- **Dependency:** Complete this alignment before implementing the page and stylesheet to avoid mismatched markup and design.

### 2. Create the project data and preview configuration

- **Coder owns `app/project-data.json`:** Create valid JSON with a top-level `projects` array and multiple example projects. Every project must include `name`, `owner`, `status`, `recentActivity`, and `priority`. Include a short contributor-friendly `summary` if the UI will display it.
- **Coder owns `.vscode/launch.json`:** Create strict JSON with no comments. Add a configuration named **Run Project Pulse Dashboard** that serves the app from `${workspaceFolder}/app` with `python3 -m http.server 5500`, and opens `http://localhost:%s/index.html` through `serverReadyAction`.
- **Dependencies:** The required data fields and preview behavior come from the brief and Step 3 instructions. These two files do not depend on the final page styling or each other.
- **Parallel work:** This step can run in parallel with the Designer’s design guidance from Step 1, provided the Coder follows the agreed field contract.

### 3. Implement the dashboard page and styling

- **Coder owns `app/index.html`:** Create the page with the exact title **Project Pulse**. Reference `styles.css` and load `project-data.json`; render visible project cards using the `project-card` class. Show each project’s name, owner, status, recent activity, and priority, and display the summary if provided.
- **Coder owns `app/styles.css`:** Implement the Designer-approved visual direction, including `.dashboard` and `.project-card` selectors, readable spacing, responsive layout, status/priority affordances, `border-radius`, and `box-shadow`.
- **Designer responsibility:** Review the implemented experience against the design and accessibility guidance; report any changes needed to the Orchestrator. Designer does not edit these implementation files in this plan, so file ownership remains unambiguous.
- **Dependencies:** Start after Step 1’s design/data alignment and after the data contract in `app/project-data.json` is established. The page and stylesheet must use matching class names and work together.
- **Sequential work:** Complete the page and stylesheet integration before final validation. They may only be developed independently if the markup/class contract is agreed in advance and the Coder checks the integrated result.

### 4. Validate the integrated dashboard and preview

- **Coder:** Validate the JSON and launch configuration, inspect the page’s data-loading behavior, and run the preview configuration.
- **Designer:** Review the visible result for information hierarchy, responsive behavior, and accessibility.
- **Orchestrator:** Resolve findings, verify the four assigned outputs are present, and report any remaining limitations.
- **File assignments:** Validation only; edits, if needed, remain with the original file owner.

## File assignments

| File | Owner | Responsibilities |
| --- | --- | --- |
| `app/index.html` | Coder | Accessible page structure, exact **Project Pulse** title, stylesheet and JSON references, and visible project cards with all required project fields. |
| `app/styles.css` | Coder; guided by Designer | Responsive dashboard/card presentation; `.dashboard` and `.project-card` hooks; readable spacing and status/priority treatment; rounded corners and shadows. |
| `app/project-data.json` | Coder | Strictly valid JSON with a top-level `projects` array; each entry has `name`, `owner`, `status`, `recentActivity`, and `priority`; optional `summary` for contributor-friendly context. |
| `.vscode/launch.json` | Coder | Strict JSON; named **Run Project Pulse Dashboard**; serves from the `app/` working directory and opens `index.html`, not a directory listing. |
| `docs/project-pulse-plan.md` | Planner / Orchestrator | This implementation plan only. |

## Dependencies and parallel-vs-sequential decisions

- **Parallel:** Designer can prepare the visual and accessibility guidance while Coder creates `app/project-data.json` and `.vscode/launch.json`. These tasks have separate file ownership and no direct data dependency.
- **Sequential:** Agree on the data/display contract before completing `app/index.html`; the page depends on the JSON fields and the Designer’s guidance.
- **Sequential:** Integrate and review `app/index.html` and `app/styles.css` before final validation because their class names and responsive behavior must align.
- **Sequential:** Validate only after all four app/preview files are present. Fixes should be made by their assigned owner, then revalidated.

## Edge cases and risks

- Handle JSON fetch or parse failures with a clear in-page message rather than leaving a blank dashboard.
- Handle an empty or missing `projects` array with a useful empty state.
- Ensure missing or unexpected field values do not produce unreadable cards or JavaScript errors; required fields should still be populated in the sample data.
- Do not rely on color alone to convey status or priority; keep text labels visible and ensure sufficient contrast.
- Ensure the layout remains usable at narrow viewport widths and with keyboard navigation.
- The dashboard must be served over HTTP for JSON loading; opening `index.html` directly as a `file://` URL may not work.
- Confirm the Codespace supports the VS Code launch type/debugger used for the Python server. The repository does not currently define a launch configuration or document a debugger-specific launch schema.

## Validation expectations

- Confirm `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` exist.
- Run `python3 -m json.tool app/project-data.json` and `python3 -m json.tool .vscode/launch.json`; both must parse successfully.
- Check that the page has the exact title **Project Pulse**, references `styles.css` and `project-data.json`, renders `project-card` elements, and shows status, recent activity, and priority.
- Check that the stylesheet contains `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`, and that the layout adapts to narrow viewports.
- Run **Run Project Pulse Dashboard**. Confirm it serves from `app/`, opens `http://localhost:%s/index.html`, and displays the dashboard rather than a directory listing.
- Inspect the browser for failed data requests, console errors, missing content, and broken layout. Review keyboard usability, text legibility, and status/priority labels.

## Open questions

- Which example projects, owners, statuses, activity descriptions, priorities, and summaries should be used? The brief does not provide real project data; use clearly illustrative sample content unless Mona supplies specifics.
- Which exact VS Code Python launch type is supported in the target Codespace? Confirm compatibility when running the launch configuration.
