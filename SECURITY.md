# Security

## Reporting a vulnerability

**Do not open a normal issue.** The issue board is public, and an issue there is a public
disclosure before there is anything to update to.

Use **[GitHub's private advisory form](../../security/advisories/new)**, or contact the
maintainer directly.

Please include what an attacker could actually do, how to reproduce it, and the RUKUS version.
You will get an acknowledgement; if the report is valid you will be told when a fix ships,
and credited unless you would rather not be.

---

## What RUKUS is, from a security point of view

It is worth being plain about this, because it changes what "a vulnerability" means here.

**RUKUS runs on a machine that can write to live robot controllers.** It holds FTP and KCL
credentials for those controllers, and it can change values on them. Anything that lets code
or data from outside reach those capabilities is serious, even if it looks minor on a
desktop app.

### What it stores, and where

Everything is in `Documents\RUKUS\` on the local machine. There is no server, no account, and
no cloud component.

| | |
|---|---|
| `Clusters\` | Robot definitions — **including FTP and KCL passwords** |
| `Backups\` | Whatever you have backed up off the controllers |
| `WriteAuditLog.jsonl` | Every value RUKUS has written, with old and new values |
| `Logs\` | Application logs |

**Controller passwords are stored so RUKUS can reconnect without asking every time.** They
are protected by the file system's permissions and nothing stronger. Treat the `Clusters\`
folder as credential material: if the machine is shared, that matters.

The logs and the diagnostics bundle deliberately record **robot names and IP addresses but
never passwords**.

### What it talks to

| Destination | When | What is sent |
|---|---|---|
| **Your controllers** | Whenever you ask it to | FTP / KCL / HTTP on your own network |
| **api.github.com** | Startup, only if update checks are on | Nothing about you — an unauthenticated read of the public releases list |

That is the complete list. There is **no telemetry, no analytics, and no crash reporting
service**. Nothing about your cell leaves your network unless you attach a diagnostics bundle
to an issue yourself.

Update checking can be turned off entirely in **Settings → Updates**, after which RUKUS makes
no outbound connection except to your own robots.

---

## The beta is not code-signed

Installers are **not** signed with a code-signing certificate for the beta. Windows
SmartScreen will warn, correctly, that it does not recognise the publisher.

Instead, **every release publishes its installer's SHA-256** in the release notes, and the
[install guide](INSTALL.md#2-check-the-download-is-ours) shows how to check it. RUKUS's own
updater does the same check automatically and refuses to run anything that fails it, or any
release that publishes no hash at all.

This is a deliberate trade-off for a beta with a known audience, not an oversight. Code
signing is on the list for the first non-beta release.

**What this means for you:** the hash is the thing that tells you the installer is genuine.
Please actually check it, especially on a machine that reaches a live cell.

---

## Supported versions

During the beta, only the **most recent release** gets fixes. There are no long-term support
branches yet.

## Out of scope

These are known and accepted, not vulnerabilities to report:

- **Controller passwords are recoverable from `Clusters\` by anyone with access to the
  Windows account.** Documented above. Use Windows permissions.
- **SmartScreen warns on the installer.** Expected while unsigned — see above.
- **RUKUS can put a controller into a state you did not intend** if you tell it to. It asks
  first, shows a pre-flight, and logs everything, but it does not second-guess a deliberate
  instruction.
- **Anything requiring physical or administrative access to the machine RUKUS runs on.** If
  an attacker already has that, RUKUS is not the weak point.
