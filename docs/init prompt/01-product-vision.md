# Smart Desk: product vision

## 1. The idea

**Confirmed intent.** Smart Desk is an open-source app, primarily for a phone, that lets someone organize their life through AI. The founder's starting comparison is the way an AI assistant on a laptop can work directly with a file system: it can inspect existing material, change things, create new files, and produce results that persist beyond a single message.

Smart Desk brings that style of assistance into a mobile environment. A person opens the app, talks to the AI, and asks it to do work. That work can affect their schedule, their to-do lists, stored information, or an output the AI creates for a specific request. The interface displays those results as useful objects the person can continue using.

The app contains a persistent internal workspace that behaves in some ways like a repository. The AI can have access to files, structured data, scripts, and outputs within that workspace. It can execute code in a sandbox and use the output to present something inside the app.

The founder's phrase, "the capabilities should be as flexible as the AI can be," is central. The product is not fully defined by a fixed inventory of task types. Its environment is meant to let the AI implement a useful response to requests that the application developer did not individually anticipate.

## 2. The founder's own framing

The following phrases preserve the important distinctions in the original discussion:

> "primarily your phone"

> "organize their life on their phone the same way they organize stuff on their laptop via AI"

> "managed purely with the ChatGPT subscription of the user"

> "the capabilities should be as flexible as the AI can be"

> "internally inside of the app kind of a sandbox or a file system that kind of behaves like a repo"

> "displayable in the app via some sort of graphing script"

> "a small OpenClaw with seamless and insanely flawless UI"

Spelling and repeated speech have been lightly normalized in these short excerpts. The OpenClaw comparison describes the desired breadth of an agent with an environment to act in. It does not settle the architecture or imply feature parity with that project.

## 3. The accepted interpretation

The founder explicitly accepted this description of the concept:

Smart Desk is a mobile workspace where AI can create and run tools, save its work, and display the results inside the app. Scheduling and to-do lists are examples of what it can do, rather than the boundaries of its capabilities.

Four ideas explain the accepted model:

1. **Persistence:** files, collected data, scripts, and generated results remain available across conversations.
2. **Execution:** the AI can write and run code inside an appropriate sandbox to carry out a request.
3. **Presentation:** outputs appear as charts, tables, pages, and interactive tools that can be used inside Smart Desk.
4. **Continuity:** follow-up instructions modify the same work and can reuse the information and implementation already created.

The developer provides the environment and the mechanisms for acting and displaying results. The AI uses those mechanisms to carry out the user's request.

## 4. What the person experiences

The person uses ordinary language to express an outcome. They do not need to choose a script file, name a database table, understand an execution container, or formulate a programming task.

They can ask for something to be organized, calculated, gathered, displayed, or changed. Smart Desk turns the request into actions within its authorized capabilities. A result can become a persistent part of the workspace, not merely a message describing what could be done.

The conversation and the visible result remain connected. If the user asks for a change to a chart they are viewing, the assistant needs enough context to know which chart is meant. If the user returns to a list later, the list needs to reflect the saved state rather than the content of an old conversational response.

The aspiration of a seamless interface includes the transitions between asking, waiting, viewing, interacting, and revising. The eventual visual style, navigation structure, and component system have not been selected.

## 5. Confirmed capability map

| ID | Capability | Meaning in the agreed vision |
| --- | --- | --- |
| C01 | Primarily mobile | The phone is the primary experience being imagined. Specific platforms remain undecided. |
| C02 | Conversational operation | Users can open the app and talk to AI to get work done. |
| C03 | User ChatGPT subscription | The intended source of AI access is the user's eligible ChatGPT plan. |
| C04 | Persistent workspace | The AI works with saved material in an internal environment resembling a repository. |
| C05 | Sandboxed execution | The environment supports running code or scripts for requested work. |
| C06 | Flexible capabilities | Capabilities can emerge from AI-generated implementations, within actual runtime and permission limits. |
| C07 | Internet data collection | Requests can involve collecting or scraping information from the internet. |
| C08 | In-app artifacts | Generated charts, tables, pages, and interactive tools are displayed and usable inside the app. |
| C09 | Follow-up modification | Users can revise existing work through conversation. |
| C10 | Scheduling | The app can manage a schedule and keep it visually available. |
| C11 | To-do lists | The app can create and manage task lists. |
| C12 | In-app notifications | Relevant reminders or notices can appear within the app. |
| C13 | Seamless interface | The experience should make broad agent capability easy to use on a phone. |
| C14 | Open-source product | The founder explicitly confirmed that Smart Desk will be open source. The specific license remains undecided. |
| C15 | External app connections | The founder wants integration with other apps and named email as an example. Specific providers and actions remain undecided. |

These identifiers are a traceability aid. They do not imply a build order, a priority, or separate products.

## 6. The workspace as the center of the product

The workspace is the durable context in which work happens. A conversation expresses intent; the workspace holds the things that intent creates or changes.

A request can have several related materials: original information, cleaned data, an analysis script, a rendered graph, and a saved configuration describing how that graph should be displayed. Keeping those materials connected allows a future request to build on the previous result.

For example, "make this graph weekly instead of daily" can modify the transformation and view while preserving the underlying observations. It does not need to repeat the collection step unless the user also requests updated source information or the existing data is insufficient.

The repository analogy implies an organized environment with related assets and durable state. It does not by itself require a Git interface for users, one Git repository per customer, commits for every operation, or synchronization to GitHub. The Git repository holding Smart Desk's source and documentation is a separate object from an eventual end user's workspace.

## 7. Flexible capability, explained precisely

There are two parts to the flexibility:

- The AI can choose a sequence of available actions to satisfy a request.
- The AI can create or modify code and presentation artifacts when the request requires an implementation that does not already exist.

For a graph request, the application need not contain a dedicated screen for that exact dataset and comparison. The AI can produce a dataset and rendering instructions or code that the app knows how to execute and display.

For a request to reorganize a list, a new script may be unnecessary. A direct operation on the saved list could be enough. Flexibility does not imply that every tap needs an AI request or that every task requires new code.

"As flexible as the AI" describes ambition, not unlimited authority or guaranteed access to every external system. The actual capabilities depend on the available runtime, network access, permissions, integrations, model support, and the information the user provides. Where a request exceeds those boundaries, the interface must communicate the actual state of the work.

The founder later added connections to other apps, naming email as an example. A connected app can contribute information or accept an action when that provider offers a supported interface and the user grants the relevant access. The exact apps and actions remain open. The separate [feasibility study](06-limitations-and-external-apps.md) explains the current limitations.

## 8. Conversation and speech

The founder wants to open the app and talk to it. Speech is part of the desired interaction, not an incidental detail. The exact speech experience has not been decided: dictation, recorded requests, live two-way conversation, or a combination would imply different implementation choices.

The accepted examples also describe conversational follow-up. The core intent is the continuity of the request and the work it affects, regardless of how the utterance is captured. A conversation can refer to a visible artifact, an earlier request, or existing workspace information.

The technical route for voice must be established separately from text-model access. A ChatGPT plan integration should not be assumed to grant every audio service. The documented integration constraints are recorded in the integration document rather than silently changing the voice requirement.

## 9. Scheduling

Smart Desk includes a scheduler and a schedule that stays visually accessible to the user. Users can ask the AI to create or change scheduled items through conversation.

The schedule is an operational part of the workspace. When the assistant says it has added an item, the item must exist in the relevant persisted state and appear in the schedule view. A chat response alone does not fulfill that behavior.

Scheduling has several meanings that need to remain distinct:

- Putting an event or activity on the user's visible schedule.
- Arranging a reminder or notice about that event.
- Scheduling the execution of an AI or script task in the future.

The first is confirmed, and in-app notifications are independently confirmed. Whether arbitrary AI work should run later without an active conversation is an open decision. The documentation does not equate having a scheduler with having unrestricted background automation.

Calendar-provider synchronization, recurring events, conflict handling, and timezone behavior require later decisions. They are discussed as unresolved implementation details rather than added features.

## 10. To-do lists

Smart Desk includes saved to-do lists that AI can create and manage. A user can express a task in ordinary language and have it become a real item in the workspace.

The same task may be mentioned in conversation, shown in a list, or associated with a schedule entry. The implementation needs a coherent way to refer to it so that changing one representation does not leave conflicting versions elsewhere.

The exact task model has not been chosen. Lists, completion state, dates, grouping, and other fields should be defined from the requested behaviors rather than expanded automatically into a separate project-management product.

## 11. In-app notifications

The founder explicitly asked for notifications inside the app. Those notices can communicate reminders or relevant information about the user's work, but the exact notification categories have not been selected.

The app's implementation needs to distinguish a notice being created from that notice actually being displayed or read. An AI sentence saying "I'll remind you" is not enough if no reminder mechanism was configured.

Operating-system push notifications, lock-screen alerts, and background delivery outside the app are separate decisions. The confirmed requirement remains visible; it is not replaced with a promise about mobile background behavior that has not been investigated.

## 12. Internet collection and scraping

The founder specifically described a user asking the AI to scrape material from the internet. In the envisioned flow, collected information can be saved and transformed into something useful inside Smart Desk.

An illustrative request is to collect comparable prices from specified pages and graph them. The work includes acquisition, interpretation, structured storage, transformation, and presentation. A user may then ask to change the grouping or filtering without repeating the original collection unnecessarily.

The exact collection capabilities are unresolved. Fetching a public page, executing a browser, using an official service API, and accessing an authenticated account are different mechanisms. This document does not select or guarantee all of them.

If a source cannot be read, the output needs to reflect that limitation. A partially collected comparison should not appear to include every requested source. Generated values must not be substituted for missing observations and presented as scraped facts.

## 13. Graphs, visual results, and interactive tools

The graphing example motivates a general artifact concept. An artifact is a saved result with a representation the app can display. It may be backed by a script, data, a structured view specification, or another supported format.

A graph needs its data and its interpretation to remain connected. Axis labels, units, aggregation, and filters affect what the person understands. A change in presentation should not quietly alter the meaning of the underlying measurements.

Tables and pages are additional presentation forms accepted in the clarification. Interactive tools extend this to outputs that respond to user input, such as filtering a dataset. These are examples of the general rendering capability, not authorization for an unlimited set of named specialized tools.

The claim motivating the product is about the founder's desired workflow. This specification does not assert that ChatGPT categorically cannot display graphs or artifacts. Smart Desk's intent is to make persistent, executable, revisable work central to this particular mobile experience.

## 14. End-to-end scenario: schedule and list

Illustrative request:

> "Add grocery shopping to my list and schedule it for tomorrow at six."

The assistant identifies the task and the intended time. If the meaning of six or the relevant date is unresolved, it obtains the information needed rather than silently committing an arbitrary interpretation. It writes the task and its schedule association through the workspace's supported operations.

The user sees the list item and the scheduled activity. The conversational response describes the result that actually occurred. If one operation fails, the response and visible state should make the partial result understandable.

A follow-up such as "move it to seven" refers to that saved activity. It should update the existing item rather than create an unrelated duplicate.

The interaction demonstrates conversation, persistence, scheduling, lists, and continuity. It does not determine the eventual screen layout or whether a user can also edit each field manually.

## 15. End-to-end scenario: collect and graph

Illustrative request:

> "Collect these prices from the internet and graph how they compare."

The assistant identifies the supplied sources and the comparison the user wants. It uses available collection mechanisms, records the observations, and organizes them into a dataset. Where records differ in units or contain missing values, those differences need to be accounted for in the analysis.

It creates an implementation for the requested graph, executes or prepares it through the sandbox, and places the resulting artifact in the workspace. Smart Desk shows the chart as a usable in-app result.

The output remains connected to its data and generated implementation. The user can return to it after the immediate response, and the assistant can use it as context for subsequent work.

## 16. End-to-end scenario: revise the existing result

Illustrative follow-up:

> "Group them by brand and let me filter by price."

The assistant identifies the active chart and checks whether its saved data includes the necessary brand and price information. If it does, it can change the transformation and interface using the existing material. If not, the missing information becomes explicit.

The updated artifact becomes the visible result. The filter changes which relevant observations are shown. The assistant should not present a new, unrelated chart as if it were the original artifact's updated state.

This scenario expresses the accepted capability to modify saved work and create an interactive presentation. It is not a decision about which chart library, frontend runtime, or programming language supplies the behavior.

## 17. Returning to previous work

Persistence means the user can leave and later return to the work. A schedule, list, or artifact should still correspond to the saved workspace state. Conversation history may help explain how it was created, but the durable object must not depend on the model remembering every previous token.

The exact navigation for finding previous work remains open. The product needs an understandable relationship between the current conversation and the saved things it refers to. Whether that relationship appears as a workspace view, artifact list, contextual panel, or another interaction is a design decision.

## 18. The interface aspiration

The founder wants an exceptionally seamless and polished UI. That applies to both the application around the work and the work generated within it.

A chart that appears embedded in the app but is unreadable on a phone would not express the intended experience. Likewise, an interface that repeatedly forces a user to inspect source code to understand an ordinary result would expose complexity the accepted concept intends to hide.

The visual language can be consistent while the contents remain dynamic. The flexibility concerns what the AI can create and do; it does not require every generated result to reinvent navigation or basic interaction conventions.

No particular branding, theme, typeface, navigation model, animation system, or component library has been chosen.

## 19. Observable meaning of success

The full idea is represented when a person can state an intended outcome, see the corresponding persisted changes or generated output, return to it, and revise it through conversation. The requested schedule, lists, and in-app notifications remain part of that picture, alongside general executable and visual work.

This is a description of the intended behavior, not a release gate or a newly prescribed test plan. A future implementation and its verification work require their own explicit authorization.

The scope remains the complete concept described here. This documentation does not nominate a simpler substitute, select a first release, or remove the difficult parts of the vision.
