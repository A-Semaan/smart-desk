# Smart Desk: workspace and execution

## 1. Status of this architecture

**Implementation interpretation.** This document explains the system responsibilities implied by the agreed product. It is a conceptual reference, not a selected technology stack or an instruction to start building. Names, paths, states, and interfaces below are illustrative. Decisions that affect deployment, scope, cost, or user authority remain open until resolved by the founder.

The confirmed foundation is a persistent, repository-like workspace, code execution in a sandbox, and outputs that Smart Desk can display and revise. The implementation must connect these capabilities to the phone experience and the user's ChatGPT plan.

## 2. Separate the responsibilities

The system can be understood as a set of logical responsibilities even if an eventual implementation combines several of them in one process:

| Responsibility | What it explains |
| --- | --- |
| Mobile application | Conversation, visible schedule, lists, notifications, and artifact presentation |
| Identity and plan connection | The user's supported ChatGPT sign-in and permission to use their plan |
| External connections | Separately authorized access to the specific outside apps the founder later selects |
| Agent coordination | Context selection, model requests, tool execution, and completion tracking |
| Workspace persistence | Durable objects, source material, generated code, and outputs |
| Execution environment | Running generated scripts within an appropriate boundary |
| Artifact presentation | Turning saved output into a usable view inside Smart Desk |
| Schedule and notice handling | Storing scheduled items and delivering the selected in-app behavior |

This separation is about responsibility, not a prescription for seven services. A concrete deployment could distribute the responsibilities differently depending on platform constraints and the founder's decisions.

## 3. The fundamental flow

```text
User request on the phone
        |
        v
Conversation + current workspace context
        |
        v
AI reasoning using the authorized ChatGPT plan
        |
        v
Available app operations and sandbox execution
        |
        v
Saved state, data, scripts, and artifacts
        |
        v
Phone interface displays the actual result
        |
        v
Follow-up request refers to the same saved work
```

The model interprets and plans; the application and runtime carry out operations. A model-produced request to run a tool is not evidence that the tool ran. A generated source file is not evidence that its output rendered. A completion message needs to reflect the state actually reached by those operations.

## 4. Three different environments

There are three environments that must not be confused:

1. **The Smart Desk development repository.** This GitHub repository contains application source and documentation.
2. **An end user's workspace.** This is the persistent collection of the user's data, scripts, schedules, lists, and generated outputs.
3. **A runtime used for execution.** This is where a particular script or operation runs, with the resources and permissions made available to it.

An end user's workspace need not be a public Git repository. Publishing Smart Desk's source does not imply publishing user workspaces. Likewise, a runtime can consume a bounded view of workspace data without receiving access to every file in the application.

## 5. Repository-like workspace

The workspace needs a coherent organization the AI can inspect and modify. That can include a file system, a virtual file system, structured records, or a combination. The conversation has not chosen the physical storage mechanism.

An illustrative logical layout is:

```text
workspace/
  workspace.json
  data/
    price-comparison.json
  scripts/
    build-price-comparison.*
  artifacts/
    price-comparison/
      manifest.json
      view.*
  organization/
    tasks.*
    schedule.*
  context/
    artifact-index.*
```

The wildcard extensions deliberately avoid choosing a language or database format. These paths illustrate the relationship between source material, generated implementation, visible output, and organization data. They are not a committed directory schema.

A storage implementation could represent tasks as database rows while exposing an appropriate workspace interface to the agent. It could also use files for some materials and structured storage for others. The important property is that operations resolve to consistent durable state.

## 6. Persistence and identity

The system needs a way to distinguish an object from the screen currently displaying it. A saved chart can be opened from a conversation and later from another part of the app. Those appearances should refer to the same underlying artifact when that is what the user intends.

Stable identifiers can explain this continuity. A title is useful to a person, but titles may change or be duplicated. A stable artifact reference can connect a conversation message, the dataset used by the artifact, its rendering implementation, and the view being displayed.

Changes also need a coherent publication boundary. If a script is being rewritten while an older chart is visible, the interface should not unknowingly combine the old script with a half-written new dataset. Possible approaches include transactional updates, immutable output versions, or staged publication. Selecting the mechanism is an implementation decision, not a decision made here.

Internal revision tracking is distinct from a user-facing history or restore feature. The latter has not been requested merely because internal consistency is necessary.

## 7. Conceptual data relationships

The following entities are a vocabulary for discussing implementation. They are not finalized tables or a requirement to create a service for each row.

| Entity | Meaning | Illustrative information |
| --- | --- | --- |
| Workspace | A durable boundary for saved work | Identifier, ownership reference, storage location |
| Conversation | A sequence of requests and responses | Messages, active artifact reference, associated run references |
| Run | One attempt to carry out a request | Request reference, status, tool outcomes, resulting object references |
| Dataset | Structured observations or input material | Records, schema, units, source references, collection time |
| Script | An implementation that can transform or render material | Entry point, runtime type, dependencies, expected inputs |
| Artifact | A result the app can display | Identifier, title, renderer type, data references, entry point |
| Task item | An item in a to-do list | Identifier, text, list relationship, selected task fields |
| Schedule item | A scheduled activity | Identifier, time representation, linked task if relevant |
| Notice | An in-app notification | Trigger relationship, content, display state |
| Connection | Authorized access to a provider | Provider identity and protected credential reference |

The schema should be driven by actual behavior once decisions are made. This conceptual list does not add user accounts beyond what is needed for the chosen connection and ownership model, multi-workspace navigation, collaboration, or arbitrary connectors.

## 8. Agent context

To act on a request, the agent needs the relevant workspace context. That can include the current conversation, the artifact being viewed, object references, selected files, tool descriptions, and the results of prior actions in the current run.

Passing the whole workspace into every model request is not inherent to the vision. The coordination layer can retrieve the material relevant to the current task. Exactly how that happens is open; there is no selected search engine, embedding store, or context-management framework.

The durable source of truth is the workspace state. A conversation summary can help the agent locate relevant work, but it must not silently replace the saved data. If the summary says a task exists and the actual operation failed, the stored outcome should control what is reported and shown.

References to a currently visible artifact are especially important on a phone, where requests such as "change this" are natural. The application can convey the active object reference to the agent rather than forcing the user to repeat a filename.

## 9. The execution cycle

A conceptual request cycle is:

1. Receive the user's instruction and its immediate interface context.
2. Resolve which saved objects and inputs are relevant.
3. Obtain model output using the authorized integration.
4. Interpret supported tool requests or generated implementations.
5. Execute those operations within the configured runtime and authority.
6. Record actual results, including partial outcomes or failures.
7. Persist the resulting state or artifact when appropriate.
8. Update the view and provide a conversational explanation of the outcome.
9. Preserve enough references for the next request to continue the work.

The cycle can repeat when the model needs an intermediate result before deciding the next operation. It is not a requirement to call a model repeatedly for ordinary deterministic view interactions.

## 10. Tools versus generated code

Some actions are naturally expressed as direct application operations: reading a saved list, changing a scheduled time, or opening a known artifact. Others require generated code: cleaning an unusual dataset, calculating a requested metric, or building a custom visualization.

The environment can expose both kinds of capability. Illustrative categories include workspace reads and writes, supported network collection, execution, and artifact publication. Exact tool names and parameter contracts remain undecided.

The tool boundary needs to keep proposed action and executed action separate. For example, a model may request an update with an invalid date. The runtime should return a concrete failure rather than fabricate a successful record. The next model response can then address the problem using the real tool result.

Generated code should use the runtime's available capabilities. Giving code an unrestricted host shell is a separate architecture and authority decision; the product's flexibility does not automatically settle it.

The founder also wants connections to other apps, with email as an example. A connector can present authorized operations to the agent through the app's tool boundary. The provider's credential and permission scope belong to the trusted connection layer, separate from ChatGPT authorization and ordinary workspace files. See [the external-app feasibility study](06-limitations-and-external-apps.md).

## 11. Sandbox responsibilities

The sandbox exists to provide a place for AI-generated implementations to run. Its concrete design affects what the AI can accomplish, how long work can continue, which libraries are available, and how outputs reach the phone.

The implementation discussion needs to resolve:

- Which languages and runtime versions are supported.
- How code receives input data and produces output.
- Whether and how dependencies can be installed.
- What filesystem paths a run can access.
- What network destinations and operations are available.
- How compute, memory, storage, and execution time are bounded.
- How a run is stopped or recovered after interruption.
- How credentials are kept outside generated code and display content.

These are design questions implied by executing code. Specific limits, isolation mechanisms, and permission prompts have not been chosen. They should not be presented as a completed security design.

## 12. Runtime placement remains open

There are materially different possible deployments:

| Placement | Consequences to evaluate |
| --- | --- |
| On the phone | Interaction with device resources, supported runtimes, lifecycle, storage, and platform distribution rules |
| On a user's own machine or server | Connection setup, reachability, authentication, execution continuity, and responsibility for the host |
| On a service operated for Smart Desk | Hosting cost, isolation, account architecture, operational ownership, and provider eligibility |
| A combination | State ownership, data movement, reconnect behavior, and which operations happen where |

No row has been selected. A phone-first experience does not prove all compute must occur on the phone, and open-source distribution does not decide where user workloads execute.

Current mobile operating-system and distribution constraints need platform-specific research once target platforms and runtime choices are proposed. This document deliberately avoids unsupported claims that arbitrary code execution or uninterrupted background work is available everywhere.

## 13. Artifact contract

A saved output needs enough information for Smart Desk to display it consistently. A conceptual artifact manifest could contain:

```json
{
  "id": "artifact_price_comparison",
  "title": "Price comparison",
  "kind": "interactive-view",
  "entrypoint": "view",
  "dataRefs": ["dataset_prices"],
  "runRef": "run_collect_prices",
  "revision": 1
}
```

This is explanatory pseudodata, not an API schema. A real contract would depend on the chosen renderer, storage model, and isolation boundary.

The contract needs to express what the result is, which data it depends on, and how to open it. A view should not need to rediscover its dataset by guessing filenames. It should also distinguish an output available for viewing from one still being generated or one that failed.

## 14. Rendering approaches

Several mechanisms could satisfy the accepted artifact concept:

- A structured specification interpreted by app components.
- Generated web content displayed within an isolated presentation surface.
- A rendered image or document for outputs that do not need interaction.
- A combination chosen according to the artifact's behavior.

These are options, not a selected stack or a narrowed artifact scope. A static image alone would not explain the accepted example of an interactive price filter. Conversely, a simple plot need not become a fully generated application if a supported renderer can express it.

The renderer needs a defined relationship with application state. An artifact filter might be a local view operation, while a button that changes a saved task would need an authorized operation on workspace state. The eventual interface between generated content and trusted app capabilities must make that distinction explicit.

## 15. Generated content and trusted app behavior

Generated views are part of the experience, but their code and imported content should be treated separately from the application components that hold credentials or control privileged operations.

For example, scraped page text is input data. It can contain misleading instructions. That text should not become a new source of authority over the agent or the app merely because it was included in a dataset or rendered page.

Similarly, an interactive chart does not need unrestricted access to authentication tokens in order to filter its dataset. The rendering architecture should expose the operations an artifact actually needs through a defined boundary.

The exact capability system, isolation technology, and confirmation policy remain open. These observations describe engineering implications of generated code and internet inputs; they do not add a product-level permission workflow without a founder decision.

## 16. Internet acquisition pipeline

The graphing and scraping scenario can be decomposed into a logical pipeline:

```text
Requested sources
  -> supported acquisition mechanism
  -> collected material
  -> extraction and normalization
  -> structured dataset
  -> analysis or graphing implementation
  -> visible artifact
```

Different stages can fail independently. A page may be reachable but not contain the requested field. Two observations may appear comparable but use different currencies or measurement units. A chart can render successfully even when its source data is incomplete.

Keeping source references and collection times associated with observations helps the system explain what it actually collected. Those are internal data relationships; the exact presentation of provenance to the user remains a design choice.

Browser automation, authenticated scraping, provider API access, refresh intervals, and recurring data collection have not been selected. They should not be assumed from a general request to support internet collection.

## 17. Schedule data versus job execution

A visible schedule needs persisted time information and a way for the app to display it. A reminder needs a mechanism that evaluates the relevant trigger and creates or delivers a notice. An autonomous job needs an execution host available at the requested time.

Those responsibilities can be related, but they are not interchangeable. A schedule item can exist even when no AI run is active. A reminder can be deterministic once configured. A future scraping job would require more than saving a calendar entry.

This distinction matters for subscriptions and mobile lifecycle. The system must not assume the model stays awake between messages or that saving a natural-language promise causes future execution. The founder has not yet decided whether arbitrary background AI jobs belong in the required behavior.

## 18. Consistency across lists, schedules, and views

The same user intention can touch several saved objects. Adding a to-do and scheduling it creates a relationship between task state and time state. An implementation must define whether those are one object with multiple representations or linked objects with separate identities.

Either way, partial completion must be representable. If the task is saved but the scheduling step fails, the application should not report both as complete. If a user retries after a lost connection, the system needs a way to determine whether a previous write already succeeded before blindly repeating it.

Idempotency, transaction boundaries, and conflict handling are possible implementation concepts for this problem. Their exact design is open. No multi-user or multi-device synchronization feature is being inferred from the need to avoid duplicate writes.

## 19. Run states and interruption

Illustrative run states include preparing, executing, awaiting needed input, completed, partially completed, failed, and interrupted. These labels explain the distinctions the system needs to communicate; they are not a finalized state machine or UI vocabulary.

Several interruptions can happen during ordinary phone use: a connection is lost, the app is closed, authentication expires, the runtime stops, or the model's allowance is exhausted. The resulting state should be based on what actually happened to the work.

If an artifact was already saved, a later conversational failure should not imply it disappeared. If code was written but never executed, the app should not label the desired output complete. Recovery behavior depends on runtime placement and persisted operation records.

Whether the user receives explicit pause, cancel, retry, or resume controls is a product decision still to be made. Internal handling of interruption should not be confused with approval of each possible control.

## 20. Credentials and ownership

The plan connection belongs to a particular authenticated user and authorization context. Workspace access and model usage should respect that ownership relationship in the eventual design.

Credentials are not ordinary workspace files for the agent to edit or generated artifacts to read. Their storage and use should be handled by the trusted integration layer using the selected platform's appropriate facilities.

Publishing the application source must not publish credentials or user content. The repository created for this project contains documentation only. This document does not assume an operator-hosted account system, a shared tenancy model, or a particular encryption implementation.

## 21. Runtime dependencies and reproducibility

A generated script can depend on a language version, library, font, or network resource. The implementation needs to know what is available when executing it. A result that worked once because of an accidental runtime state can be difficult to revise later if that state is lost.

Possible designs include a known runtime environment, recorded dependencies, or artifact-specific execution metadata. Choosing between those designs requires the runtime decision. The broad requirement is continuity of useful work, not a newly prescribed package manager or build system.

Network retrieval and generated implementation also have separate freshness questions. Reopening a chart could display saved observations; rerunning collection could produce new observations. The interface must not confuse those actions, and the founder must decide what refresh behavior is desired.

## 22. What remains unselected

No programming language, mobile framework, database, chart library, execution service, cloud vendor, agent framework, sync architecture, browser automation engine, or package installation policy has been chosen in the conversation.

This conceptual architecture preserves enough detail to evaluate those choices later. It does not use their absence as a reason to remove capabilities or quietly replace the general workspace with a fixed-function organizer.
