# Smart Desk: limitations and external app feasibility

Checked against official provider and platform documentation on **October 3, 2026**. The conclusions below are a feasibility study, not a provider approval, App Store ruling, selected architecture, or decision to integrate any named service beyond the founder's general request. Provider rules and preview capabilities may change.

## 1. What was added to the vision

**Confirmed intent:** Smart Desk should integrate with other apps; the founder gave email as an example. The founder has not yet named required email providers, requested specific mailbox operations, or selected other services. “Whatever” describes the breadth sought, but it cannot mean automatic access to every installed app or online account.

The useful model is a connection to a particular service with a particular set of authorized actions. A person might ask the AI to use information from an authorized account, or to carry out an action in it. The account provider's API, permissions, policy, and availability set the real boundary.

## 2. Overall feasibility

| Area | Feasibility based on current documentation | Main limitation |
| --- | --- | --- |
| Email through Gmail | API path exists | Reading the mailbox uses restricted OAuth scopes and public distribution can require verification and assessment. |
| Email through Outlook/Microsoft 365 | API path exists | Delegated permissions and organization consent policy control access. |
| External calendars | API or device routes exist for named providers | Authorization and sync rules differ by provider and operation. |
| External files | API or user-selected file routes exist | The phone app cannot freely inspect every other app's private files. |
| Any arbitrary app | Depends on that app | No universal API, permission, or integration contract exists. |
| ChatGPT-plan agent with app-specific tools | Documented route exists for eligible usage | Hosted connectors/MCP are unavailable on the reviewed plan-usage route; Smart Desk must supply its own supported tool operations. |
| AI-generated code in an iPhone app | High distribution uncertainty | Apple's App Store rule 2.5.2 directly affects code that changes app functionality. |
| Guaranteed future work on the phone | Platform-dependent and limited | Mobile systems control background execution and timing. |

This table does not establish an implementation order or remove anything from the founder's vision. It says which parts have a documented path and which parts require further validation.

## 3. How an external connection could work

**Implementation interpretation:** Smart Desk could keep the AI, service connections, and workspace as separate responsibilities:

```text
User asks Smart Desk to work with a connected app
        |
        v
Agent chooses a supported Smart Desk operation
        |
        v
Trusted connector checks account and granted permission
        |
        v
Provider API performs a read or write
        |
        v
Result is saved or displayed in the Smart Desk workspace
```

For example, a request concerning email would involve an email provider connection. The ChatGPT connection powers eligible AI requests; the email provider connection grants access to that mailbox. Those are separate sign-ins and permissions. OpenAI's plan-usage documentation explicitly says it does not supply the user's ChatGPT conversations or account context to the app. [OpenAI plan-usage overview](https://developers.openai.com/siwc/token-sharing-open-source)

OpenAI's current preview permits supported function/custom tools, so Smart Desk can expose its own defined operations to the model. The same preview lists hosted MCP/connectors, Code Interpreter, file search, and native computer use as unsupported on this route. The app's own connector or runtime would have to execute the requested operation and report its actual result. [OpenAI preview limitations](https://developers.openai.com/siwc/token-sharing-open-source/preview-limitations)

This is a conceptual architecture, not a decision to use a specific connector framework or to give generated scripts raw provider tokens. The eventual authority boundary and action approval behavior remain open decisions.

## 4. Gmail: technically possible, significant access review

The Gmail API can access mailboxes and send messages. Its permission scopes differ by action. `gmail.send` supports sending and is listed as **sensitive**; `gmail.readonly`, `gmail.compose`, and `gmail.modify` are listed as **restricted**. The precise scope depends on whether Smart Desk will only send, read messages, create drafts, label messages, or perform another requested operation. [Gmail scope documentation](https://developers.google.com/workspace/gmail/api/auth/scopes)

For a public app requesting restricted Google scopes, Google documents a verification process. If restricted data is stored or transmitted through a third-party server, an independent security assessment may also be required. Google's documentation describes limited exceptions for personal or test use, but those exceptions do not establish approval for a generally distributed public product. [Google restricted-scope verification](https://developers.google.com/identity/protocols/oauth2/production-readiness/restricted-scope-verification)

Google also requires accurate disclosure and consent for how Workspace data is used, including transfers for a visible user-facing feature. Using email content as AI input therefore needs to be designed and represented accurately. Google's Limited Use policy also restricts unrelated reuse of the data. These are provider requirements, not a claim that user-directed AI handling of email is categorically forbidden. [Google Workspace user-data policy](https://developers.google.com/workspace/workspace-api-user-data-developer-policy)

Gmail API calls and sending are subject to quotas and limits. Google currently documents a daily project threshold and says charges for exceeding it are planned for later in 2026; the timing and billing details need rechecking before any cost estimate. This is separate from the person's ChatGPT-plan usage. [Gmail API usage limits](https://developers.google.com/workspace/gmail/api/reference/quota)

**Feasibility conclusion:** Gmail integration has a documented API path. The exact requested operations, OAuth scopes, data handling, public verification, and any server-side assessment must be resolved before promising a public integration.

## 5. Outlook and Microsoft 365: documented API path

Microsoft Graph documents reading a signed-in user's messages with delegated permissions. `Mail.ReadBasic` is the least-privileged permission listed for the message-list endpoint; fuller mailbox content and operations require other permissions. Microsoft separately documents `Mail.Send` for sending. [Graph message listing](https://learn.microsoft.com/en-us/graph/api/user-list-messages), [Graph permissions](https://learn.microsoft.com/en-us/graph/permissions-reference)

For work or school accounts, the organization's consent policy can prevent a user from granting access to a new app. Some permissions or organization settings may require an administrator to approve the connection. Personal Microsoft accounts and organizational accounts therefore cannot be treated as identical deployment cases. [Microsoft consent overview](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/user-admin-consent-overview)

Microsoft Graph can throttle requests, returning a `429` response and a retry delay. A design that repeatedly scans mailboxes must account for provider limits. [Graph throttling guidance](https://learn.microsoft.com/en-us/graph/throttling)

**Feasibility conclusion:** Outlook and Microsoft 365 mail integration is technically supported through Graph, with account type, delegated scope, tenant policy, and rate limits determining actual availability.

## 6. Calendars and files: feasible examples, separate decisions

The founder asked for integration with other apps generally, with email as the specific example. Calendars and files illustrate what additional connections would involve; this study does not select them as required providers.

Google Calendar documents an API for creating events. Apple EventKit documents different levels of access to a device calendar, including write-only and full access on recent iOS versions. These are distinct routes with different permission and synchronization behavior. Smart Desk's own visual schedule is already part of the product even if no external calendar connection is chosen. [Google Calendar event creation](https://developers.google.com/workspace/calendar/api/guides/create-events), [Apple EventKit access](https://developer.apple.com/documentation/eventkit/accessing-the-event-store)

Google Drive provides APIs to create and manage files. On the phone itself, Android gives an app its own storage and limits access to other apps' private storage; Apple also confines apps to their designated container except through supported access mechanisms. A repo-like Smart Desk workspace can exist inside Smart Desk without becoming unrestricted access to every file on the device. [Google Drive file guide](https://developers.google.com/workspace/drive/api/guides/create-file), [Android app storage](https://developer.android.com/training/data-storage/app-specific), [Apple App Review 2.5.2](https://developer.apple.com/app-store/review/guidelines/)

## 7. The limitations that matter most

### 7.1 No universal access to other apps

A connection needs a supported API, device permission, file-sharing mechanism, or other documented interface from the specific app or service. Being installed on the same phone does not grant Smart Desk access to another app's private data. Provider access can be withdrawn, limited to certain scopes, or unavailable for a particular account. [Android app storage](https://developer.android.com/training/data-storage/app-specific), [Apple App Review guidelines](https://developer.apple.com/app-store/review/guidelines/)

### 7.2 The ChatGPT plan covers eligible AI use, not all capabilities

OpenAI's documented route supports eligible requests for Plus and Pro users, subject to the user's existing plan allowance and current preview rules. It does not automatically provide speech processing, generated-code execution, hosted connectors, internet browsing for every account, storage, or provider API access. Hosted Code Interpreter and audio input/transcription are currently listed as unsupported on that plan-usage route. [OpenAI quickstart](https://developers.openai.com/siwc/quickstart), [OpenAI preview limitations](https://developers.openai.com/siwc/token-sharing-open-source/preview-limitations)

### 7.3 Open source does not settle hosted-service eligibility

OpenAI documents the plan-usage path for open-source and locally hosted apps and directs paid or remotely hosted offerings to a separate interest process. Smart Desk's source being public is confirmed, but the eventual execution host, service operation, and distribution details have not been chosen. Eligibility of that concrete configuration remains to be established. [OpenAI plan-usage overview](https://developers.openai.com/siwc/token-sharing-open-source)

### 7.4 AI-generated executable code is the largest iOS distribution question

Apple's App Store rule 2.5.2 says apps may not download, install, or execute code that introduces or changes their features or functionality, with a limited educational-app exception. Smart Desk's ambition to generate code or tools dynamically intersects this rule. The documentation alone does not establish whether a particular proposed runtime and presentation model would be accepted. It requires a concrete design and platform review. [Apple App Review 2.5.2](https://developer.apple.com/app-store/review/guidelines/)

Running code away from the phone and displaying its results may address some local execution constraints, but that is an architectural inference, not an App Store approval. It also introduces hosting, connection, security, and cost questions that the founder has not decided.

### 7.5 Background execution is controlled by the operating system

Apple says the system does not guarantee launching a background task at its earliest requested time. Android provides WorkManager for persistent work but also applies execution constraints and quotas. A visible schedule and in-app notifications remain feasible product goals; guaranteed long-running agent work at an exact future time is a separate deployment question. [Apple background-task timing](https://developer.apple.com/documentation/backgroundtasks/bgtaskrequest/earliestbegindate), [Android WorkManager guidance](https://developer.android.com/develop/background-work/background-tasks/persistent)

### 7.6 Provider approval and data policy can govern an integration

Gmail mailbox reading is the clearest example: an API exists, but restricted scopes can trigger verification and a security assessment, especially when data crosses a server. Microsoft organization policies can block a connection or require administrator consent. For other apps, each provider's API, terms, and account controls need a separate check. [Google verification](https://developers.google.com/identity/protocols/oauth2/production-readiness/restricted-scope-verification), [Microsoft consent overview](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/user-admin-consent-overview)

### 7.7 Limits, latency, and ongoing costs are independent

The ChatGPT plan has its own allowance. External provider APIs can also throttle requests, and Gmail documents planned charges for usage beyond a project threshold later in 2026. Running scripts, storing workspaces, and handling speech need actual resources. The app cannot infer that all of those are included in a user's ChatGPT subscription. [ChatGPT plan usage](https://learn.chatgpt.com/docs/sign-in-with-chatgpt), [Gmail usage limits](https://developers.google.com/workspace/gmail/api/reference/quota), [Graph throttling](https://learn.microsoft.com/en-us/graph/throttling)

### 7.8 External content can affect agent behavior

Email, web pages, and connected documents can contain instructions written by someone other than the user. The implementation needs to treat that content as task data and keep provider credentials and privileged operations under the app's control. This is an architectural inference from allowing an agent to read untrusted outside content, not a newly chosen product feature or permission policy.

## 8. What remains undecided about connections

The founder still needs to define, when implementation depends on it:

- Which providers and categories are actually supported. Email is the example given; Google and Microsoft are feasibility cases, not selected launch integrations.
- What actions matter within each connected app: reading, search, drafts, sending, updating, deletion, or something else.
- Whether accounts are personal, organizational, or both.
- Which actions require a user decision at the moment of execution.
- Where provider credentials, imported data, and generated scripts live in the eventual deployment.
- What happens when a provider revokes access, a quota is reached, or a connected app lacks an API for the requested action.
- Whether connected information is copied into the Smart Desk workspace, referenced remotely, or handled through a combination.

These are open decisions, not extra approved features or a proposed release sequence. The [decision register](04-integration-and-open-decisions.md) records them beside the broader platform and runtime questions.

## 9. Feasibility statement

**The general integration idea is technically feasible for services with suitable APIs and authorized accounts.** Email has documented routes through Gmail and Microsoft Graph. The hardest uncertainties for the complete Smart Desk vision are public Gmail access requirements, iPhone App Store treatment of dynamic code, the placement of the execution sandbox, and how the chosen mobile app will handle long or scheduled work.

Feasibility for a named provider and action can be established only after that provider, action, account type, deployment model, and platform are specified. This study does not imply that every app can be integrated or that the full proposed product has already been validated.
