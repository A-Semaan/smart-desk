# Smart Desk: integration and open decisions

## 1. Purpose and date

This document separates verified external facts from Smart Desk decisions that have not yet been made. Official OpenAI documentation was checked on **October 3, 2026**. Provider availability and preview behavior can change; recheck the linked sources before implementing the integration.

The following sections are not a claim that Smart Desk has already been registered, approved, connected, or validated on a mobile device. No provider integration has been implemented in this repository.

## 2. Confirmed subscription intent

The founder wants Smart Desk's AI capabilities to use the user's own ChatGPT subscription. The product is open source and has no Smart Desk operated backend. Its virtual file system, rendering, and reminders are local to the phone. The founder wants read-only connections to other apps, naming email as an example, so information from those apps can be used to do work in the local workspace. The external-app feasibility and constraints are detailed in [the feasibility study](06-limitations-and-external-apps.md).

These are product directions to preserve. This documentation does not substitute developer-paid API billing, require users to bring API keys, or introduce another model provider. Any such change would require the founder's explicit decision.

## 3. What Sign in with ChatGPT establishes

OpenAI documents Sign in with ChatGPT as supporting identity and optional ChatGPT plan usage. Eligible Plus and Pro users can authorize participating apps to make eligible AI requests through their plan. The open-source flow uses OAuth credentials for eligible Responses API calls without a partner API key or client secret. Commercial availability is described as limited to selected partners. [OpenAI quickstart](https://developers.openai.com/siwc/quickstart)

The plan-usage overview covers open-source and locally hosted applications and directs paid or remotely hosted offerings to an interest process. Smart Desk's no-backend design aligns with the local-app concept, but the documented OAuth flow uses a loopback callback and does not itself establish native iOS or Android support. Plan authorization does not grant access to the user's ChatGPT conversations or other account context. [Plan-usage overview](https://developers.openai.com/siwc/token-sharing-open-source), [registration and sign-in](https://developers.openai.com/siwc/token-sharing-open-source/sign-in)

Usage draws on the user's existing allowance; connecting an app does not create a separate allocation. Users can manage connected-app usage and access in ChatGPT settings. The integration also does not import ChatGPT memories. [ChatGPT user documentation](https://learn.chatgpt.com/docs/sign-in-with-chatgpt)

## 4. Current preview boundaries relevant to Smart Desk

The documented plan-usage route has specific Responses API constraints. HTTP calls require streaming, disabled response storage, and explicit input context. Persistent server-side conversation state must not be assumed. Function/custom tools are supported through documented forms, while web search remains dependent on model and account policy.

Hosted Code Interpreter, file search, image generation, native computer use, hosted MCP/connectors, and Responses tool search are listed as unsupported on this route. Audio/video inputs, the Files upload API, and transcription are also unsupported. These limits apply through Codex app-server as well as direct use of the route. [Preview limitations](https://developers.openai.com/siwc/token-sharing-open-source/preview-limitations)

This is a focused summary, not the complete request schema or an exhaustive integration recipe. Use the source for exact current parameters when implementing.

## 5. Implications for the intended product

**Implementation interpretation.** The model-access route and Smart Desk's own execution environment need separate designs. The product cannot assume that authorization of model usage supplies a complete hosted runtime for its generated scripts.

Similarly, the confirmed ability to talk to the app needs an actual speech implementation. The implementation might involve a device capability or another supported mechanism, but no mechanism is selected here. The voice requirement remains part of the vision while that technical decision stays open.

Internet collection also needs an actual available mechanism. The existence of model access is not evidence that a browser, authenticated connector, or arbitrary scraper is included. The runtime and tool configuration determine what the agent can fetch and process.

Smart Desk's saved workspace is its own responsibility. Subscription sign-in should not be treated as a source of preexisting personal context from ChatGPT or as a persistence layer for Smart Desk objects.

## 6. Mobile integration is not established yet

The reviewed sources establish the general provider route and its relevant limitations. They do not, by themselves, validate the complete proposed Smart Desk experience on a chosen phone platform. The documented open-source flow currently requires an HTTP loopback callback on `127.0.0.1`; its behavior across a mobile browser handoff and return to the app needs to be established before treating ChatGPT-plan access in a native phone app as solved. [OpenAI registration and sign-in](https://developers.openai.com/siwc/token-sharing-open-source/sign-in)

Once platforms are chosen, the implementation needs to establish the supported authorization flow, return-to-app behavior, credential handling, local execution model, and distribution requirements for that combination.

No mobile SDK, redirect arrangement, runtime, speech system, or application framework is selected in these documents. The product is primarily mobile; the exact supported operating systems remain open.

## 7. Open-source decision and repository status

**Confirmed intent.** Smart Desk will be an open-source product, and the founder requested a public GitHub repository named `smart-desk` under the authenticated account.

The source license has not been selected. This repository currently contains initial documentation and no license grant. Public repository visibility and the intended open-source direction should be recorded accurately without pretending a license decision has already been made.

The founder has selected a local phone workspace without an operated Smart Desk backend. Open-source distribution does not by itself settle the source license, mobile OAuth behavior, or app-store acceptance. Contribution governance, packaging, and installation instructions can be documented once the relevant decisions and implementation exist.

## 8. Resource and cost boundaries

The intended subscription pays for eligible AI usage according to the provider's actual rules. It should not be assumed to cover every resource Smart Desk might need.

The phone provides local execution, storage, rendering, and notification scheduling. Optional speech assets can consume device storage and need a download when first installed. ChatGPT inference and live provider reads use the network. External provider APIs have separate limits, and their cost rules need checking for the selected connection.

No business model, usage quota, or paid tier has been chosen. The founder's subscription-based AI intent and no-backend constraint remain unchanged.

## 9. Open-decision register

Every row below is unresolved unless the founder later answers it. Listing a choice does not authorize the implementer to choose an answer, add it as a feature, remove it, or defer it to a later release.

| ID | Decision | What remains to be established |
| --- | --- | --- |
| D01 | Phone platforms | iOS, Android, or both; any other platform support |
| D02 | Application technology | Native or cross-platform implementation and the actual stack |
| D03 | Local runtime design | How the phone executes the allowed workspace and rendering operations within platform rules |
| D04 | Runtime capabilities | Languages, packages, execution limits, filesystem boundaries, and network access |
| D05 | Physical workspace storage | How the confirmed local virtual file system of folders and Markdown content is persisted on the phone |
| D06 | Workspace exposure | How folders and Markdown files are presented, and whether supporting scripts or metadata are visible |
| D07 | Artifact rendering | Structured views, generated web content, rendered files, or a combination |
| D08 | Artifact authority | What generated views can do beyond presenting and locally filtering data |
| D09 | App-shell implementation | How the stable virtual-file-system viewer and renderer are built; AI does not rewrite the shell |
| D10 | Voice experience | Dictation, recorded requests, live voice, and the supported technical route |
| D11 | Plan integration | Eligibility and authorization flow for the chosen mobile and hosting arrangement |
| D12 | Internet access | Supported collection mechanisms, browser access, and authenticated source behavior |
| D13 | Optional scheduled agents | The founder did not originally request future autonomous AI jobs; whether to add them remains undecided |
| D14 | Local notification details | How scheduled offline reminders and in-app notices relate on each platform |
| D15 | Calendar relationships | Internal scheduling, external calendars, recurrence, conflicts, and timezone behavior |
| D16 | Task model | Exact list/item fields and supported direct interactions |
| D17 | User authority | Which operations need explicit confirmation and what grants persist |
| D18 | Data lifecycle | Local retention, deletion, recovery, and device-backup behavior |
| D19 | Device continuity | Whether any cross-device continuity is desired, without assuming a Smart Desk backend |
| D20 | Artifact lifecycle | Refresh, version exposure, export, sharing, or publishing behavior, if requested |
| D21 | Visual design | Navigation, typography, colors, components, motion, and concrete polish criteria |
| D22 | Source license | The specific license for the open-source product |
| D23 | Distribution | App packaging and distribution routes for the selected phone platforms |
| D24 | Resource ownership | Device storage and optional asset size, plus any outside provider API cost |
| D25 | Connected providers | Which external apps or services Smart Desk will support beyond the general integration intent |
| D26 | Connected reads | Which read and search operations Smart Desk needs for each provider |
| D27 | Connected account types | Whether each connection supports personal accounts, organizational accounts, or both |
| D28 | Connection authority | When the user grants read access and how Smart Desk presents use of outside data |
| D29 | Connected-data handling | Whether outside data is copied into the workspace, referenced remotely, or handled both ways |

The decision register records uncertainty. It is not a backlog of approved features and does not imply priority or implementation order.

## 10. Technical unknowns to resolve when authorized

The largest unresolved feasibility questions follow directly from the selected concept:

- Can the chosen mobile authorization flow access the user's eligible plan under the eventual distribution arrangement?
- How can the chosen local runtime execute permitted workspace operations with the required libraries and continuity?
- How will generated interactive content be displayed without receiving unrelated application authority?
- How will speech reach the agent while preserving the subscription intent and the selected operating constraints?
- What happens to ongoing work when the phone disconnects or the app is suspended?
- Which internet acquisition mechanisms are actually available to the chosen runtime?
- How will saved artifacts, scripts, and data remain consistent during follow-up edits?

These are questions for later authorized implementation work. No prototype, experiment, test suite, or release gate is added by listing them here.

## 11. Decisions that must not be inferred

An implementer must not infer that:

- The first release is only a scheduler or to-do app.
- A static image satisfies every requested interactive artifact.
- Local storage means ChatGPT inference and live provider reads also work without a network.
- Open-source distribution automatically settles licensing or hosting eligibility.
- The OpenClaw analogy requires using its source or adopting its architecture.
- A repository-like workspace means every user workspace is published to GitHub.
- ChatGPT plan access includes all OpenAI tools, arbitrary voice services, or unlimited usage.
- Offline timed reminders imply a continuously running agent or an operated server.
- A schedule entry automatically causes future AI work to run.
- Unresolved sharing, sync, collaboration, or publishing behavior is already in scope.

These boundaries preserve the founder's intent and prevent accidental scope decisions. They are not exclusions imposed on future requests.

## 12. Source register

| Source | Relevant use in this documentation | Checked |
| --- | --- | --- |
| [Sign in with ChatGPT quickstart](https://developers.openai.com/siwc/quickstart) | Eligibility, identity versus plan usage, open-source route | October 3, 2026 |
| [Plan-usage overview](https://developers.openai.com/siwc/token-sharing-open-source) | Hosting context and separation from ChatGPT account context | October 3, 2026 |
| [Registration and sign-in](https://developers.openai.com/siwc/token-sharing-open-source/sign-in) | Loopback callback required by the documented open-source flow | October 3, 2026 |
| [Preview limitations](https://developers.openai.com/siwc/token-sharing-open-source/preview-limitations) | Request behavior, tool support, and audio limitations | October 3, 2026 |
| [ChatGPT user documentation](https://learn.chatgpt.com/docs/sign-in-with-chatgpt) | Existing allowance, user controls, and no imported conversations or memories | October 3, 2026 |

All architecture proposals and product examples outside these sourced facts are interpretations of the founder's concept. They are not claims of provider support or evidence of an implemented integration.
