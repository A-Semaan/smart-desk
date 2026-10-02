# Smart Desk: limitations and external app feasibility

Checked against official provider and platform documentation on **October 3, 2026**. The conclusions below are a feasibility study, not a provider approval, App Store ruling, selected implementation, or decision to integrate any named service beyond the founder's general request. Provider rules and preview capabilities may change.

## 1. What was added to the vision

**Confirmed intent:** Smart Desk should read from other apps so the AI can do work inside its own local workspace; the founder gave email as an example. The founder does not seek universal access, email management, or a Smart Desk operated server. The founder is willing to pursue verification and access approval where needed. The exact providers and read operations remain unselected.

The useful model is a connection to a particular service with a particular set of authorized reads. A person can ask the AI to use information from that account and create or update material in Smart Desk's local virtual file system. The account provider's API, permissions, policy, and availability set the real boundary. Live reads and ChatGPT inference require a network connection even though Smart Desk operates without its own backend.

## 2. Overall feasibility

| Area | Feasibility based on current documentation | Main limitation |
| --- | --- | --- |
| Read-only Gmail | API path exists | `gmail.readonly` is a restricted scope; public verification applies, and transferring mail content to OpenAI needs Google policy review. |
| Email through Outlook/Microsoft 365 | API path exists | Delegated permissions and organization consent policy control access. |
| Read-only external calendars | API or device routes exist for named providers | Authorization rules differ by provider and access level. |
| External files | API or user-selected file routes exist | The phone app cannot freely inspect every other app's private files. |
| Any arbitrary app | Depends on that app | No universal API, permission, or integration contract exists. |
| ChatGPT-plan agent with app-specific tools | Documented route exists for eligible usage | Hosted connectors/MCP are unavailable on the reviewed plan-usage route; Smart Desk must supply its own supported tool operations. |
| Local Markdown workspace and visualizations | Feasible in principle | A stable viewer and data renderer differ from changing native app functionality; generated executable content needs separate App Store review. |
| Offline timed reminders | Supported by mobile operating systems | Notification permission and Android alarm rules apply; autonomous AI runs are a different question. |
| ChatGPT-plan sign-in in a native phone app | Not yet established by the reviewed guide | The documented open-source OAuth flow requires an HTTP loopback callback and has no explicit mobile implementation guide. |

This table does not establish an implementation order or remove anything from the founder's vision. It says which parts have a documented path and which parts require further validation.

## 3. How an external connection could work

**Implementation interpretation:** Smart Desk could keep the AI, service connections, and workspace as separate responsibilities:

```text
User asks Smart Desk to use information from a connected app
        |
        v
Agent chooses a supported Smart Desk operation
        |
        v
Local connector checks account and granted read permission
        |
        v
Provider API performs a read
        |
        v
Result is used to create or change local Smart Desk workspace material
```

For example, a request concerning email would involve an email provider connection. The ChatGPT connection powers eligible AI requests; the email provider connection grants the separately authorized mailbox read. Those are separate sign-ins and permissions. OpenAI's plan-usage documentation explicitly says it does not supply the user's ChatGPT conversations or account context to the app. [OpenAI plan-usage overview](https://developers.openai.com/siwc/token-sharing-open-source)

OpenAI's current preview permits supported function/custom tools, so Smart Desk can expose its own defined operations to the model. The same preview lists hosted MCP/connectors, Code Interpreter, file search, and native computer use as unsupported on this route. The app's own connector or runtime would have to execute the requested operation and report its actual result. [OpenAI preview limitations](https://developers.openai.com/siwc/token-sharing-open-source/preview-limitations)

This is a conceptual architecture, not a decision to use a specific connector framework or to give generated scripts raw provider tokens. The connector and workspace remain on the phone; OpenAI and the outside provider remain online services.

## 4. Gmail: read-only is possible, but the scope remains restricted

The Gmail API can read mailboxes. The relevant `gmail.readonly` scope is classified by Google as **restricted** even though it cannot send, edit, or delete mail. `gmail.metadata` is also restricted and does not expose message bodies, so it does not satisfy every email-reading task. Read-only access reduces the actions Smart Desk can perform, but it does not change Google's classification of mailbox data. [Gmail scope documentation](https://developers.google.com/workspace/gmail/api/auth/scopes)

For a public app requesting restricted Google scopes, Google documents a verification process. There is no Smart Desk server to store mail. However, if the AI must reason over a message, that message or relevant excerpt is sent to OpenAI's service. Google says an app accessing restricted data from or through a third-party server needs an independent security assessment. Whether a particular Smart Desk flow falls under that requirement must be confirmed with Google during verification; the absence of Smart Desk's own server does not settle it. Google's limited personal/test exceptions do not establish approval for a generally distributed public product. [Google restricted-scope verification](https://developers.google.com/identity/protocols/oauth2/production-readiness/restricted-scope-verification)

Google also requires accurate disclosure and consent for how Workspace data is used, including transfers for a visible user-facing feature. Using email content as AI input therefore needs to be designed and represented accurately. Google's Limited Use policy also restricts unrelated reuse of the data. These are provider requirements, not a claim that user-directed AI handling of email is categorically forbidden. [Google Workspace user-data policy](https://developers.google.com/workspace/workspace-api-user-data-developer-policy)

Within the founder's local design, the phone could retrieve only the messages relevant to a request and send only the needed content to ChatGPT, with clear notice to the user. This is an implementation inference that reduces unnecessary transfer; it is not a substitute for Google's OAuth verification or a guarantee that Google would waive an assessment.

Gmail API reads are subject to quotas and limits. Google currently documents a daily project threshold and says charges for exceeding it are planned for later in 2026; the timing and billing details need rechecking before any cost estimate. This is separate from the person's ChatGPT-plan usage. [Gmail API usage limits](https://developers.google.com/workspace/gmail/api/reference/quota)

**Feasibility conclusion:** A local, read-only Gmail connector has a documented API path. The work is in Google's verification and data-transfer requirements, especially when the user asks ChatGPT to process email content. This is a review and implementation burden, not evidence that the integration is impossible. ChatGPT and Claude's own Gmail connections demonstrate that approved integrations exist; their approvals and credentials do not transfer to Smart Desk. [ChatGPT plugins](https://learn.chatgpt.com/docs/plugins), [Claude Workspace connectors](https://support.claude.com/en/articles/10166901-use-google-workspace-connectors)

## 5. Outlook and Microsoft 365: documented API path

Microsoft Graph documents reading a signed-in user's messages with delegated permissions. `Mail.ReadBasic` is the least-privileged permission listed for the message-list endpoint; reading the message body needs a fuller read permission. Smart Desk does not need a sending permission for the stated read-only integration. [Graph message listing](https://learn.microsoft.com/en-us/graph/api/user-list-messages), [Graph permissions](https://learn.microsoft.com/en-us/graph/permissions-reference)

For work or school accounts, the organization's consent policy can prevent a user from granting access to a new app. Some permissions or organization settings may require an administrator to approve the connection. Personal Microsoft accounts and organizational accounts therefore cannot be treated as identical deployment cases. [Microsoft consent overview](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/user-admin-consent-overview)

Microsoft Graph can throttle requests, returning a `429` response and a retry delay. A design that repeatedly scans mailboxes must account for provider limits. [Graph throttling guidance](https://learn.microsoft.com/en-us/graph/throttling)

**Feasibility conclusion:** Outlook and Microsoft 365 mail integration is technically supported through Graph, with account type, delegated scope, tenant policy, and rate limits determining actual availability.

## 6. Calendars and files: feasible examples, separate decisions

The founder asked for integration with other apps generally, with email as the specific example. Calendars and files illustrate what additional connections would involve; this study does not select them as required providers.

Google Calendar documents an API for accessing calendar information. Apple EventKit can access a device calendar with the relevant user permission. These are distinct routes with different permission and synchronization behavior. Smart Desk's own visual schedule is already part of the product even if no external calendar connection is chosen. [Google Calendar API overview](https://developers.google.com/workspace/calendar/api/guides/overview), [Apple EventKit access](https://developer.apple.com/documentation/eventkit/accessing-the-event-store)

Google Drive supports per-file authorization through `drive.file` and the Google Picker, which is classified as non-sensitive; broad `drive.readonly` is restricted. This makes user-selected file access materially different from reading an entire Drive. On the phone itself, Android and Apple confine apps' private storage. Smart Desk's virtual workspace can exist inside Smart Desk without access to every file on the device. [Drive scopes](https://developers.google.com/workspace/drive/api/guides/api-specific-auth), [Android app storage](https://developer.android.com/training/data-storage/app-specific), [Apple App Review 2.5.2](https://developer.apple.com/app-store/review/guidelines/)

## 7. The limitations that matter most

### 7.1 No universal access to other apps

A connection needs a supported API, device permission, file-sharing mechanism, or other documented interface from the specific app or service. Being installed on the same phone does not grant Smart Desk access to another app's private data. Provider access can be withdrawn, limited to certain scopes, or unavailable for a particular account. [Android app storage](https://developer.android.com/training/data-storage/app-specific), [Apple App Review guidelines](https://developer.apple.com/app-store/review/guidelines/)

### 7.2 The ChatGPT plan covers eligible AI use, not all capabilities

OpenAI's documented route supports eligible requests for Plus and Pro users, subject to the user's existing plan allowance and current preview rules. It does not automatically provide speech processing, generated-code execution, hosted connectors, internet browsing for every account, storage, or provider API access. Hosted Code Interpreter and audio input/transcription are currently listed as unsupported on that plan-usage route. [OpenAI quickstart](https://developers.openai.com/siwc/quickstart), [OpenAI preview limitations](https://developers.openai.com/siwc/token-sharing-open-source/preview-limitations)

### 7.3 The no-backend design still needs native mobile validation

OpenAI documents the plan-usage path for open-source and locally hosted apps. Smart Desk's workspace and execution are local, and there is no Smart Desk operated service to qualify. The remaining issue is whether the currently documented loopback sign-in flow can be made reliable in the selected native mobile platform. The open-source desktop example does not establish that result. [OpenAI plan-usage overview](https://developers.openai.com/siwc/token-sharing-open-source), [sign-in flow](https://developers.openai.com/siwc/token-sharing-open-source/sign-in)

### 7.4 The fixed viewer is a different case from downloaded native functionality

The founder clarified that Smart Desk itself is a fixed app for manipulating and visualizing a local virtual file system. Markdown files, folders, data, and declarative graph descriptions are workspace content. Apple's rule 2.5.2 restricts downloading or executing code that changes app functionality, while rule 4.7 expressly allows certain HTML5 and JavaScript content or plug-ins subject to additional conditions. Therefore the earlier claim that the whole concept faces a blanket iOS code-execution barrier was too broad. A fixed Markdown viewer and graph renderer have a plausible App Store route; any general script runtime or generated interactive mini-app still needs a concrete design checked against both rules. [Apple App Review rules 2.5.2 and 4.7](https://developer.apple.com/app-store/review/guidelines/)

The founder has ruled out running workspace code on a Smart Desk operated server. App Store acceptance for a particular local renderer or runtime remains a platform review question, not an established rejection.

### 7.5 Local reminders are feasible; autonomous AI jobs are separate

An iPhone app can schedule a local notification by time; iOS delivers it even when the app is not running. Android can schedule local alarms and notifications, with notification and exact-alarm permission rules on recent versions. No Smart Desk server or continuously awake model is needed for a reminder already scheduled on the phone. An AI agent that wakes later to reason, read the internet, or edit files is different: mobile background work is controlled by the operating system, and Apple does not guarantee a requested launch time. The founder did not originally require autonomous scheduled agents. [Apple local notifications](https://developer.apple.com/documentation/usernotifications/scheduling-a-notification-locally-from-your-app), [Android alarms](https://developer.android.com/develop/background-work/services/alarms), [Apple background task timing](https://developer.apple.com/documentation/backgroundtasks/bgtaskrequest/earliestbegindate)

### 7.6 Provider approval and data policy can govern an integration

Gmail mailbox reading is the clearest example: an API exists, but read-only scope is still restricted, and sending its content to OpenAI may affect Google's assessment requirements. Microsoft organization policies can block a connection or require administrator consent. For other apps, each provider's API, terms, and account controls need a separate check. [Google verification](https://developers.google.com/identity/protocols/oauth2/production-readiness/restricted-scope-verification), [Microsoft consent overview](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/user-admin-consent-overview)

### 7.7 Limits, latency, and ongoing costs are independent

The ChatGPT plan has its own allowance. External provider APIs can also throttle requests, and Gmail documents planned charges for usage beyond a project threshold later in 2026. Local scripts, workspaces, and optional speech models consume device resources. The app cannot infer that provider API use is included in a user's ChatGPT subscription. [ChatGPT plan usage](https://learn.chatgpt.com/docs/sign-in-with-chatgpt), [Gmail usage limits](https://developers.google.com/workspace/gmail/api/reference/quota), [Graph throttling](https://learn.microsoft.com/en-us/graph/throttling)

### 7.8 External content can affect agent behavior

Email, web pages, and connected documents can contain instructions written by someone other than the user. The implementation needs to treat that content as task data and keep provider credentials and privileged operations under the app's control. This is an architectural inference from allowing an agent to read untrusted outside content, not a newly chosen product feature or permission policy.

### 7.9 Offline speech has Google and open-source routes

The smallest download for Smart Desk is to use speech capabilities already installed on the phone when they satisfy the chosen language and offline requirement. Apple exposes on-device speech recognition and system-managed downloadable assets. Android can check for on-device recognition, request a language-model download, and select a text-to-speech voice that does not require a network. Availability and quality depend on the particular device, language, and installed speech service. [Apple on-device recognition](https://developer.apple.com/documentation/speech/sfspeechrecognizer/supportsondevicerecognition), [Apple speech assets](https://developer.apple.com/documentation/speech/assetinventory), [Android speech recognition](https://developer.android.com/reference/android/speech/SpeechRecognizer), [Android voice network flag](https://developer.android.com/reference/android/speech/tts/Voice)

Google's ML Kit GenAI Speech Recognition is a more specific **on-device Android** route. Google offers ML Kit at no per-request cost. Its speech API is still alpha: Basic mode uses the traditional on-device recognizer on most Android devices at API level 31 or higher; higher-quality Advanced mode currently supports Pixel 10 and Pixel 11, with other devices in development. The app can check model availability and download required assets. This Google route is proprietary and Android-specific, so it does not by itself provide an open-source, cross-platform speech engine. [ML Kit overview](https://developers.google.com/ml-kit/guides), [GenAI Speech Recognition](https://developers.google.com/ml-kit/genai/speech-recognition/android)

Google Cloud Speech-to-Text and Text-to-Speech are different: they are online services with free monthly usage allowances, billing beyond them, and Google Cloud credentials. Speech-to-Text V2 currently lists 60 free standard-recognition minutes per month per billing account; Text-to-Speech has voice-dependent free character allowances. A public client should not embed a shared project credential, and a shared free allowance would not become unlimited or per-user merely because Smart Desk is open source. A user-supplied Google Cloud project is a different, still-online arrangement and has not been requested as a feature. [Cloud Speech-to-Text pricing](https://cloud.google.com/speech-to-text/pricing), [Cloud Text-to-Speech pricing](https://cloud.google.com/text-to-speech/pricing), [Google API-key guidance](https://docs.cloud.google.com/docs/authentication/api-keys-best-practices)

Fully offline, open-source mobile options also exist. `Moonshine Voice` supports iOS and Android, streaming speech-to-text, optional model downloads, and languages including English, Arabic, Spanish, German, Japanese, Mandarin, and Vietnamese. Current streaming speech-to-text models are MIT-licensed; some older non-streaming language models have a non-commercial license and must be distinguished. `whisper.cpp` supplies multilingual speech-to-text models, with `tiny` listed at 75 MiB and `base` at 142 MiB. `sherpa-onnx` supports offline speech-to-text and text-to-speech on both mobile platforms. [Moonshine project](https://github.com/moonshine-ai/moonshine), [Moonshine model licenses and languages](https://moonshine-voice.readthedocs.io/en/latest/models/available-models/), [whisper.cpp model table](https://github.com/ggml-org/whisper.cpp/blob/master/models/README.md), [sherpa-onnx project](https://github.com/k2-fsa/sherpa-onnx)

For spoken replies, existing offline system voices are the no-extra-model option. Downloadable alternatives include lightweight Piper voices and the larger, Apache-licensed Kokoro-82M model. The actively maintained Piper engine is GPL-3.0, while individual voice licenses vary; that needs checking against Smart Desk's eventual license and selected voice. No model is established as the best across all phones, languages, accents, noise conditions, latency targets, and voice preferences. Device testing would determine which option actually meets the founder's quality and size aims. Local speech-to-text and text-to-speech can work fully offline; ChatGPT reasoning still requires a network connection. [Piper engine](https://github.com/OHF-Voice/piper1-gpl), [Kokoro](https://github.com/hexgrad/kokoro)

## 8. What remains undecided about connections

The founder still needs to define, when implementation depends on it:

- Which providers and categories are actually supported. Email is the example given; Google and Microsoft are feasibility cases, not selected launch integrations.
- Which read and search operations matter within each connected app.
- Whether accounts are personal, organizational, or both.
- Which actions require a user decision at the moment of execution.
- How provider credentials and imported data are stored on the phone.
- What happens when a provider revokes access, a quota is reached, or a connected app lacks an API for the requested action.
- Whether connected information is copied into the Smart Desk workspace, referenced remotely, or handled through a combination.

These are open decisions, not extra approved features or a proposed release sequence. The [decision register](04-integration-and-open-decisions.md) records them beside the broader platform and runtime questions.

## 9. Feasibility statement

**The clarified local-workspace concept has documented paths for its principal device functions.** Email has read-only routes through Gmail and Microsoft Graph. Local timed reminders are supported by the phone operating systems. The concrete uncertainties are Google's approval and data-transfer treatment for Gmail content sent to OpenAI, OpenAI's undocumented native-mobile loopback sign-in behavior, and App Store review of any chosen local script or interactive-content runtime.

Feasibility for a named provider and read operation can be established only after that provider, operation, account type, and phone platform are specified. This study does not imply that every app can be integrated or that native mobile ChatGPT-plan sign-in has already been validated.
