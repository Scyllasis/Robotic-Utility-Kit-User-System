# RUKUS — Robotic Utility Kit

**Back up, monitor, compare and edit FANUC robot controllers across a whole cell, from one
Windows PC.**

> ### 🧪 Public beta
>
> RUKUS is in **beta**. Get the installer from [Releases](../../releases), and read
> [INSTALL.md](INSTALL.md) first — particularly the checksum step and the SmartScreen
> warning, both of which you will hit, because beta builds are not code-signed.
>
> It talks to machines that move. [Known limitations](../../releases) are listed in full on
> each release, and they are the honest gaps rather than a formality.
>
> Found something wrong? The [issue board](../../issues) has templates for bugs, ideas and
> questions. **[Watch this repository](../../subscription)** to hear about new builds.

---

## What it is

RUKUS talks to FANUC controllers over FTP, KCL and HTTP and gives one operator a view of a
whole cell instead of one robot at a time. It is a plain Windows desktop app — no server, no
agent on the controller, nothing installed on the robot.

It is built for the jobs that are tedious one robot at a time:

| | |
|---|---|
| **Back up a whole cluster** | On a schedule, or on demand. Filters by file type, zips if you want. |
| **Find the odd one out** | Compare two robots, or check a cluster for drift against a reference. |
| **Answer "where is this used?"** | Search every program in a backup for a register, an I/O point or a position. |
| **Write to many at once** | Set the clock, set the speed override, set a register — across a cluster, with a pre-flight that shows you what would change before it changes. |
| **Watch it run** | Live I/O, alarms and production state, with a dashboard for a cell. |
| **See the points** | Plot taught positions in 3D, or in a flat 2D view where the distance on screen is the distance taught. |
| **Read what the robot holds** | System variables, I/O, registers, frames — read, and where it is safe, edit. |

There is a full [User Guide](USER-GUIDE.md).

## Requirements

- **Windows 10 (1809 or later) or Windows 11**, 64-bit
- Network access to your controllers (FTP, and HTTP for the live features)
- A **195 MB** download

Disk, first install:

| | |
|---|---|
| RUKUS itself | **312 MB** |
| Windows App Runtime | **166 MB** — a Microsoft component, installed once and shared with any other app that uses it. Already present on many machines; skipped if so. |

**Nothing to install first.** The .NET runtime is inside the app, and the Windows App
Runtime is carried inside the installer and put in place for you. There is no prerequisite
to download and no separate step — it is one `.exe`, and it works on a machine with nothing
on it.

## Getting it

1. **[Install guide](INSTALL.md)** — including what to do about the SmartScreen warning, and
   how to check the download is the file we published.
2. **[Update guide](UPDATING.md)** — how RUKUS tells you about new versions, and how to
   control that.

## Read this before you point it at a real cell

RUKUS **writes to live controllers**. Setting a speed override, editing a register or a
system variable, sending a file — these are real changes to a running machine.

- **This is a beta.** It has been exercised hard against virtual controllers and a real
  cell, but it has not been through a wide release. Treat it accordingly.
- **Everything it writes, it logs.** Every write goes to a `WriteAuditLog.jsonl` with the
  robot, the address, the old and new values, and whether it succeeded. Nothing writes
  silently.
- **Writes ask first.** Bulk operations run a read-only pre-flight and show you exactly what
  would change, on which robot, before anything is sent.
- **It is your cell.** Nothing here removes the need to know what a value does before you
  change it across twelve robots.

## Reporting a problem

Use the **[issue board](../../issues)**. There are templates for bugs, ideas and questions —
the bug one asks for the few things that make a report actionable (what you did, what
happened, the controller software version, and the diagnostics bundle).

**For a security problem, do not open a normal issue** — the board is public and an issue
there is a disclosure. See [SECURITY.md](SECURITY.md).

## Licence

**Free to use, free to pass on, not open source.** See [LICENSE.txt](LICENSE.txt) — it is
short and in plain English. The short version:

- Use it on as many machines as you like, including commercially in your own facility.
- Share the installer with anyone, as long as you pass it on **complete and unchanged** and
  charge nothing for it. Point people at [Releases](../../releases) where you can — that is
  the only place a build's checksum is published.
- No modifying, reverse-engineering, rebranding or selling it.
- It comes with **no warranty**, and the liability section is worth reading before you point
  it at a cell — see the paragraph below.

RUKUS bundles other people's software and fonts. Those are listed in
[THIRD-PARTY-NOTICES.txt](THIRD-PARTY-NOTICES.txt) and ship inside the app, each under its
own licence — and nothing in RUKUS's licence takes away rights those licences give you.

## What is not here

**The source code.** This repository is the guides, the releases and the issue board. RUKUS
is not open source; the code lives in a private repository.

---

## Not affiliated with FANUC

RUKUS is an independent tool. **FANUC**, **ROBOGUIDE**, **KAREL**, **SpotTool+** and
**HandlingTool** are trademarks of FANUC Corporation. This project is not endorsed by,
affiliated with, or supported by FANUC. Use it on your own equipment at your own risk.
