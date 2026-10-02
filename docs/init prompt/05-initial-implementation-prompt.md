# Smart Desk: initial implementation prompt

This is a reusable brief for a future implementation conversation. It preserves the founder's full intent and the boundaries of the current decisions. Its presence in this repository does not itself authorize building, selecting unresolved technologies, or changing scope.

The supporting documentation is part of the brief:

- [Product vision](01-product-vision.md)
- [Workspace and execution](02-workspace-and-execution.md)
- [Experience and behavior](03-experience-and-behavior.md)
- [Integration and open decisions](04-integration-and-open-decisions.md)
- [Limitations and external app feasibility](06-limitations-and-external-apps.md)

## Prompt begins

You are helping implement **Smart Desk**, an open-source app whose primary experience is on a phone.

Understand the entire intended product before proposing or making changes. Preserve the confirmed capabilities and distinguish them from architectural interpretations and unresolved decisions. The founder has not asked you to reduce the vision to an MVP or infer a release subset.

### Product intent

Smart Desk lets people organize their life and do useful work by talking to AI. The central analogy is the flexibility of an AI assistant that can work directly inside a laptop file system or repository, expressed through an exceptionally seamless mobile interface.

The app contains a persistent internal workspace that behaves like a repository. The AI can inspect and use saved material, create and modify scripts, execute code inside an appropriate sandbox, and display the outputs in the app. Work persists beyond a message and remains available for later requests.

The founder wants capabilities to be as flexible as the AI can be. The architecture must support AI-created implementations for tasks and outputs, rather than assume every useful behavior is a dedicated feature hardcoded by the developer.

The founder described the concept as a small OpenClaw with a seamless and exceptionally polished UI. Treat that as an analogy about agent capability and usability. Do not infer a dependency on OpenClaw, a requirement to copy it, or feature parity.

### Confirmed scope

Preserve all of the following:

1. Primarily mobile use.
2. The ability to open the app and talk to AI.
3. AI access through the user's own eligible ChatGPT subscription.
4. A persistent, repository-like internal workspace.
5. Sandboxed execution of code or scripts for requested work.
6. Broad capabilities enabled by AI-generated implementations.
7. Internet information collection or scraping through supported mechanisms.
8. Charts, tables, pages, and interactive tools displayed inside the app.
9. Conversational follow-up that modifies the same saved work.
10. A scheduler and a visually available schedule.
11. To-do lists that AI can create and manage.
12. In-app notifications.
13. An exceptionally seamless phone interface.
14. Open-source distribution, with the specific source license still to be selected.
15. Connections to other apps; email is the founder's example, while providers and specific actions remain undecided.

Scheduling, lists, and notifications are part of the broader workspace vision. Do not treat them as the limits of what Smart Desk can do.

### Representative behavior

A user says:

> "Collect these prices from the internet and graph how they compare."

The system uses its supported collection tools, saves the observations, creates the needed analysis or graphing implementation, executes it in the configured environment, and displays a useful artifact inside Smart Desk.

The user then says:

> "Group them by brand and let me filter by price."

The system identifies the existing artifact, reuses its saved inputs where sufficient, updates the implementation, and presents the changed interactive result. It preserves the connection between the chart, its data, and the work that generated it.

A user can also say:

> "Add grocery shopping to my list and schedule it for tomorrow at six."

The resulting task and schedule state must actually be saved and visible. If a necessary detail is ambiguous, obtain that detail. If an operation partially fails, represent the real outcome instead of claiming everything succeeded.

These examples demonstrate the general capability. Do not turn each example into an isolated hardcoded application or add unrelated functionality from it.

### Internal system model

Keep the responsibilities understandable: conversation and phone UI, provider connection, agent coordination, workspace persistence, sandbox execution, artifact rendering, and the selected schedule/notification behavior.

These are logical responsibilities, not a selected service topology. Runtime placement, storage, rendering technology, and application framework remain open.

Distinguish the development repository from end-user workspaces. Public source code does not make users' saved files or data public. A repo-like interior does not require a Git interface or GitHub synchronization for users.

Saved state should support continuity. A follow-up needs to identify the artifact or object being changed. The model's conversation is not a replacement for the underlying dataset, task list, or schedule records.

Generated implementations, execution outcomes, and rendered outputs are separate states. Do not report a successful result based only on the model having written code or requested a tool call.

### Artifact experience

Make the requested output usable inside the app. A generated chart should convey the requested comparison on a phone. An interactive filter needs actual supported behavior, not a static picture of a control.

Connect the current view to conversation context so ordinary references such as "change this" can refer to the intended saved work. Preserve the relationship between input data, transformations, and displayed meaning.

Treat generated content and collected internet text as inputs or executable content within defined boundaries. Do not implicitly grant them unrelated access to application credentials or privileged operations.

The application shell should make the dynamic work understandable. Global layout generation, technical file browsing, artifact sharing, export, and publishing are unresolved choices, not established requirements.

### Subscription integration

Use current official OpenAI documentation to establish the supported Sign in with ChatGPT route for the selected application and hosting model. Preserve the user's ChatGPT-plan intent; do not silently replace it with API-key billing or another provider.

The documentation checked on October 3, 2026 establishes relevant limitations. Read the linked integration document and recheck the provider sources before implementation. Do not assume that plan usage includes a hosted execution environment, the desired speech implementation, every tool, unlimited allowance, or access to the user's existing ChatGPT memories and conversations.

An open-source product direction does not settle every eligibility detail. Establish the actual mobile authorization and deployment combination instead of promising support based on the product label alone.

### External app connections

The founder wants Smart Desk to connect to other apps, with email as an example. Read the linked feasibility study. A service connection needs its own authorization and permitted operations; ChatGPT sign-in does not grant access to the user's email or other apps. OpenAI's current plan-usage route supports custom tool calls but not hosted MCP/connectors, so the Smart Desk application or its chosen runtime would need to provide supported connector operations.

Gmail and Microsoft Graph demonstrate documented email routes, with different scope, verification, and consent rules. The founder has not selected either provider, exact email actions, or the required account types. Do not infer universal access to installed apps or give generated scripts provider credentials by default. Resolve the material connection choices with the founder before implementing them.

### Scheduling and lifecycle

Keep a visible schedule, reminder delivery, and future autonomous job execution conceptually separate. In-app notifications are confirmed. Push notifications and arbitrary background AI jobs remain open.

Do not model the assistant as continuously awake between messages. Any future work needs an actual execution mechanism. Handle the effects of interruption according to the chosen environment and the state actually saved.

Time interpretation, recurrence, calendar connections, and direct task-editing behaviors need founder decisions where they materially affect implementation.

### Decisions and working agreements

Read the open-decision register before making architectural choices. No mobile platform, framework, database, chart library, sandbox vendor, agent framework, speech service, cloud provider, or source license has been selected.

Apply the founder's working agreements exactly:

- Do not provide product opinions unless the user explicitly asks for them.
- Do not add, modify, remove, or defer features, tasks, checks, tests, or scope decisions without an explicit user request.
- Do not infer a release subset, feature priority, or deferral from a larger specification.
- If the user's instructions conflict or a necessary decision is unclear, stop immediately and ask the user before making the change.

The conceptual schemas, example paths, and state labels in this documentation explain the idea; they are not approved implementation contracts. Record decisions when the founder makes them, and keep the documents consistent with those decisions.

### Intended outcome

The full intended product lets a person express what they want, have AI act within a persistent workspace, see and use the resulting changes or artifacts on their phone, and keep building on that work through conversation. The schedule, to-do lists, in-app notifications, open-source direction, and ChatGPT subscription model all remain part of that outcome.

Proceed only within the work authorized in the implementation conversation. Do not interpret this brief as permission to make unresolved product or architecture decisions independently.

## Prompt ends
