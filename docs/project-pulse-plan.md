# Project Pulse Dashboard Implementation Plan

## Summary and goal

Build a lightweight static **Project Pulse** dashboard for contributors. The
dashboard should make it easy to see which projects are active, who owns them,
their current status, recent activity, and their priority or risk level. It
should present a polished, readable, responsive card-based UI with accessible
markup, status badges, clear priority treatment, and a short
contributor-friendly summary.

The implementation remains dependency-light: the browser loads the static
HTML, CSS, and JSON files directly, and VS Code runs the app with Python's
built-in HTTP server. The final implementation surface is:

- `app/index.html`
- `app/styles.css`
- `app/project-data.json`
- `.vscode/launch.json`

## Ordered implementation steps

1. **Confirm the implementation contract.** Use the Project Pulse brief and
   the custom agent responsibilities as the source of truth. Agree on the
   `projects` data shape, the dashboard information hierarchy, the accessible
   markup approach, and the launch configuration before editing app files.
2. **Define representative project data.** Coder creates
   `app/project-data.json` with a top-level `projects` array. Every project
   object includes the required `name`, `owner`, `status`, `recentActivity`,
   and `priority` fields, with enough varied data to demonstrate active
   projects, statuses, and priority or risk treatments.
3. **Establish the page structure and rendering.** Coder creates
   `app/index.html` with the exact title `Project Pulse`, a contributor-facing
   heading and summary, accessible landmarks and headings, a dashboard
   container using the `.dashboard` hook, and a project-card template or
   rendering region. The page references `styles.css` and
   `project-data.json`, then renders a visible `.project-card` for each
   project. Each card exposes the project's name, owner, status,
   `recentActivity`, and `priority`.
4. **Apply the visual and responsive design.** Designer defines the visual
   hierarchy and accessibility decisions, then Coder implements them in
   `app/styles.css`. The stylesheet includes `.dashboard` and
   `.project-card` selectors, readable typography and spacing, visible status
   badges, clear priority styling, rounded corners, shadows, sufficient
   contrast, keyboard-friendly focus treatment where interactive elements
   exist, and a responsive layout for narrow and wide screens.
5. **Add the runnable preview configuration.** Coder creates
   `.vscode/launch.json` as strict JSON with no comments. It contains a
   configuration named `Run Project Pulse Dashboard`, runs exactly
   `python3 -m http.server 5500`, sets `cwd` to
   `${workspaceFolder}/app`, and uses `serverReadyAction` to open
   `http://localhost:%s/index.html` rather than the server directory root.
6. **Integrate and inspect the result.** Orchestrator reviews all four
   assigned files together, checking that HTML selectors and data fields match,
   that the page displays project cards rather than a directory listing, and
   that the launch configuration points at the same app directory.
7. **Validate and hand off.** Run the targeted file and runtime checks in the
   validation section below. Record any limitations or unresolved questions in
   the Orchestrator's final handoff; do not commit or push as part of this
   implementation plan.

## File assignments

| File | Owner | Scope and required outcome |
| --- | --- | --- |
| `app/index.html` | Coder, guided by Designer | Static page structure, exact `Project Pulse` title, stylesheet reference, JSON reference/loading, accessible dashboard region, visible project cards, and display of name, owner, status, recent activity, and priority. Include the `.dashboard` container and `project-card` class on each project card. |
| `app/styles.css` | Coder, using Designer's direction | Polished visual system and responsive layout. Include `.dashboard` and `.project-card`, status and priority treatments, readable spacing, `border-radius`, `box-shadow`, contrast, and responsive behavior. |
| `app/project-data.json` | Coder | Valid JSON with a top-level `projects` array. Every array item has `name`, `owner`, `status`, `recentActivity`, and `priority`; values are suitable for demonstrating the UI. |
| `.vscode/launch.json` | Coder | Strict JSON with no comments. Add `Run Project Pulse Dashboard` using `python3 -m http.server 5500`, `cwd: "${workspaceFolder}/app"`, and `serverReadyAction` whose `uriFormat` opens `http://localhost:%s/index.html`. |
| `docs/project-pulse-plan.md` | Planner/Orchestrator | This coordination plan only. App implementation agents must not modify it during the build phase unless the Orchestrator explicitly updates the plan. |

## Designer responsibilities

- Define a clear information hierarchy: page title and short summary first,
  followed by the project grid and scannable project cards.
- Specify visual treatments for status badges and priority or risk levels so
  they remain understandable through text and not color alone.
- Guide spacing, typography, card composition, rounded corners, shadows, and
  responsive breakpoints so the result is a polished dashboard rather than a
  bare HTML page.
- Review semantic structure, heading order, labels, contrast, focus states,
  readable text sizes, and behavior on narrow screens.
- Provide design direction to Coder without editing Coder-owned app files in
  the same implementation phase, avoiding conflicting changes.

## Coder responsibilities

- Implement only the assigned app and launch files, following the Designer's
  decisions and existing repository conventions.
- Build deterministic rendering from `app/project-data.json`; do not
  duplicate project values in markup when the data file is the source of
  truth.
- Ensure `app/index.html` references both `styles.css` and
  `project-data.json`, uses the exact title `Project Pulse`, renders visible
  cards with the `project-card` class, and shows all required project fields.
- Implement `.dashboard` and `.project-card` styling with responsive layout,
  status badges, priority treatment, `border-radius`, and `box-shadow`.
- Create `.vscode/launch.json` as strict JSON with no comments, using the
  exact launch name, command, working directory, and
  `serverReadyAction` URL requirements.
- Surface data-loading or parsing failures clearly in the page rather than
  silently showing a successful-looking empty dashboard.
- Validate the implementation before reporting it to the Orchestrator. Do not
  stage, commit, or push changes.

## Dependencies

- `app/index.html` depends on `app/styles.css` for the dashboard presentation
  and on `app/project-data.json` for the project records it renders.
- `app/styles.css` depends on the class hooks and semantic structure chosen in
  `app/index.html`, especially `.dashboard` and `.project-card`.
- `.vscode/launch.json` depends on the `app/` directory and its
  `index.html`; its working directory must make
  `http://localhost:5500/index.html` resolve to the dashboard.
- Browser-based JSON loading should be served over HTTP through the launch
  configuration. Opening `app/index.html` directly from the filesystem may
  trigger browser restrictions for `fetch`, so it is not the supported
  runtime path.
- No external framework, package installation, build step, or application
  backend is required. Python 3 and VS Code's launch support are the runtime
  prerequisites.

## Parallel work decisions

After the contract is agreed, Designer can work in parallel with Coder on
different concerns: Designer prepares the information architecture,
accessibility guidance, and visual decisions, while Coder can prepare the
JSON data contract and initial semantic page structure. Coder owns all actual
file edits for the four implementation files so that no two agents edit the
same file concurrently.

The Designer's work must be available before Coder finalizes styling and card
composition. Data-shape decisions must be available before Coder finalizes
the rendering logic. The Orchestrator should therefore treat parallel work as
parallel research and direction, not as permission for conflicting edits.

## Sequential execution

The work must proceed sequentially at these integration boundaries:

1. Planner and Orchestrator establish the requirements and explicit file
   ownership.
2. Designer supplies layout, accessibility, responsive, status, and priority
   direction; Coder confirms the data contract and implementation approach.
3. Coder implements `app/project-data.json`, `app/index.html`,
   `app/styles.css`, and `.vscode/launch.json`, reconciling the Designer's
   direction with the agreed selectors and schema.
4. Orchestrator inspects the integrated files and resolves any mismatch
   between data fields, markup, styling hooks, and launch behavior.
5. Runtime and static validation are completed before the dashboard is
   considered ready for handoff.

## Edge cases and risks

- **Empty or missing data:** A missing `projects` array, an empty array, or a
  failed JSON request must produce an explicit, readable error or empty-state
  message. It must not be mistaken for a successfully populated dashboard.
- **Malformed records:** Missing required fields should be handled
  predictably. The renderer should avoid producing broken labels or
  undefined-looking values and should surface invalid data during validation.
- **Unexpected text values:** Status and priority values may be new or
  differently cased. Text must remain readable even when a specialized
  visual class is unavailable; styling should not be the only way to convey
  meaning.
- **Long content:** Long project names, owner names, recent activity, or
  priority text must wrap without causing horizontal overflow or breaking the
  card grid.
- **Accessibility:** Status and priority must not rely on color alone.
  Maintain logical heading order, meaningful labels, sufficient contrast, and
  usable keyboard focus states for any interactive controls.
- **Responsive layout:** The card grid must remain usable on narrow mobile
  viewports and expand cleanly on larger screens without requiring horizontal
  scrolling.
- **Static-server behavior:** A server started from the repository root could
  open a directory listing, and a server started from the wrong directory
  could fail to find the assets. The launch `cwd` and URL must be checked
  together.
- **Port conflicts:** Port `5500` may already be occupied. Treat a failure to
  bind the port as an environment issue to report, not as a reason to change
  the required command or silently fall back to another port.
- **Launch schema compatibility:** `launch.json` must remain valid strict JSON,
  and the `serverReadyAction` pattern must match the Python server's ready
  output in the target VS Code environment.

## Validation expectations

Validate the exact requirements, not only that files exist:

- Confirm `app/index.html` has the exact document title `Project Pulse`.
- Confirm it references `styles.css` and `project-data.json`, contains a
  `.dashboard` hook, and renders visible cards with the `project-card` class.
- Confirm each rendered card exposes the project's `name`, `owner`, `status`,
  `recentActivity`, and `priority`.
- Parse `app/project-data.json` as JSON, confirm the top-level `projects`
  value is an array, and confirm every project object contains all five
  required fields.
- Confirm `app/styles.css` contains `.dashboard` and `.project-card`, plus
  `border-radius`, `box-shadow`, readable card styling, status and priority
  treatments, and responsive rules.
- Parse `.vscode/launch.json` as strict JSON and confirm it contains no
  comments, the exact name `Run Project Pulse Dashboard`, the command
  `python3 -m http.server 5500`, `cwd` set to
  `${workspaceFolder}/app`, and a `serverReadyAction` that opens
  `http://localhost:%s/index.html`.
- Run the launch configuration or an equivalent server from `app/`, request
  `/index.html`, and confirm the dashboard page and its referenced assets are
  served instead of a directory listing.
- Inspect the rendered result at narrow and wide viewport sizes for card
  wrapping, readable spacing, status and priority clarity, and absence of
  horizontal overflow. Check keyboard navigation and basic semantic
  accessibility.
- Stop the preview server after runtime validation and report any remaining
  environment-specific limitation.

## Open questions

- Should future versions add filtering or sorting, or is the initial
  deliverable intentionally read-only?
- Should project priority values use a fixed vocabulary such as `High`,
  `Medium`, and `Low`, or should the UI support arbitrary team-defined risk
  labels?
- Should the dashboard later include a separate summary metric area for
  active-project counts, or is the title and contributor-friendly summary
  sufficient for this static version?
- Which browser and VS Code platform should be the compatibility baseline if
  `serverReadyAction` output matching differs across environments?

## Assumptions

- This iteration is a small static frontend with no framework, bundler,
  package manager, API, or persistence layer.
- `app/project-data.json` is the single source of truth for the visible
  project records, and browser-side loading is acceptable.
- The initial data set contains multiple representative projects and uses
  human-readable values for every required field.
- A modern browser with JavaScript enabled is available for rendering data
  into cards.
- Python 3 is installed and available as `python3`.
- VS Code supports `serverReadyAction` and the `node-terminal`-style launch
  configuration used to run the HTTP server.
- The Orchestrator, Planner, Designer, and Coder follow the repository agent
  definitions in `.github/agents/`; no agent stages, commits, or pushes
  implementation changes.
