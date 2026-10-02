# Smart Desk: experience and behavior

## 1. Experience intent

**Confirmed intent.** The founder wants broad agent capabilities with a seamless, exceptionally polished interface, primarily on a phone. People should be able to ask for work and use the results without needing to operate a development environment.

**Implementation interpretation.** This document translates that ambition into interaction considerations and illustrative flows. It does not select a navigation design, commit to extra controls, or prescribe a final visual identity.

The experience has to connect three things: what the user asks for, what the system actually does, and what the user can see and use afterward. A polished conversation alone does not account for the persistent workspace and generated outputs in the accepted concept.

## 2. The relationship between conversation and work

The conversation expresses intent and explains outcomes. Saved workspace objects hold the actual schedule, tasks, datasets, code, and artifacts. The interface presents those objects in a form appropriate to the phone.

These parts need to remain connected. A user viewing a chart should be able to refer to that chart naturally in conversation. A user reading a completion message should have a clear path to the result it describes. The specific interaction that connects them remains a design decision.

Conversation history can explain why something was created, but the current object is what the app should display as current. An old response mentioning a previous scheduled time must not override the schedule after the user changes it.

## 3. Stable application, flexible content

The app can provide stable conventions for navigation, loading, errors, and viewing work while allowing the AI to create different kinds of content within those conventions.

For example, charts, tables, and generated pages may have different internal structures but still fit the same phone viewport and share understandable loading behavior. The user should not need to relearn basic navigation every time a new artifact appears.

The founder has clarified that the app shell remains stable: it manipulates and visualizes the local virtual file system. AI-generated workspace content can change without the AI rewriting the application's own functionality or global navigation.

## 4. Logical surfaces

The vision involves several logical surfaces. These are responsibilities the UI must express, not a decision to create a separate tab for each one.

| Surface | Role in the experience |
| --- | --- |
| Conversation | Capture the request, carry follow-up, and explain actual outcomes |
| Saved work | Make persistent results available again |
| Artifact view | Display and allow the supported interaction with a generated result |
| Schedule | Keep scheduled activities visually accessible |
| To-do list | Present and manage saved task items |
| In-app notices | Show reminders or relevant notifications within Smart Desk |
| Account connection | Explain and manage the selected ChatGPT connection experience |

Possible navigation structures include combined views, contextual panels, or separate screens. This document does not choose among them.

## 5. A user's first encounter

The product needs to communicate what a person can ask it to do: organize their information and create useful results inside the app. Technical terms such as sandbox, runtime, or repository can remain internal unless the person explicitly needs that information.

The ChatGPT connection should accurately distinguish signing in from granting plan usage. Any onboarding implementation must follow the applicable provider requirements once the integration is selected. The product should not imply that connecting imports all of the user's ChatGPT history or gives unlimited usage.

Whether a user can explore saved content before connecting, how account selection appears, and what the initial empty workspace looks like remain open. Those are experience decisions, not settled requirements.

## 6. Asking for work

The founder's desired interaction begins with opening the app and talking. The app captures the request and associates it with whatever context the person is currently using.

A user might ask for a new result, a change to something visible, or information about existing work. Those requests have different consequences. "Show my schedule" need not create a new schedule. "Move this to seven" needs to identify the existing item before changing it.

If the request lacks necessary information, the assistant should ask for that information in terms the user understands. The required clarification should concern the intended outcome, not an unnecessary demand for implementation details.

The conversation has not chosen the precise balance of speech and text, the microphone interaction, or a live voice session model. Those remain open without diminishing the confirmed ability to talk to the app.

## 7. While work is happening

The interface needs to distinguish understanding a request from performing it. A user asking for internet collection may wait while source pages are fetched, information is extracted, and a chart is prepared.

Progress communication can describe the operation in ordinary language, such as collecting the requested information or preparing the chart. It should reflect real run state rather than a decorative animation that implies certainty about unknown progress.

Exact progress indicators and controls remain design choices. The key implication is that an in-progress task cannot be presented as a finished artifact simply because the assistant has described its intended result.

Long work also raises lifecycle questions. If the user leaves the app, the outcome depends on where execution is hosted. The interface must match that actual behavior rather than assume the process continued.

## 8. Showing a completed result

A completed result can include a change to an existing object, a newly generated artifact, or both. The app should surface the saved outcome in a form that is useful on the phone.

For a schedule operation, this means the relevant schedule state is updated. For a graphing operation, it means a graph is available and connected to the data it represents. For a list operation, it means the requested list state exists.

The assistant's explanation can mention what changed and any material limitation. It should not substitute a long narrative for a usable chart or list when the request calls for that artifact.

The layout of result cards, the placement of conversational messages, and the transition into a dedicated view are not selected here.

## 9. Generated charts on a phone

A useful chart needs legible labels, meaningful units, and an arrangement suited to the available display area. Merely shrinking a desktop-sized plot can hide the information the user asked to understand.

The implementation needs to account for the actual data and chart structure. A comparison with many categories, long names, or widely varying values needs a presentation that preserves meaning. This does not predetermine a particular chart type or library.

When a user requests grouping by brand, the data transformation and the labels must reflect that grouping. When a user requests a price filter, its effect should be understandable from the visible result. A filter that changes the display should not silently overwrite the source observations.

Missing or unavailable observations should remain distinguishable from zero values. A chart can be visually complete while factually incomplete, so presentation must reflect the actual collection outcome.

## 10. Tables, pages, and interactive views

The accepted concept includes multiple forms of output. A table can support comparison, a page can organize a result, and an interactive view can respond to user input.

These are not all equivalent to an image. The renderer chosen for each type needs to support the behavior that the user requested. Conversely, an output with no interaction does not inherently need an unrestricted embedded application.

Generated content should fit the surrounding experience in sizing, typography, and interaction behavior. The exact design system remains undecided. The document establishes the relationship between app quality and artifact quality, not a visual specification.

Whether artifacts can be exported, shared, published, or installed elsewhere is not settled by displaying them inside Smart Desk. Those capabilities must not be added by inference.

## 11. Revising work through conversation

Follow-up is a central part of the accepted model. The user can refer to existing work and ask the AI to change it.

The implementation needs to identify the target object, understand the requested difference, and use the appropriate saved inputs. Some requests change only presentation. Others change calculations or require new data. The system should distinguish those cases so it does not repeat costly work or change unrelated content unnecessarily.

After the change, the visible artifact should represent the intended updated state. If the system could not complete the update, it should not erase a previously usable result merely to replace it with an unexplained failure.

The exact publication and recovery mechanics are architectural choices. User-facing revision history and undo controls have not been confirmed as separate features.

## 12. Schedule interaction

The schedule should remain visually available as a saved part of the app. The founder did not specify a day, week, month, timeline, or agenda layout. Those options remain design decisions.

Natural-language time references need a clear relationship to the user's intended date and timezone. "Tomorrow at six" is not fully determined without the relevant temporal context. The system should not hide material ambiguity behind a confident completion message.

Changing a task's scheduled time must update the actual saved representation. If a task and a schedule item are linked, the views need to remain consistent about that relationship.

External calendar integration, recurring schedules, automatic conflict resolution, and travel-related timezone adjustment remain unresolved. They are not implicitly authorized features.

## 13. To-do list interaction

To-do lists give a durable visual representation of work the user wants to track. Conversation can create and manage those lists and items.

The interface needs to make it clear which list an item belongs to and what the current saved state is. A conversational acknowledgment should correspond to a real update.

Direct editing, drag-and-drop ordering, nested tasks, labels, and richer project structures have not been chosen. The eventual design can answer those questions when the founder specifies the intended behavior. Their absence from the current decision set is not a decision to remove or defer them.

## 14. Notification interaction

In-app notifications are confirmed. A notice needs understandable content and a relationship to the event or work that caused it. A reminder about a scheduled activity should not point to a stale object if that activity has changed.

The exact notice presentation is open: an inbox, inline message, banner, badge, or another form could express it. The conversation also has not selected read-state behavior, categories, urgency levels, or delivery timing rules.

The founder wants timed reminders to work offline when the app is closed. The phone can deliver local notifications that Smart Desk has already scheduled. The operating system's notification permission and presentation settings still determine what the user sees. A newly arriving email cannot trigger an offline alert until Smart Desk has retrieved information about it while connected. [Apple local notifications](https://developer.apple.com/documentation/usernotifications/scheduling-a-notification-locally-from-your-app), [Android notification permission](https://developer.android.com/develop/ui/compose/notifications/notification-permission)

## 15. Errors and partial outcomes

A seamless interface includes clear handling of things that do not work. The user needs to understand whether a request produced no result, a partial result, or a usable saved result followed by a later failure.

Examples of materially different outcomes include:

- Some requested pages could not be collected, but a comparison of the available observations was produced.
- A dataset was saved, but the graphing implementation failed.
- A task was created, but its scheduled time could not be saved.
- The subscription connection stopped accepting requests before the work finished.
- An existing artifact can still be viewed, but a requested AI revision cannot currently run.

These examples explain truthful state communication. They do not prescribe extra user-facing recovery features or a test matrix. Specific retry, continuation, and correction behavior needs decisions appropriate to the selected architecture.

## 16. Account and usage behavior

The desired AI access model uses the person's existing ChatGPT subscription. The experience therefore needs to reflect the status of the authorized connection and the provider's actual usage allowance.

Connecting an account does not mean every service used by the app is paid for by that subscription. The local runtime and storage use device resources; external providers can have their own usage limits; optional downloaded speech assets use device storage.

The UI should avoid implying that a successful sign-in guarantees every capability. The integration document records the verified boundaries. A future implementation must present actual availability without silently replacing the founder's subscription-based intent with another billing model.

The founder also wants Smart Desk to read from other apps, with email as an example. Each outside account needs its own relevant authorization. In the interface, a request involving connected data should correspond to what that account actually permits and what Smart Desk actually retrieved. The exact providers and read workflows remain open; [the feasibility study](06-limitations-and-external-apps.md) records current constraints.

The founder wants this to be transparent and light. A connection experience should plainly explain its read scope, provider limits, and the fact that relevant outside content will be sent to ChatGPT when the user asks AI to work with it. Optional offline language and voice resources should have understandable download sizes and installed status. The exact screens and wording remain design choices.

## 17. Accessibility and phone interaction considerations

Readable type, usable touch targets, understandable focus behavior, and information that does not depend only on color are relevant considerations when translating the founder's polished mobile ambition into a design.

Dynamic content needs the same attention as the app shell. A generated chart's interaction should not become unusable solely because its labels are long or the phone is small. Voice capture should not obscure the result being discussed.

Specific accessibility standards, supported device sizes, orientation behavior, and performance budgets remain undecided. They require explicit decisions and suitable implementation work; this document does not create an unrequested compliance or testing program.

## 18. Technical detail in the user experience

The internal system may contain scripts, files, runs, manifests, and execution logs. Ordinary use should not require the user to manage those internals to understand a schedule or a graph.

The agent can explain limitations in terms of the requested outcome. For example, it can identify which source information was unavailable rather than showing an unprocessed exception as the only explanation.

Whether technical users can inspect files, source code, execution details, or runtime configuration is an open decision. The repository-like interior does not automatically require an IDE-style surface in the phone app.

## 19. The full experience remains the goal

The combination of conversation, persistent work, execution, in-app artifacts, scheduling, lists, notifications, and a polished mobile interface defines the documented ambition. The documentation does not convert that ambition into a fixed set of hardcoded productivity screens or select a smaller release.

Later design work should make the confirmed behaviors concrete while respecting unresolved decisions. Any change to the capabilities themselves belongs to the founder.
