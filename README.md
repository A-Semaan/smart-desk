# Smart Desk

Smart Desk is an open-source, primarily mobile personal workspace managed through conversation with AI. The AI can work with a persistent, repository-like workspace, run scripts inside a sandbox, and turn the results into charts, tables, pages, and interactive tools that people can use inside the app.

Schedules, to-do lists, and in-app notifications are part of that vision. The broader ambition is to make the app's capabilities as flexible as the AI can be, with an exceptionally seamless phone experience.

The intended AI access model is the user's own ChatGPT subscription through the supported OpenAI integration. Eligibility, supported capabilities, and deployment arrangements still need to be established for this specific product.

## Project status

This repository currently contains the initial product and implementation documentation. No application, runtime, or integration has been implemented yet.

The product is intended to be open source, as confirmed by the founder. The specific license has not yet been selected; this repository does not currently grant rights through an open-source license.

## Initial documentation

Start with [the documentation index](docs/init%20prompt/README.md).

| Document | Purpose |
| --- | --- |
| [Product vision](docs/init%20prompt/01-product-vision.md) | The complete product intent, confirmed capabilities, and end-to-end scenarios |
| [Workspace and execution](docs/init%20prompt/02-workspace-and-execution.md) | Conceptual architecture, persistent data, sandbox execution, artifacts, and scheduling |
| [Experience and behavior](docs/init%20prompt/03-experience-and-behavior.md) | How conversation, generated interfaces, and ongoing work fit together on a phone |
| [Integration and open decisions](docs/init%20prompt/04-integration-and-open-decisions.md) | Verified OpenAI constraints, unresolved choices, and technical questions |
| [Initial implementation prompt](docs/init%20prompt/05-initial-implementation-prompt.md) | A reusable brief for a future implementation session |

These documents distinguish confirmed intent from explanatory implementation concepts and unresolved decisions. They do not select a technology stack, define a smaller release, or authorize additional features.
