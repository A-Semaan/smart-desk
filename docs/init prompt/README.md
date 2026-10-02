# Smart Desk: initial documentation

Created: October 3, 2026.

This folder captures the initial Smart Desk concept from the founder's conversation and expands it into a comprehensive reference for design and implementation discussions. It is intended to preserve the idea accurately enough that a future designer or developer can understand the whole system without needing the original chat.

## Read in this order

1. [Product vision](01-product-vision.md): what Smart Desk is, what it enables, and what the founder has explicitly asked for.
2. [Workspace and execution](02-workspace-and-execution.md): how the pieces can relate internally, including storage, code, data, artifacts, and the agent's execution cycle.
3. [Experience and behavior](03-experience-and-behavior.md): how the system should feel to a person using it on a phone.
4. [Integration and open decisions](04-integration-and-open-decisions.md): current external facts, boundaries of those facts, and decisions that remain with the founder.
5. [Limitations and external app feasibility](06-limitations-and-external-apps.md): researched constraints and how email or other app connections could work.
6. [Initial implementation prompt](05-initial-implementation-prompt.md): a consolidated handoff brief for later work.

## Authority and terminology

The documentation uses three levels of certainty:

- **Confirmed intent:** a capability or direction the founder explicitly described, or explicitly accepted in the conversation.
- **Implementation interpretation:** a technical explanation of how the confirmed idea could operate. This is a reference model, not approval of a particular architecture, stack, additional feature, or implementation task.
- **Open decision:** a choice the conversation did not settle. It stays open until the founder decides; an implementer must not silently select an answer when the choice materially changes behavior, scope, hosting, or cost.

Examples explain the requested flexibility. They are not a list of separate apps or extra features that must be hardcoded. Candidate entity fields, file layouts, tool names, and state labels are illustrative rather than finalized interfaces.

This is a full-vision document. It does not introduce an MVP, assign feature priorities, defer capabilities, prescribe a release sequence, or create a test suite. Nothing here is evidence that a working application already exists.

## Founder instructions preserved

The working agreements supplied for this project are:

> Do not provide product opinions unless the user explicitly asks for them.
>
> Do not add, modify, remove, or defer features, tasks, checks, tests, or scope decisions without an explicit user request.
>
> Do not infer a release subset, feature priority, or deferral from a larger specification.
>
> If the user's instructions conflict or a necessary decision is unclear, stop immediately and ask the user before making the change.

The technical interpretations in these documents do not override those instructions. Documenting a consideration is not the same as selecting it for implementation.

## The concept in one paragraph

Smart Desk is an open-source personal workspace on a phone that people manage by talking to AI. It gives the AI a persistent, repository-like environment and an execution sandbox so that the AI can organize information, create scripts, collect internet data, connect to authorized external apps, and produce useful visual or interactive results inside the app. Schedules, to-do lists, and in-app notifications belong in this environment. The founder wants the capabilities to remain as flexible as the AI can be, powered through the user's ChatGPT subscription, with an exceptionally seamless interface that hides unnecessary technical complexity.

## Source boundaries

Product intent comes from the founder's statements and accepted clarification. External integration facts come from official provider and platform documentation, with links and the verification date recorded in [the integration document](04-integration-and-open-decisions.md) and [the feasibility study](06-limitations-and-external-apps.md). The comparison to OpenClaw is an analogy supplied by the founder, not a researched equivalence or a decision to adopt its code.

## Keeping this documentation accurate

When the founder later resolves an open decision, update the relevant passage and decision entry together. Do not turn an implementation interpretation into a confirmed requirement merely because it appears in a diagram, example, or prompt. Preserve the broad workspace concept when documenting later details, and record an explicit scope change only when the founder requests one.
