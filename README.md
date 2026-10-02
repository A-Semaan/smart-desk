# Smart Desk

Smart Desk is an open-source, primarily mobile personal workspace managed through conversation with AI. It is intended to be highly modular: local workspace capabilities, external connectors, visualizers, and optional downloadable parts work together through the user's ChatGPT integration. Its workspace is a local, repository-like virtual file system of folders and Markdown content. The app provides stable tools to manipulate and visualize that workspace, including charts, tables, pages, and interactive results.

Schedules, to-do lists, in-app notifications, and connections to other apps are part of that vision. Email is the founder's example of an external connection. The broader ambition is to make the app's capabilities as flexible as the AI can be, with an exceptionally seamless phone experience.

The intended AI access model is the user's own ChatGPT subscription through the supported OpenAI integration. Smart Desk has no operated backend: local storage, rendering, speech support, and reminders belong on the device. AI requests and requests to connected outside services still need the network. Native mobile eligibility for the current OpenAI sign-in flow remains to be established.

## Project status

This repository currently contains the initial product and implementation documentation. No application, runtime, or integration has been implemented yet.

The product is intended to be open source, as confirmed by the founder. The specific license has not yet been selected; this repository does not currently grant rights through an open-source license.

## Initial documentation

Start with [the documentation index](docs/init%20prompt/README.md).

| Document | Purpose |
| --- | --- |
| [Product vision](docs/init%20prompt/01-product-vision.md) | The complete product intent, confirmed capabilities, and end-to-end scenarios |
| [Workspace and execution](docs/init%20prompt/02-workspace-and-execution.md) | Conceptual architecture, persistent data, sandbox execution, artifacts, and scheduling |
| [Modular system intent](docs/init%20prompt/07-modular-system.md) | How connected modules, optional downloads, and ChatGPT coordination fit the product vision |
| [Experience and behavior](docs/init%20prompt/03-experience-and-behavior.md) | How conversation, generated interfaces, and ongoing work fit together on a phone |
| [Integration and open decisions](docs/init%20prompt/04-integration-and-open-decisions.md) | Verified OpenAI constraints, unresolved choices, and technical questions |
| [Limitations and external app feasibility](docs/init%20prompt/06-limitations-and-external-apps.md) | Current platform limits and the feasibility of email and other app connections |
| [Initial implementation prompt](docs/init%20prompt/05-initial-implementation-prompt.md) | A reusable brief for a future implementation session |

These documents distinguish confirmed intent from explanatory implementation concepts and unresolved decisions. They do not select a technology stack, define a smaller release, or authorize additional features.
