# Smart Desk: modular system intent

## 1. Confirmed product direction

The founder has clarified that Smart Desk is meant to be **highly modular**. The product is a collection of connected parts, including many external-service connectors and downloadable parts, joined by the user's ChatGPT integration. This is a defining structure of the app, not merely a way to organize its source code.

The phone app still has a stable shell that manipulates and visualizes a local virtual file system of folders and Markdown content. Its workspace and app operations remain local, with no Smart Desk operated backend. ChatGPT provides the AI coordination for requests; an outside provider's API is used only when a separately authorized connection is needed. Offline-capable parts can continue to work locally without an AI request.

## 2. How the pieces relate

The conceptual relationship is:

```text
Person and stable phone interface
               |
               v
     ChatGPT-powered coordination
               |
               v
    Available Smart Desk modules
       /         |          \
  local       connectors   optional
 workspace    for reads   downloads
       \         |          /
               v
   Saved local workspace result
               |
               v
     Smart Desk visualization
```

This diagram is about roles, not a chosen process layout or requirement that every direct UI action call ChatGPT. The AI should know what the currently available parts can do and combine them for a user's request. The parts themselves perform the local operation, provider read, speech processing, or rendering within their actual permissions and platform limits. The agent then reports the result that actually occurred.

## 3. What "module" means here

At the product level, a module is a separately understandable capability that can connect to the rest of Smart Desk. The conversation establishes a modular direction, but it does not yet define a binary plug-in format, public extension API, package registry, executable-code loading mechanism, or marketplace.

The existing vision gives examples of parts that must fit together:

| Existing capability | Role in the modular picture |
| --- | --- |
| Local workspace | Stores the persistent Markdown and folder-based material that other parts read or change. |
| ChatGPT integration | Interprets the user's request and coordinates supported operations across available parts. |
| External connectors | Read from individually authorized services, such as an email provider, for work inside Smart Desk. |
| Execution capability | Runs supported scripts or operations needed to create requested results on the phone. |
| Visualizers | Present saved charts, tables, pages, and interactive results inside the stable app shell. |
| Schedule, tasks, and notices | Keep the organizer state and local reminder behavior connected to the workspace. |
| Language and voice resources | Supply optional offline speech capabilities according to user preference. |

This table does not declare that each row must be a separate downloadable package or that these are the only kinds of module. The founder specifically wants many connectors and downloadable parts; the exact catalogue and packaging remain to be defined.

## 4. Composition rather than isolated integrations

A connector is useful because its authorized read can feed a workspace task. For example, a person could ask Smart Desk to use information from an email, organize it in a local Markdown file, and display a graph. The email connection reads; the ChatGPT integration coordinates; the workspace stores the result; an available renderer displays it. The email account is not modified by this flow.

Likewise, an installed language resource could turn speech into text locally before the request reaches ChatGPT. A schedule operation could update local workspace state and register an offline reminder with the phone. These examples show the same parts working together without treating each combination as a separate hardcoded app.

The modular structure is meant to preserve flexibility as more supported parts are added. Whether a specific part can participate in a request depends on its installation state, the person's granted permissions, the phone platform, network availability, and its actual capability. A missing part should be represented as missing, rather than the assistant claiming it completed the operation.

## 5. Downloadable parts and a light app

The founder wants parts to be downloadable and the app to remain light and seamless. Language and voice resources are explicit examples; other downloadable module types have not yet been specified. A download is an installation choice for a supported capability, not a change to the AI model's authority or an automatic grant of access to an outside account.

The implementation will need a way to explain what a part enables, whether it is installed or available on the device, any relevant download size, and whether it requires an external account, internet access, or offline storage. This describes the transparency needed for the stated product; it does not choose a module manager UI, update policy, installation source, or default set of downloads.

Platform delivery differs for code and resource assets. The [feasibility study](06-limitations-and-external-apps.md) records documented Android on-demand feature delivery, Apple's downloadable asset packs, and the separate review question for downloaded executable functionality. Those paths do not select an app store or package system for Smart Desk.

## 6. Boundaries preserved by modularity

- A ChatGPT plan connection authorizes eligible AI usage. It does not automatically authorize a Gmail, Drive, or other service connection.
- A read-only connector does not gain write access because the AI can combine it with local workspace operations.
- Downloaded voice or language assets can work offline after installation; ChatGPT reasoning and live provider reads still require internet access.
- Modules do not rewrite the stable application shell merely because they provide new data, tools, or rendered results.
- A generated workspace artifact is user work. Whether it is itself an installable module has not been decided.

These boundaries keep the earlier confirmed local-workspace, read-only-integration, and no-backend decisions intact.

## 7. Decisions still open

The founder has selected the modular direction, but not its implementation contract. Before building a module system, the following need concrete decisions where they affect behavior:

- Which parts are built into the app, optional downloads, or supplied by the operating system.
- Whether only Smart Desk maintainers can supply modules or other developers can create and distribute them.
- How a module describes its capabilities to the ChatGPT coordinator and how modules exchange workspace references or results.
- How parts are installed, updated, disabled, removed, and kept compatible with saved work.
- How module permissions, provider credentials, and executable content are separated on each phone platform.
- What the first supported connector catalogue contains and what each connection may read.

No packaging format, registry, permission schema, runtime, first-release subset, or module catalogue is chosen by this document. The confirmed requirement is that Smart Desk's capabilities are assembled from connected modules under the ChatGPT-powered experience, rather than treated as one fixed monolith.
