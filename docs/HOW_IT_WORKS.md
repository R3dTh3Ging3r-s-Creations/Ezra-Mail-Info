# How Ezra Mail works

[← Back to the project](../README.md)

This is an overview of the current v0.8.2 design, not a security certification or
approval to use Ezra with an organization's data.

## The installation

Ezra is a self-hosted email and calendar interface. It connects to accounts that
the user authorizes, stores synced copies in the installation, and presents them
in a browser. It does not replace Microsoft or Google as the mailbox provider.

The application uses Next.js and TypeScript, SQLite for local storage, and
Ollama for local model inference. Model processing runs where the configured
model service is hosted; that may be a different machine from the browser.
"Local" does not mean the entire application works without network access.

The intended setup is on a trusted private network. Direct public internet
exposure is not a supported deployment model.

## What the daily brief does

Today puts calendar commitments near the top, followed by a morning brief with
links to its source items. The morning snapshot stays in place; a separate
"Since your brief" section describes later developments.

Items needing attention show received and waiting ages. Earlier unfinished items
remain accessible, while informational items are separated into "Worth knowing."

**Mark handled records a decision in Ezra.** It does not itself reply, mark the
provider's email as read, or change an external task. Provider changes and sending
use their own review flows. AI-generated summaries and suggestions still need
human judgment and can be checked against the original items.

## Provider access

Microsoft uses delegated authorization: the user signs in with Microsoft and
grants access subject to their organization's rules. The current connection uses
a device-code sign-in flow and checks the returned mailbox identity before
saving the connection.

| Microsoft connection | Access requested |
| --- | --- |
| Initial mail setup | `offline_access`, `User.Read`, `Mail.Read`, `Mail.ReadWrite` |
| Adding calendar access | Mail access plus `Calendars.ReadWrite` |
| Enabling sending | Mail access plus `Mail.Send` |

The initial mail setup is **not read-only**. Sending requires a separate permission
and an exact-message review in the app. Google connections have their own provider
consent and permission requirements.

The project owner does not operate a shared hosted mailbox service. The person
running an installation is responsible for its access controls, configuration,
data retention, backups, and connected services.

## Optional notifications

Browser notifications, Web Push, and Telegram are optional. Quiet hours and
notification detail settings help control interruptions and what appears in alerts.

- **Web Push** uses the browser's push service to deliver encrypted payloads.
- **Telegram** sends the chosen notification content through Telegram's service.
  It is a bot integration, not SMS. Do not assume it has the same privacy properties
  as the local model or storage.
- Notification details such as sender and subject are a separate choice; generic
  notification copy is the default.

For workplace data, external notification channels should stay disabled unless
the organization explicitly approves their use and content.

## Work-account review

Before connecting a work account, ask whether personally developed, self-hosted
software is permitted. If an organization is willing to consider it, useful review
points include:

- Where the app, database, backups, and model service would run.
- Which mail and calendar permissions are needed and why.
- Whether the device and device-code authentication flow meet company policies.
- Which notification channels, if any, are acceptable.
- How access would be removed and retained data handled if a trial ends.

An app registration or a successful personal-account connection does not establish
organizational approval. Microsoft Conditional Access and consent policies still
apply. A blocked sign-in should be reviewed by the organization's IT team.

The source repository is private. Read-only access can be arranged with Eric for
review; this public repository does not grant access to the code or an installation.

## Development and current limits

Eric maintains Ezra as a personal project with help from AI coding tools. That
development assistance is separate from the local model used by the running app.

The current interface includes daily briefs, mail and calendar views, account
workspaces, draft and Outbox workflows, and notification settings. Broader testing
on real devices and installation environments is ongoing. Public screenshots use
fictional data and do not demonstrate completed testing on a physical device.

[Privacy policy](PRIVACY.md) · [Terms](TERMS.md) · [Back to Ezra Mail](../README.md)
