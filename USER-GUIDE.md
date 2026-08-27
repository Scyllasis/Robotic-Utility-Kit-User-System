# RUKUS User Guide

**Status:** Updated 2026-08-25 for the beta-feedback pass. **Every menu path in this guide was
re-checked against `MainWindow.xaml` on 2026-08-25 and five were wrong** - Smart Scan, Bulk
Operations and the KCL Console had all moved to **Tools**, and the Error Code Lookup lives under
**System**, not Monitor. The rest was last verified 2026-08-16 against the running app.

> **Verified against the app 2026-08-23.** Every menu path, the robot right-click list, and the
> Watcher-versus-Editor naming were checked against `MainWindow.xaml` rather than remembered.

> **What changed on 2026-08-25.** One window per thing (see
> [One window per thing](#one-window-per-thing)); the Error Code Lookup is a native window rather
> than an embedded browser page; downloading from a robot no longer re-lists the whole device
> first; the Robot Report pulls only the files the ticked sections read; the Write Audit Log now
> records what a bulk write changed *from*; and dialogs follow the skin. Three windows that had
> shipped without a section here now have one: [Check Trends](#check-trends),
> [Point Analysis](#point-analysis) and the
> [KAREL .PC Inspector](#karel-pc-inspector).


A step-by-step guide to using RUKUS for managing your FANUC robot controllers.

> The app also has a built-in guide: **System → User Guide**, kept in sync with the
> current screens.
>
> **Beta UI changes (2026-07):** the KCL command box moved out of the Production
> Dashboard into its own **Tools → KCL Console**. Robot/device/status/alarm lists are
> now grids of **cards** (click anywhere on a card to select it) rather than tables,
> and every window shares one card look. Descriptions below still apply; a few
> screenshots may pre-date the card styling.

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Finding Things Fast](#finding-things-fast)
3. [Managing Clusters](#managing-clusters)
4. [Smart Scan](#smart-scan)
5. [Backing Up Robots](#backing-up-robots)
6. [Bulk Operations](#bulk-operations)
7. [Comparing Robots](#comparing-robots)
8. [Where Used](#where-used)
9. [Robot Timeline](#robot-timeline)
10. [Production Dashboard](#production-dashboard)
11. [I/O Editor](#io-editor)
12. [Data Editor](#data-editor)
13. [I/O and Data Watcher](#io-and-data-watcher)
14. [System Variables Browser](#system-variables-browser)
15. [Robot Position Viewer](#robot-position-viewer)
16. [Error Log Viewer](#error-log-viewer)
17. [Error Watcher](#error-watcher)
18. [Error Code Lookup](#error-code-lookup)
19. [Signal Scope](#signal-scope)
20. [Robot Info](#robot-info)
21. [File Control](#file-control)
22. [File Generator](#file-generator)
23. [KCL Quick Commands](#kcl-quick-commands)
24. [Alarm Collectors](#alarm-collectors)
25. [Scheduled Tasks](#scheduled-tasks)
26. [Write Audit Log](#write-audit-log)
27. [Settings](#settings)
28. [Everyday Conveniences](#everyday-conveniences)
29. [Reporting Problems](#reporting-problems)
30. [File Locations](#file-locations)
31. [Position Plotter](#position-plotter)
32. [Point Analysis](#point-analysis)
33. [Robot Report](#robot-report)
34. [Check Trends](#check-trends)
35. [KAREL .PC Inspector](#karel-pc-inspector)
36. [One window per thing](#one-window-per-thing)

---

## Getting Started

### Requirements

- Windows 10/11 (64-bit, ARM64, or 32-bit)
- Network access to your robot controllers (same subnet or routed)
- Robot KCL/HTTP credentials (if Protected Resources is enabled)
- Robot FTP credentials (if not using anonymous FTP)

### First Launch

1. Run `RUKUS.exe` from the extracted folder.
2. Select your network adapter from the dropdown at the top of the main window. This tells RUKUS which network card to use when communicating with robots.
3. Create a cluster: **Cluster → New Cluster** (or right-click in the cluster list).
4. Name your cluster (e.g., "Cell A", "Paint Line").
5. Add robots to the cluster (see [Adding Robots](#adding-robots)).

### Adding Robots

**Manual entry:**
1. Right-click a cluster → **Add Robot**.
2. Enter the robot's IP address and a friendly name.
3. Enter FTP and KCL credentials if required.
4. Click **OK**.

**Smart Scan (recommended):**
1. Click **Smart Scan** from the toolbar or **Tools → Smart Scan**.
2. Click **Start Scan** to sweep your subnet for FANUC controllers.
3. Select discovered robots and click **Add to Cluster**.

---

## Finding Things Fast

Press **Ctrl+K** in any window to open the command palette.

1. Type a few letters of what you want — a window name, or an action.
2. Use **↑ / ↓** to move through the results.
3. Press **Enter** to run the highlighted one, or click the one you want.
4. **Esc** closes it.

It covers every window in the app plus the common actions (run a backup, refresh).
Searching matches the title and a set of keywords, so "diff" finds **Compare Robots**
and "mass" finds **Bulk Operations**.

> Ctrl+K works from inside a text box too, which is usually where your hands are when
> you reach for it.

### The Menu Bar

Seven menus, split by **what you are doing** - which is the only question you can answer
before opening one.

| Menu | What's in it |
|---|---|
| **System** | The app itself: Open Settings, Collect Diagnostics, Open Logs Folder, User Guide, **Error Code Lookup**, About, Exit. |
| **Cluster** | The cell's *record*: new/rename/delete a cluster, cluster properties, view raw data, import/export. Nothing here touches a robot. |
| **Tools** | Things you **run** against the cell: Smart Scan, Bulk Operations, Alarm Collectors, Backup Scheduler, KCL Console, Remove RUKUS Files. |
| **Monitor** | What the cell is doing right now: Production Dashboard, I/O and Data Watcher, Error Watcher, Signal Scope. |
| **Data** | Reading and changing values on a robot: System Variables, I/O Editor, Data Editor, Write Audit Log. |
| **Files** | Files on the robot and on disk: File Transfer (FTP), File Generator, Open Backups Folder, Open Error Logs Folder. |
| **Analyse** | Questions about what you have: Point Plot, Robot Timeline, Compare Robots, Where Used, Robot Report. All read-only. |

**The line between Cluster and Tools** is the one worth knowing: Cluster edits the list of robots
*on this PC*, Tools reaches out *to the robots*. "Rename this cluster" and "scan the subnet for
robots" are not the same kind of job, so they are not in the same menu.

**Error Code Lookup sits with the User Guide**, not with the error watchers. It is a reference
book that ships with the app and opens offline - its subject is robot faults, but the thing itself
is a document, and the menus group by what a thing *is*.

### Right-clicking a robot

Not everything lives on the menu bar. Right-clicking a robot card gives you, in order:

**Edit Robot** · **Copy IP Address** · **Set Clock to PC Time** · **Re-verify Identity...** ·
**Explore Robot Info...** · **View Error Log...** · **Robot Web UI...** · **Robot Files** (Load,
Download, Bounce, Remove RUKUS Files) · **Delete Robot**.

> Several of these are *only* here — there is no menu-bar route to Set Clock, Re-verify Identity,
> Robot Web UI or the Robot Files group.

> If you are looking for the old **File** or **Tools** menus, they are gone. Tools had grown
> to thirteen entries with no organising principle, and this app never opens or saves a
> document, so "File" promised something it did not have. Every item kept its behaviour -
> only where it lives changed.

---

## Managing Clusters

A **cluster** is a logical group of robots — typically robots in the same physical cell or on the same network segment.

### Creating a Cluster

- **Cluster → New Cluster** or right-click in the cluster list → **New Cluster**.
- Enter a name for the cluster.

### Adding Robots to a Cluster

- Right-click a cluster → **Add Robot**.
- Or use Smart Scan to discover and add robots automatically.

### Removing Robots

- Right-click a robot → **Remove Robot**.
- This removes it from the cluster but does not affect the controller itself.

### Cluster Operations

- **Rename:** Right-click cluster → **Rename**.
- **Delete:** Right-click cluster → **Delete**.
- **Import/Export:** Right-click cluster → **Import/Export** to share cluster configurations with other RUKUS installations.

### Network Adapter Binding

Each cluster is bound to a specific network adapter. This ensures RUKUS communicates with robots through the correct network card, which is important when your machine has multiple network interfaces.

- Click the **Adapter** dropdown at the top of the main window.
- Select the adapter connected to your robot network.

---

## Smart Scan

Smart Scan discovers FANUC controllers on your network by probing for FTP-capable devices.

### Running a Scan

1. Click **Smart Scan** or go to **Tools → Smart Scan**.
2. Ensure the correct network adapter is selected.
3. Click **Start Scan**.
4. RUKUS will probe IP addresses on your subnet for FTP devices.
5. Discovered devices appear in the results list.

### Understanding Results

- **Hostname:** Read from the controller's `$HOSTNAME` variable (if available), otherwise guessed from reverse DNS.
- **IP Address:** The controller's network address.
- **Model:** Best-effort detection (still in development).

### Adding Discovered Robots

1. Select one or more robots from the scan results.
2. Click **Add to Cluster**.
3. Choose an existing cluster or create a new one.
4. Enter credentials if prompted.

---

## Backing Up Robots

### Single Robot Backup

1. Right-click a robot in the cluster list.
2. Select **Backup**.
3. Choose the backup type:
   - **MD Backup:** Full backup of the controller's memory device.
   - **Filtered Backup:** Only specific file types (e.g., `.TP`, `.VA`, `.I`).
4. The backup starts immediately with live progress.

### Batch Backup

1. Select multiple robots in the cluster (Ctrl+click or Shift+click).
2. Click the **Backup** button on the toolbar.
3. All selected robots back up concurrently.

### Backup Progress

The backup window shows per-robot status:
- **Connecting...** — Establishing FTP connection.
- **Downloading: filename** — Currently downloading a file.
- **Done** — Backup complete.
- **Failed** — An error occurred (check the status message).

### Zip After Backup

Optionally compress the backup folder after download:
- Check **Zip after backup** in the backup dialog.
- The zip file is created next to the backup folder, and the unzipped folder is deleted.

### Backup Filters

When using Filtered Backup, select which file types to include:
- `.TP` — Teach Pendant programs
- `.VA` — Variables
- `.I` — I/O configuration
- `.DG` — Diagnostic files
- `.LS` — Program listings
- And more...

### Backup Location

Backups are saved to `Documents\RUKUS\Backups\` by default. You can change this in **System → Open Settings… → Folders**.

---

## Bulk Operations

Runs one action across a whole cluster instead of robot by robot. Open it from
**Tools → Bulk Operations** (or Ctrl+K, "bulk").

### Running an Operation

1. Pick the **Cluster**.
2. Pick the **Action** (see below).
3. Untick any robot you want left out — everything starts ticked, and **All** / **None**
   are there for the extremes.
4. Fill in the action's fields, if it has any.
5. Click **Run**, or just press **Enter**.

Each robot gets its own progress row, in cluster order, with the same spinner / green
check / red warning you already know from backups and file transfers. A failed row keeps
its **view robot errors** link.

### The Actions

| Action | What it does | Notes |
|--------|--------------|-------|
| **Back up** | Backs up every selected robot | Read-only. Uses the extension filters and Zip setting from the main window |
| **Set register value** | Writes one value to R, SR or UALRM on every robot | Changes what programs do |
| **Set I/O value** | Writes one value to an output — DOUT, GOUT, RO, FLG | **Drives real hardware** |
| **Set register name** | Names R, PR, SR or UALRM on every robot | Cosmetic |
| **Set I/O name** | Names any I/O point on every robot | Cosmetic |

**Naming is usually what you want.** Making `DI[3]` read "Part Present" on all six robots
in a cell is otherwise six trips to six pendants, which is exactly how they drift apart.

Two things you may notice are deliberate:

- **PR can be named but not set.** A position register has no single value to type.
- **Only outputs can be set.** An input reflects the state of the world; a controller
  will not let you write one.

### Before a Write Runs

Any action that writes asks once, naming the target, the value and the robot count —
"Set R[5] = 100 on 6 robots in Cell A" — even if you have per-write confirmations turned
off in Settings. One cluster-wide write is a different thing from one value typed into an
editor.

A banner above the buttons says what kind of write it is. Setting an **output** carries
the strongest warning in the app, because valves, grippers and signals to other equipment
can physically move.

Every robot's write is recorded separately in the [Write Audit Log](#write-audit-log), so
a bulk set of six robots produces six entries, each naming its own controller.

### If Something Fails

One robot failing never stops the rest — that is the whole point. A locked controller or
a bad password fails that row and the others carry on. When the run ends you get a summary
like `4 of 5 succeeded · 1 failed`, and **Retry failed** re-runs *only* the robots that
failed. It reads them off the finished run, not off the ticks, so changing the selection
while you read the failures cannot quietly change what gets retried.

**Cancel** skips robots that have not started yet and lets the ones already running
finish. Those show as *skipped*, not failed — nothing was sent to them.

### The Results pane

When the run ends, the pre-flight pane becomes the **Results** pane: the same rows you were
reading a moment ago, now saying what happened to each robot.

It has two shapes and picks the one its rows can fill:

- **After a preview** — robot, target, the robot's own name for it, the old value, the new value,
  and the outcome.
- **Without one** — a backup, a health check, a drift check, or a write run without pressing
  Preview has no before-and-after to report, so the pane shows robot, result, and *what happened*.

> **Fixed 2026-08-25.** The pane used to draw all six columns whatever the run was, so a backup
> produced a Results table with four permanently empty columns under headings promising values.
> A column blank for every row does not read as "not applicable" — it reads as broken.

### How Many at Once

RUKUS talks to several robots at a time, capped by **System → Open Settings… → Connection →
Robots at once** (default 4, maximum 10). Each robot is one FTP session, so the limit is
your PC's network adapter rather than the controllers. Turn it down if a large cell backs
up unreliably.

---

## Comparing Robots

Answers "why is this robot different from that one?" Open it from
**Analyse → Compare Robots** (or Ctrl+K, "diff"). **It never writes to either robot.**

### Running a Comparison

1. Pick the **Cluster** and what kind of **Data** to compare.
2. Choose each side: a **Live robot**, or a **Backup**. Any combination works — two live
   robots, a robot against a backup, or two backups.
3. **Folder…** lets a side point at any folder on disk, including one that did not come
   from RUKUS.
4. Click **Compare**.

### What You Can Compare

| Kind | What it reads |
|------|---------------|
| Registers | R, PR, SR and user alarms |
| I/O | Every port type the controller serves |
| System variables | The full variable set |
| Frames | UFRAME and UTOOL definitions |
| Programs | The TP programs on the controller |

### Reading the Results

Each row shows both sides and says **changed**, **added**, **removed** or **same** in
words as well as colour. The two value columns are headed with the actual source names,
captured when you pressed Compare — never "A" and "B".

- **Changed only** (on by default) hides everything that matches.
- **Ignore formatting** treats `1.0` and `1.000` as the same number.
- **Ignore names** compares values only, so a differently-named register still matches.
- **Export…** saves what is listed to CSV.

If one side cannot serve something the other can — a backup has no user-alarm data at all,
for example — the comparison narrows to what both sides genuinely have and tells you what
it left out. It will not report a whole category as "removed" just because one side could
not see it.

### Comparing Programs

Programs are compared per program, not per line: the question is "which programs differ",
and each is reduced to a fingerprint like `160 lines · a3f21c9d`.

Two things worth knowing:

- **Error and log files are not programs.** A backup folder holds far more `.ls` files
  than it has programs — `errcurr`, `logbook` and friends share the extension. Those are
  left out. Without that, two captures of the same idle robot ten minutes apart would show
  a dozen differences, none of them meaningful.
- **Ignore program dates** is on by default. Two robots running an identical taught
  program still differ on when it was created and last edited. Owner, comment and
  protection are still compared — only the two timestamps are ignored.

**Reading programs from a live robot is slow.** It is a file transfer, one connection per
program, so a live-vs-live comparison of a busy controller can take several minutes. The
status line says which robot it is on. Every other kind is a page fetch and returns in
seconds.

> Only folders containing register files appear in the **Backup** dropdown. To compare a
> folder that holds programs and nothing else, use **Folder…**.

---

## Where Used

Answers the question that comes before every change: **"I'm about to change `R[47]`. What else
touches it?"**

### Opening it

- **Analyse → Where Used**.

### Searching a backup

In its default mode the programs come from a **folder on disk**, not from a live controller. That means the search
costs the robots nothing, works while the cell is running, and works when a robot is switched off.

The trade is that the answer is only as current as the backup, which is why the line under the
search row tells you the backup's **age** and not just its name.

> **The one wrong answer this window can give:** a stale backup reports "not used" about a register
> a program started using last week. Check the age before you trust a no-results result.

### Running a search

1. Click **Change folder...** and pick a backup folder. Subfolders are searched too, so a folder
   holding several robots' backups searches all of them in one pass.
2. Type what you want to find in **Find** — an address like `R[47]` or `DO[12]`, or any plain text.
3. Click **Search** (or just press **Enter**).

### How the search matches

- **An address is matched whole.** Searching `R[4]` will *not* report `R[47]`.
- **Anything else is searched as plain text** — a comment, a label, part of an instruction.
- The query is matched **as you typed it**. A `.LS` program spells its addresses its own way, and
  RUKUS does not guess between `DO[12]` and `DOUT[12]` — guessing would produce a search that
  silently finds nothing and looks like an answer. If you get no hits, try the other spelling.
- Only **`.LS`** programs are searched. `.TP` files are binary and are skipped.

### Reading the results

Each hit is one line: the **program name**, the **folder** it came from, the **line number**, and
the program line itself. The line is shown raw, exactly as an editor will show it at that number —
which is what makes it checkable.

- **Double-click a hit** to open that file at that line in your external editor (set under
  **System → Open Settings... → Editor**).
- **Export CSV** saves the whole result set.

---

## Robot Timeline

Answers **"what happened to this robot, and in what order?"** - and the question underneath it,
which is usually the real one: **what changed just before it started misbehaving?**

RUKUS already records backups, writes and alarms, but each in its own place. This puts all three
on one axis for a single robot.

### Opening it

- **Analyse → Robot Timeline** (or **Ctrl+K**, "timeline").

### It reads files, not robots

Every row came from something RUKUS wrote earlier: the backups tree, the write audit log, and
the archived error log. Nothing here contacts a controller, so it costs the cell nothing, works
while the line is running, and works with the robot switched off.

### Using it

1. Pick the **Robot**. The window follows the app's shared robot selection, so it usually
   already shows the one you were looking at.
2. Pick the **Period** - last 7, 30 or 90 days, or everything on disk.
3. Tick **Backups**, **Writes** and **Alarms** to show or hide each kind. These filter what is
   already loaded; they do not re-read the disk.

**Double-click any row** to reveal where it came from in File Explorer - the backup folder for a
backup, or the log file for a write or an alarm. **Export CSV** saves what is shown.

### Read the notes above the list

This window's most misleading possible output is an empty list, because **"this robot raised no
alarms" and "nobody has ever archived this robot's alarms" look exactly the same** - both are
zero rows.

So every source that is missing, empty or carries a caveat adds a line above the list. If you
have never run **Tools → Bulk Operations → Archive error log** for a robot, the notes say so
rather than letting the silence imply a clean history.

> **Alarm times come from the controller's own clock.** Backups and writes are stamped by this
> PC; an alarm is stamped by the robot, and nothing in the file says what timezone it meant. If a
> controller's clock disagrees with the PC's, alarms shift against the other two - and **order is
> the whole point of this window**. A controller an hour out can make a write look like it
> happened *after* the alarm it caused. The notes tell you when the newest archived alarm was, so
> you can sanity-check it.

### What it does not show

**Health checks, drift checks and robot comparisons are not here.** Each of those is computed
live and shown in its own window; none of them writes a record to disk, so there is nothing for a
timeline to read back. That is a gap in those features rather than in this one.

---

## Production Dashboard

The Production Dashboard provides a live status overview of all robots in a cluster.

### Opening the Dashboard

- Select a robot → **Monitor → Production Dashboard** (or toolbar button).

### What It Shows

| Column | Description |
|--------|-------------|
| **Robot Name** | Friendly name of the controller |
| **IP Address** | Network address |
| **Status** | Running, Paused, Stopped, E-STOP, or Error |
| **Current Program** | Name of the active TP program |
| **Current Line** | Line number the robot is on |
| **Robot Mode** | AUTO, T1, or T2 |
| **Last Alarm** | Most recent alarm (if any) |
| **Connection** | FULL (web server) or LIMITED (FTP fallback) |

### Connection Modes

- **FULL:** Robot's web server is responding; KCL commands sent over HTTP.
- **LIMITED:** Web server is down; status read over FTP (slower updates, less detail).

RUKUS automatically falls back to FTP when the web server stops responding, and returns to full-speed polling when it recovers.

### KCL commands

KCL commands are no longer entered on the dashboard. Open **Tools → KCL Console** for an ad-hoc KCL prompt with its own robot picker, quick-command lists, a command reference, and colour-highlighted responses (see the KCL Console section).

### Credential Retry

If a command returns a 401/403 error, RUKUS prompts you to re-enter credentials. This allows you to correct credentials without restarting the session.

---

## I/O Editor

Monitor and control digital and analog I/O points on a robot.

> **This section is about the I/O *Editor*** (`Data → I/O Editor`) — the full port list, with
> a watch list, writes and renames. The **I/O and Data Watcher** (`Monitor → I/O and Data
> Watcher`) is a different window; see [I/O and Data Watcher](#io-and-data-watcher).

> **Which window is this?** The steps below open the **I/O Editor** (`Data → I/O Editor`) - the full port list with a watch list, writes and renames. The **I/O and Data Watcher** (`Monitor → I/O and Data Watcher`) is a separate window that watches I/O and registers together as tiles - see [I/O and Data Watcher](#io-and-data-watcher).

### Opening the I/O Editor

- Select a robot → **Data → I/O Editor**.

### Viewing I/O Points

The I/O list shows all configured I/O points on the controller:
- **Port Number** — Physical port address
- **Type** — UO, UI, RO, RI, SO, SI, etc.
- **Signal Name** — User-defined alias
- **Current Value** — ON/OFF (digital) or numeric (analog)

### Watching Signals

1. Check the **Watch** checkbox next to signals you want to monitor.
2. Watched signals appear in the **Watch List** at the top.
3. Values update at the configured poll interval (**System → Open Settings… → Connection → I/O Watcher poll interval**).

### Writing Values

- Right-click a writable signal → **Set Value**.
- Enter the new value (ON/OFF for digital, number for analog).

### Renaming Ports

- Right-click a signal → **Rename**.
- Enter a descriptive name (e.g., "Gripper Close Command").

---

## Data Editor

Monitor robot registers (NumReg, PosReg, etc.) in real time.

> **This section is about the Data *Editor*** (`Data → Data Editor`). The combined watcher
> is [I/O and Data Watcher](#io-and-data-watcher).

### Opening the Data Editor

- Select a robot → **Data → Data Editor**.

### Watching Registers

1. Browse the register list.
2. Check the **Watch** checkbox for registers you want to monitor.
3. Watched registers appear in the **Watch List** with live values.

### Register Types

- **NumReg** — Numeric registers (floating point)
- **PosReg** — Position registers (joint, user frame, or world coordinates)
- **R** — Simple numeric registers

---

## I/O and Data Watcher

One window for everything you are watching, across robots and across kinds. I/O points and
registers sit side by side as tiles, so a handshake that spans two robots is one screen rather than
two editors.

### Opening it

- **Monitor → I/O and Data Watcher**.

### Adding what you want to watch

1. Pick a **Robot**, a **Type** and an **Index**, then **Add**.
2. Or tick **Watch** on a signal or register inside the I/O Editor or the Data Editor — both feed
   this same list.

**Pin** a tile to keep it across sessions; pinned tiles come back when you next open the window.
They are restored on *open* rather than at app launch, deliberately: re-adding a pin starts polling
a real controller, and reaching out to hardware before you have asked for anything is the wrong
default.

### Closing versus minimising

- **Minimise** hides to the tray and **keeps polling**.
- **Close** stops watching. That is the deliberate difference: closing is the explicit stop.

> Values are read-only here. A tile shows what a point reads; writing one goes through the editors,
> which is where the write confirmation and the audit record live.

---

## System Variables Browser

Browse and edit FANUC system variables with plain-English descriptions.

### Opening the Browser

- Select a robot → **Data → System Variables**.

### Browsing Variables

The left pane shows a tree of system variable namespaces:
- `$ALM_IF` — Alarm interface variables
- `$MOR` — Motion group variables
- `$OPWORK` — Operator worksheet variables
- `$DMR_GRP` — Data memory group variables
- And many more...

Click a variable to see its value, type, and description (if available in the reference database).

### Reading Live Values

Select a variable and click **Read** (or it reads automatically) to fetch the current value from the controller.

### Writing Values

1. Select a writable variable.
2. Enter the new value.
3. Click **Write** to send it to the controller.

### Variable Descriptions

RUKUS bundles a reference database of 53,000+ known system variables with 3,800+ plain-English descriptions. If a description is available, it appears below the variable value.

---

## Robot Position Viewer

Display the robot's current joint and Cartesian positions in real time.

### Opening the Viewer

- Open the **Production Dashboard** (**Monitor → Production Dashboard**); the robot position view is opened from there.

### Position Display

- **Joint Position:** Current angle of each axis (J1–J6).
- **User Frame Position:** Cartesian coordinates (X, Y, Z, W, P, R) in the active user frame.
- **World Position:** Cartesian coordinates in the world frame.

### Refresh Rate

Positions update automatically at the configured poll interval.

---

## Error Log Viewer

View the robot's alarm history with the built-in Error Book reference.

### Opening the Error Log

- Right-click a robot → **View Error Log...**.

### Viewing Alarms

The error log shows historical alarms from the controller:
- **Alarm Code** — The FANUC alarm code (e.g., `SRVO-001`)
- **Message** — Alarm description
- **Timestamp** — When the alarm occurred

### Looking a code up

Click any alarm code to open the [Error Code Lookup](#error-code-lookup) on that code. It shows:
- **Title** — full alarm name
- **Cause** — what triggers this alarm
- **Remedy** — steps to resolve it

Clicking a second code re-points the lookup window that is already open rather than stacking
another one.

### Alarm History

RUKUS archives alarms per robot, so history survives the controller's own log rollover.

---

## Error Watcher

Monitor all robots in a cluster for new alarms simultaneously.

### Opening Error Watcher

- **Monitor → Error Watcher**.

### How It Works

- Error Watcher polls each robot in the cluster at the configured interval.
- New alarms appear in real time at the top of the list.
- Alarms are archived per robot so you can review history later.

### Filtering

- Filter by robot name, alarm code, or severity.

---

## Error Code Lookup

An offline dictionary of FANUC error codes — what a code means, what causes it, and what to do
about it.

### Opening it

- **System → Error Code Lookup**.

### Using it

Type in the **Find** box — it has focus the moment the window opens. You can search either way:

- **By code** — `SRVO-094`. A missing leading zero is fine: `SRVO-94` finds it too, because that
  is how the code often reads off a pendant.
- **By keyword** — `brake`, `overtravel`, `collision`. The cause and remedy text is searched as
  well as the title, so a phrase from the manual finds the alarm it belongs to.

Results are **ranked, not just filtered**: an exact code match comes first, then a title match,
then anything found in the cause or remedy. Searching `SRVO-021` puts SRVO-021 at the top rather
than burying it among the forty other alarms whose remedy paragraph mentions it.

**Facility** beside the box narrows to one prefix — `SRVO`, `INTP`, `DCS`. Leave it on *All* to
search everything.

Pick a row and the right-hand pane shows the code, its title, and the **cause**, **remedy** and any
extra note the manual carries. **Copy** puts the whole entry on the clipboard — code, title, cause
and remedy — because a colleague sent just `SRVO-021` still has to go and look it up.

The line under the search box says how many matched. Long result sets are capped at 500 and *say
so*; narrow the search to see the rest.

### What it covers

**10,490 codes** across **113 facility prefixes** — `SRVO`, `MOTN`, `INTP`, `TPIF`, `SYST`, `PRIO`,
`CVIS`, `PALL`, `SPOT`, `ARC` and many more.

> **Rebuilt 2026-08-25.** This used to be an embedded browser page, which is why it never looked
> like the rest of the app — a browser control sits outside the skin system, the type ramp, and
> `Ctrl+K`. It is now an ordinary RUKUS window reading the same data, so it follows whichever skin
> you have on and the command palette works in it.

> **It needs no robot and no internet.** The whole reference ships inside RUKUS as a local file, so
> it works on a plant PC with no network and on a laptop nowhere near the cell. It is the natural
> companion to the [Error Log Viewer](#error-log-viewer) and [Error Watcher](#error-watcher): those
> tell you *which* alarm fired, this tells you what it means.

---

## Signal Scope

A virtual oscilloscope for robot signals. Several signals share **one scrubbable timeline**, so you
can see not just what each one did but **what happened in what order** — which is the question a
list of current values can never answer.

Digital signals draw as **step traces**; registers and analog points draw as **lines**.

### Opening it

- **Monitor → Signal Scope** (or **Ctrl+K**, "scope").

### Adding signals

1. Pick the **Robot**.
2. Pick the **Type** — `DIN`, `DOUT`, `GIN`, `GOUT`, `AIN`, `AOUT`, `SI`, `SO`, `RI`, `RO`, `FLG`,
   `UI`, `UO`, `R`, `PR`, `SR`, `UALRM`. **Other...** lets you enter a type RUKUS does not list.
3. Enter the **Index** and click **Add**.

Or click **Browse...** to pick from the robot's actual point list instead of typing an index blind.

Drag signals in the left-hand rail to reorder them.

> **Signals from DIFFERENT robots can sit on the same timeline.** This is the most useful thing the
> scope does and the easiest to miss: add `DOUT[3]` from robot A and `DIN[7]` from robot B, and you
> are watching a handshake between two controllers on one time axis. For an interlock that
> occasionally races, it is the fastest way to see which side moved first.

### Reading it

The status bar reports the **measured** sample rate — seconds per sample and Hz — not the rate you
asked for. The two differ by however long the fetch actually takes, and the measured one is the
number that tells you whether a fast signal is being caught.

Use **Time window** to show 30 seconds, 1 minute, 5 minutes or 10 minutes at once. **Drag the plot**
to scrub back through the buffer; **Resume live** returns to the present.

### Freeze, Pause, and the difference

- **Freeze** stops the *display* only. Sampling carries on in the background, so resuming catches
  the picture up.
- **Pause** stops the polling as well.

### Measuring

- **Cursors** — turn the toggle on and click the plot to place two cursors. The status bar shows
  the time between them.
- **Edge markers** — per signal, mark **Rising edge**, **Falling edge**, **Both edges**, or **Off**.
  Set them from the Signal Scope window's own **Tools → Edge markers** menu, or right-click a
  signal in the rail.

### Exporting

- **Export → Save samples as CSV...** writes what the scope currently holds: one row per sample
  instant, one column per signal. A signal's cell is the value it was *holding* at that instant,
  because a step trace holds its level between samples. Analog values between two samples are **not**
  interpolated — the file contains only readings the controller actually reported.
- **Export → Save plot as PNG...** saves the plot exactly as it appears, including a frozen or
  scrubbed-back view.

### Clearing, and what closing does

- **Clear history** empties the samples but keeps the signals.
- **Clear signals** removes everything.

> **Closing the window does not stop the scope.** Its value is the history it has accumulated, and
> an accidental close throwing away five minutes of a captured sequence is exactly what this rule
> prevents. **Clear signals** is the explicit stop.

> The buffer holds roughly the last five to ten minutes depending on the poll interval. Older
> samples drop off the front, so export a capture you want to keep.

---

## Robot Info

Get a quick snapshot of a robot's configuration and status.

### Opening Robot Info

- Right-click a robot → **Explore Robot Info...**.

### What It Shows

- **Model** — Robot model and series
- **Software Version** — Controller software version
- **Options** — Installed options and features
- **I/O Counts** — Number of configured I/O points
- **Summary Sections** — Browsable sections from `SUMMARY.DG`
- **Recent Backup** — Link to the most recent local backup

---

## File Control

Transfer files between your PC and the robot controller.

### Opening File Control

- Select a robot → **Files → File Transfer (FTP)**.

### Browsing Remote Files

The remote file list shows files on the controller's FTP server:
- Navigate directories using the folder tree.
- File size and date are displayed.

### Upload Files

1. Click **Load** (upload).
2. Select local files to send to the controller.
3. Files are uploaded to the current remote directory.

### Download Files

1. Select files in the remote file list.
2. Click **Download**.
3. A pre-flight lists exactly what will be written and what it will overwrite. Untick anything you
   want to keep, then confirm.

> **Faster since 2026-08-25.** The pre-flight used to open a second FTP session and re-list every
> file on the device before a single byte moved — on a controller that is slow to authenticate and
> serves very few connections, that was most of the wait. It now reuses the listing the file pane
> already fetched, so a download starts when you press the button. Refreshing the pane refreshes
> that information too.

### Bounce (Round-Trip Test)

The **Bounce** feature tests file integrity by:
1. Uploading a local file to the controller.
2. Downloading it back over the same local file.
3. Comparing the result.

This verifies that the file transfer path is working correctly.

---

## File Generator

The File Generator scaffolds a valid **starter** FANUC file for you to build from. It
saves locally — you decide where to put the file and how to load it onto a controller.
Every type is generated from real captured files, so the structure is correct out of the
box; controller-computed sizes fill in when the file is loaded.

### Opening the File Generator

**Files → File Generator...**

### Choosing a File Type

Pick from the **File type** dropdown; the form on the left changes to match, and the
preview on the right updates live as you type:

- **TP Program (.LS)** — a teach-pendant program shell (name, comment, motion group, line count, read-only).
- **KAREL Program (.KL)** — a KAREL source shell with the common `%` directives.
- **SSI Page (.stm)** — a status / server-side-include page (see below).
- **Command File (.cm)** — a KCL command file that runs a sequence of commands top to bottom.

### Building an SSI Page (.stm)

Click **Add field** for each value the page should report. For each row, choose a **Data
source** and only the inputs it needs appear:

- **Digital / analog I/O** — pick the port type (DIN, DOUT, …) and an index.
- **Register** — pick the bank (R / SR / PR); tick **Whole bank** to dump the whole bank, or give an index.
- **System variable** — start typing to search thousands of known variables and pick one.
- **User alarm** — an alarm index (emits severity + message).
- **Include file** / **Custom** — a controller file to include, or any raw source you type.

An optional **Label** names the value; leave it blank to auto-name it.

### Building a Command File (.cm)

The file starts blank with a header (best run from a Controlled Start). Click **Add
command** for each row:

- **System variable (SETVAR)** — search for a variable and give a value (text is auto-quoted).
- **Register maximum** — pick a bank and a maximum count.
- **Raw line** — any command, passed through verbatim.

Each SETVAR / register row also takes an optional inline comment.

### Saving

**Save As…** lets you choose the location and filename. The generated file is a starting
point — open it in your editor and customise it before loading it onto a robot.

## KCL Quick Commands

Save and run frequently-used KCL commands as shortcuts.

### Opening Quick Commands

- Open **Tools → KCL Console** — the quick commands live inside the KCL Console (there is no separate menu item).

### Using Quick Commands

1. Browse the command list.
2. Double-click a command to execute it.
3. Results appear in the response area.

### Adding Custom Commands

1. Click **Add Command**.
2. Enter the KCL command and a description.
3. The command is saved for future use.

### Built-in Commands

RUKUS includes a curated set of commonly-used KCL commands:
- `SHOW VAR $ALM_IF` — Read alarm interface variables
- `SHOW VAR $HOSTNAME` — Get controller hostname
- `SHOW PROGS` — List running programs
- `RUN <program>` — Start a program
- `ABORT` — Stop the current program
- And more...

---

## Alarm Collectors

**Tools → Alarm Collectors…**

A controller keeps only so many alarms and then drops the oldest. Anything that happens between two
infrequent reads is gone before anyone thinks to ask. A collector watches a robot's alarm feed
continuously and saves what the controller is about to forget.

### Why it is not a scheduled task

A schedule is registered with Windows Task Scheduler: it fires, does one job and exits. Task
Scheduler cannot fire faster than once a minute, and a faulting controller rolls its alarm log
quicker than that - so this is a small resident process instead, started at sign-in, polling every
few seconds. That is also why it has its own window.

### Setting one up

1. Turn on **Collect while RUKUS is closed**. That registers the background process with Windows.
2. Click **Add collector**.
3. Give it a **name** (optional - "Weld cells", "Line 2"), pick its **cluster**, and tick the
   **robots** it should watch. A new collector watches every robot in the cluster until you untick
   one.
4. Set **Check every (seconds)** for that collector.

**Add as many as you want.** One per cell, one per cluster, one per shift pattern - each with its
own robots and its own rate, so a fast cell can be polled every few seconds without forcing that
rate on a quiet one. Everything saves as you change it; there is no Save button.

### Knowing it is working

Under the list, the window shows what the collector process itself last reported: whether it is
running, and then **one line per robot** - which collector polled it, how many alarms it has saved,
and the reason if it is failing. A collector that has been failing against an unreachable robot for
a week looks, from Windows alone, exactly like one quietly doing its job; this is where you see the
difference.

Collected alarms land in the error-logs folder and show up in that robot's **Error Log** window.

---

## Scheduled Tasks

Run work against a whole cluster unattended - on the days and at the time you choose,
**even when RUKUS is closed**.

### Opening it

- Click the **Schedule** button on the main window, or **Tools → Backup Scheduler...**.

### Creating a task

1. Click **Add Schedule**.
2. Configure:
   - **Task** - what to do. See *What a task can do* below.
   - **Cluster** - which cluster to run it against
   - **Days** - which days of the week (e.g. Mon-Fri)
   - **Time** - what time to run
   - **Network adapter** - which NIC to use, if this PC has more than one
   - **File extensions** and **Zip** - backups only
3. Click **Save**.

Saving is what registers the task with Windows. Nothing is scheduled until you save.

### What a task can do

| Task | What it does |
|---|---|
| **Back up the cluster** | The same backup the main window performs, for every robot in the cluster |
| **Archive the error log** | Collects each robot's alarm history into `Documents\RUKUS\ErrorLogs`, adding only entries it does not already have |

**Scheduled tasks only ever READ from a controller.** Nothing that writes a value, a name
or an I/O point can be scheduled - those have to be run by hand from **Bulk Operations**,
where a person can read the confirmation first. This is deliberate: every safeguard around
a write assumes somebody is watching, and a schedule at 2am has nobody.

### How it actually runs

Each saved task becomes a real entry in **Windows Task Scheduler**, in a folder called
**RUKUS**, named after its cluster. Windows starts a small helper program
(`RUKUS.Scheduler.exe`) at the appointed time - RUKUS itself does not need to be open, and
nothing appears on screen while it runs.

To see them: open Task Scheduler, expand **Task Scheduler Library**, and click the
**RUKUS** folder. A task shows there as *Disabled* when its schedule's **Enabled** switch
is off in RUKUS.

Edit tasks in RUKUS, not in Task Scheduler. RUKUS re-applies its own settings when you
save and again when it starts, so changes made in Task Scheduler are overwritten.

### What it cannot do

- **Run while the PC is switched off.** Nothing wakes a powered-down machine. A missed run
  starts automatically the next time the machine is available.
- **Run while nobody is signed in.** Tasks run as the user who created them, with no stored
  password - that is the trade for never having to ask for one.
- **Run on battery.** A backup interrupted by a flat battery leaves a folder that looks
  complete and is not.

A sleeping machine is fine - a task can wake it.

### If a task will not register

Windows can refuse - group policy, permissions, or someone deleting the task by hand. When
that happens the schedule's row says so:

> *Not registered with Windows, so this will only run while RUKUS is open - ...*

That schedule still runs the old way, while RUKUS is open, so nothing is lost. Leaving
RUKUS running in the tray (**Settings > Close button hides to system tray**) is the
workaround until the registration problem is sorted out.

### Scheduler log

View the activity log at `Documents\RUKUS\SchedulerLog.txt`.

The log shows:
- Which cluster was worked on, and what was done
- How many robots succeeded/failed
- Any errors encountered
- Why a task was skipped or refused

It is written whether the run came from Windows, from **Run Now**, or from RUKUS's own
fallback loop - so it is the one place to look when someone asks why last night's backup
did not happen.

---

## Write Audit Log

Every value RUKUS writes to a controller is recorded, whether it succeeded or not. Open
it from **Data → Write Audit Log**.

### What Is Recorded

| Field | Notes |
|-------|-------|
| When | Shown in your local time |
| Robot | Name and address — a name alone is not unique across clusters |
| Target | The address written, e.g. `R[5]` or `DOUT[3]` |
| Change | Old → new value, where the old value was known |
| Result | Ok, Failed, Cancelled or Refused |
| Who and where | Windows user, machine, and which part of RUKUS made the write |

**Attempts are recorded, not just successes.** A write you cancelled at the confirmation,
or one the controller refused because it was locked, is in the log too — on an industrial
tool the attempted-and-refused write is often the more interesting line.

Filter the list and **Export…** it to CSV for a shift handover or a paper trail.

> A bulk operation produces one entry **per robot**, each naming its own controller.

### "Change" for a bulk write

**Fixed 2026-08-25.** Bulk writes used to log a new value and no old one, so the log could not
answer the question it exists for — *what was it before?* The reason given at the time was that
reading each robot first would double the round trips, and that was true until Bulk Operations
grew its pre-flight.

The pre-flight already reads every selected robot before the confirm, so the old value is now
carried into the log entry. Two cases still record no old value, on purpose:

- You ran the write **without pressing Preview** — nothing read the robots, so nothing is known.
- The preview on screen had gone **stale** — a stale reading is not a fact about the robot any
  more, and a wrong "it used to be 200" is worse in an audit log than no claim at all.

---

## Settings

Configure RUKUS behavior from **System → Open Settings...**.

The window is a list of short pages grouped into **General**, **Files**, **Editing** and
**Appearance** - one page per section, so what you click is what you get. There is a **search box**
above the list: type what you want rather than what the section is called ("confirm", "where do
backups go", "overwrite") and it jumps to the setting and highlights it.

### Startup Behavior

| Setting | Default | Description |
|---------|---------|-------------|
| Default Cluster | None | Auto-select this cluster on launch |
| Auto-Connect | Off | Ping all robots in default cluster on startup |
| Ask on Exit | On | Show "Minimize to Tray vs Exit" dialog |
| Close to Tray | Off | Action when Ask on Exit is suppressed |

### Connection Tuning

| Setting | Default | Description |
|---------|---------|-------------|
| Command Timeout | 8 seconds | KCL/HTTP command timeout |
| Diagnostic Timeout | 10 seconds | Timeout for large page fetches |
| I/O Poll Interval | 5 seconds | I/O Watcher refresh rate |
| Data Poll Interval | 5 seconds | Data Watcher refresh rate |
| Error Poll Interval | 30 seconds | Error Watcher refresh rate |
| Robots at once | 4 (max 10) | How many controllers to talk to simultaneously, for cluster backups and every bulk action |

> **Robots at once** is one FTP session per robot, so the limit is your PC's network
> adapter rather than any controller. Lower it if a large cell backs up unreliably;
> raising it past a point buys nothing, because the controller is usually the slow part.

### Data Locations

| Setting | Default | Description |
|---------|---------|-------------|
| Backups Folder | `Documents\RUKUS\Backups` | Where backups are saved |
| Error Logs Folder | `Documents\RUKUS\ErrorLogs` | Where error logs are saved |

### Backup Naming Templates

Customize backup folder names using tokens:
- `{ClusterName}` — Name of the cluster
- `{RobotName}` — Name of the robot
- `{BackupType}` — MD, Filtered, or IMG
- `{Date:<format>}` — Date in .NET format (e.g., `{Date:yyyy-MM-dd}`)

**Example:** `{ClusterName}\{Date:yyyy-MM-dd_HH-mm}` produces `CellA\2026-07-27_14-30`.

### Logging

| Setting | Default | Description |
|---------|---------|-------------|
| Verbose Logging | Off | Enable debug-level log output |

### External Editor

| Setting | Default | Description |
|---------|---------|-------------|
| Editor Path | Notepad | Editor for viewing files and programs |

---

## Everyday Conveniences

Small things that are easy to miss.

### Starred Robots

Click the star on a robot card to favourite it. Starred robots sort to the top of every
robot picker in the app, not just the main window. The star is remembered per cluster,
because a robot name is only unique within one — two cells routinely both have a "Robot1".

### Windows Remember Where They Were

Move or resize a window and it reopens where you left it, including whether it was
maximized. Positions are always clamped back on screen, so unplugging a second monitor
cannot strand a window somewhere you cannot reach it.

Turn it off with **System → Open Settings… → Startup → Restore window positions**. Turning it off *keeps* what is
remembered, so turning it back on returns your layouts.

> A few windows size themselves to their content — Settings, Error Log, Robot Position,
> Schedule and Robot Web — and are deliberately left alone.

### Desktop Notifications

RUKUS raises a Windows toast when something you walked away from finishes or needs you: a
backup completing or failing, a scheduled backup failing, a new alarm on a watched robot,
or a long file transfer finishing. Clicking the toast opens the relevant window.

Deliberately quiet about: a clean scheduled run, anything you cancelled, and short
transfers. One switch in **Settings** turns all of them off.

### Staged Edits and Undo

In the Data and I/O editors, **Set** writes immediately while **Stage** queues a change
without sending it. Pressing **Enter** stages.

Staged edits can be stepped through with **Ctrl+Z** and **Ctrl+Y**, and **Commit** sends
them. Only the net change per target is sent — if you typed 5, then 8, then 12, the
controller receives 12 and the log records one write, not three.

Commit stops at the first failure and leaves the rest staged, so you can fix the cause and
commit again rather than hunting for where it stopped.

### Export

The Data editor, I/O editor, System Variables, the Error Log and Compare Robots all have
an **Export…** button that writes what is **on screen** to CSV, with a header block naming
the robot, the source and the time. If you filtered the list, you export the filtered list
— what you see is what you get.

---

## Reporting Problems

If something misbehaves, use **System → Collect Diagnostics...**.

### What's Included

The diagnostic bundle saves a `RUKUS-Diagnostics-<date>.zip` to your Desktop containing:
- App version and system info
- Settings (with secrets redacted)
- Cluster/robot summary (credentials excluded)
- Application logs
- Crash log (if any)
- Scheduler log (if any)
- Notes on anything that couldn't be collected

### What's NOT Included

- Robot passwords (FTP or KCL)
- Discord webhook URLs
- Any other sensitive values

### Before Reporting

1. Enable **System → Open Settings… → Diagnostics → Verbose logging**.
2. Reproduce the problem.
3. Collect diagnostics via **System → Collect Diagnostics...**.
4. Attach the zip file to your bug report.

---

## File Locations

RUKUS stores all data under `Documents\RUKUS\`:

| Path | Contents |
|------|----------|
| `Clusters\` | Cluster configuration files (`.json`) |
| `Backups\` | Robot backups |
| `ErrorLogs\` | Error log archives |
| `Logs\` | Application logs |
| `AppSettings.json` | User settings |
| `BackupSchedules.json` | Scheduled task definitions (the filename predates non-backup tasks) |
| `SchedulerLog.txt` | Scheduler activity log |
| `CrashLog.txt` | Last-resort crash log |
| `UserKclQuickCommands.json` | Custom KCL commands |
| `IoSignalAliases.json` | I/O signal name aliases |

---

## Position Plotter

A 3D scatter visualization tool for FANUC teach points. Loads P[n] positions from `.LS` files via FTP or local upload, renders them as an interactive 3D point cloud with path lines, direction arrows, and measurement tools.

### Opening the Position Plotter

**Analyse → Point Plot** from the main menu bar.

The window opens maximized (CAD-style fullscreen) with a 3-column layout:

| Column | Width | Content |
|--------|-------|---------|
| Left | 280px | Data selectors (robot, programs, filters) |
| Center | Fills | 3D viewport with overlaid toolbars |
| Right | 280px | Details panel (swaps per tool mode) |
| Bottom | Auto | Status bar (spans all columns) |

Only one Position Plotter window can be open at a time. Clicking the menu item again activates the existing window.

### Loading Data

#### FTP Mode (Download from Robot)

1. Select **Download from Robot** from the Data Source dropdown.
2. Select a robot from the **Target Robot** dropdown.
3. Click **List Robot Files** — programs appear in the Available list.
4. Transfer programs to the Selected list:
   - Select one or more programs, then click **▼** to add them.
   - Click **▼▼** to add all programs.
   - Double-click a program to transfer it.
5. Click **Plot 3D** to load and render.

**Cache indicators:**
- **●** = cached (file unchanged since last download)
- **○** = uncached (will download on next plot)

Right-click a program in the Available list and select **Refresh This Program** to force a re-download.

#### Local Mode (Browse Local Files)

1. Select **Browse Local Files** from the Data Source dropdown.
2. Click **Browse Files** — a file picker opens for `.LS` files.
3. Enter a robot name when prompted.
4. Programs appear directly in the Selected list.
5. Click **Plot 3D** to parse and render.

#### Filters

| Filter | Description |
|--------|-------------|
| **User Frame (UF)** | Filter points by UF number. Dropdown populates from loaded programs. |
| **Show Paths** | Toggle visibility of path lines and direction chevrons. |
| **Show Used Only** | Show only points referenced by motion commands. |

### 3D Viewport

#### Navigation Controls

| Input | Action |
|-------|--------|
| **Middle drag** | Pan camera |
| **Right drag** | Orbit camera around pivot |
| **Scroll wheel** | Zoom (toward selected point if one exists) |
| **Ctrl + Middle drag** | Dolly (move camera forward/backward along look direction) |
| **Z (hold) + Right drag** | Rotate in place (pivots on camera position) |

#### 2D and 3D

The **2D** button switches the viewport between free 3D and a flat, plane-locked view.

This is not just "look from the top". In 3D the camera is a *perspective* one, so even a
top-down view foreshortens — two points the same distance apart measure differently
depending on where they sit in the frame. It looks like a drawing and is not one.

2D is **orthographic and square to one plane**, so a distance on screen is the distance
taught. Orthographic on its own would still let you orbit off-axis, and a tilted
orthographic view is the worst case — it *looks* measurable — so rotation is switched off
with it.

| In 2D | Behaviour |
|---|---|
| **Rotate** | Off. Deliberately — see above. |
| **Pan / zoom** | Still work. A plan view you cannot pan across is a picture. |
| **Top / Front / Side** | Choose which plane you are square to. Remembered across a toggle. |
| **Iso** | Leaves 2D — an isometric view is a 3D view by definition. |
| **Fit** | Fits the plane you are reading, not the isometric framing. |
| **View cube** | Hidden. A rotation control that no longer rotates reads as broken. |

Switching modes keeps your framing rather than resetting it, so toggling on a scene you
have zoomed into does not jump back out to the whole cell.

Use 2D when you want to **measure or compare**; use 3D when you want to **understand the
shape** of a path.

#### Camera Presets

Buttons at the top of the viewport:

| Button | View |
|--------|------|
| **Iso** | Isometric view. In 2D, pressing it returns you to 3D. |
| **Top** | XY plane (top-down) |
| **Front** | XZ plane (front) |
| **Side** | YZ plane (side) |
| **Fit** | Auto-fit all points in view |

All presets position the camera relative to the data centroid, so they always frame your points regardless of coordinate range.

#### Fly Mode

Click the **Fly** button (highlights blue when active) to enter fly mode. Keyboard controls become active:

**Translation:**

| Key | Direction | Speed |
|-----|-----------|-------|
| W | Forward | 100mm/s |
| S | Backward | 100mm/s |
| A | Left | 100mm/s |
| D | Right | 100mm/s |
| E | Up | 50mm/s |
| Q | Down | 50mm/s |

**Rotation:**

| Key | Direction |
|-----|-----------|
| C | Rotate right |
| X | Rotate left |
| F | Rotate up |
| V | Rotate down |

**Speed Modifiers (hold while flying):**

| Modifier | Translation | Rotation |
|----------|-------------|----------|
| Default | 100mm/s | 0.1 rad/frame |
| **Shift** | 250mm/s | 0.25 rad/frame |
| **Ctrl** | 5mm/s | 0.01 rad/frame |

Click **Fly** again or press **Esc** to exit fly mode.

#### Point Rendering

- **Used points** — cream-colored spheres (10px). Points referenced by motion commands.
- **Unused points** — muted cyan spheres (10px, semi-transparent). Points not referenced by any motion command. Still clickable for inspection.
- **Labels** — zoom-adaptive level of detail:
  - Far zoom (>5000mm): hidden
  - Medium zoom (3000–5000mm): compact (P12)
  - Close zoom (1500–3000mm): index-only (P[12])
  - Very close (<1500mm): full (P[12:"SEAL_START"])
- **Path lines** — thin semi-transparent lines connecting points in program order.
- **Direction chevrons** — golden arrowheads showing motion direction along paths.

### Inspecting Points (Select Mode)

Select mode is the default. Click any point in the viewport to inspect it.

#### Selection

- A **golden ring** appears around the selected point.
- The **details panel** (right column) shows all point properties.
- The **status bar** shows "Inspect: P[n]".
- The **orbit pivot** updates to the selected point — right-drag now orbits around it.

#### Details Panel Properties

| Field | Unit | Description |
|-------|------|-------------|
| Index | — | P[n] index number |
| Label | — | Point label (if any) |
| X, Y, Z | mm | Cartesian position |
| W, P, R | deg | Orientation (roll, pitch, yaw) |
| Config | — | Joint configuration string |
| UF | — | User Frame ID |
| UT | — | Utility Frame (Tool) ID |
| Program | — | Parent program name |
| Robot | — | Robot name |
| Type | — | Program classification (seal/rework/utility/other) |
| Move | — | Move type (J/L/C) |
| Speed | — | Speed string |
| Cont | — | Continuity (FINE/CNT100/etc.) |
| Used | — | Whether referenced by motion commands |
| Usage Count | — | Number of motion command references |
| Source | — | "ftp" or "local" |

#### Multi-Point Detection

When multiple teach points share the same XYZ coordinates (within 3mm), the details panel shows all of them as separate cards. Each card displays Index, Label, Program, Robot, XYZ, WPR, MoveType, Speed, UF, and UT.

#### Point Search

A search bar at the top of the details panel lets you find points by index or label:

1. Type a number or label in the search box.
2. Matching points appear as clickable cards (up to 50 results).
3. Click a result to jump the camera to that point (300ms animation) and set it as the selected point.

### Measuring Distances (Ruler Mode)

Switch to Ruler mode by clicking the ruler icon or pressing **R**.

#### Two-Click Measurement

1. Click the **start point**. A snap indicator (golden ring) appears on the nearest teach point.
2. Click the **end point**.
3. A cyan line appears between the two points with a distance label.

#### Ruler Output

The details panel shows:

| Field | Unit | Description |
|-------|------|-------------|
| Start | — | First endpoint (P[n] if snapped, or coordinates) |
| End | — | Second endpoint |
| **Distance** | mm | Euclidean distance |
| dX, dY, dZ | mm | Component differences |
| dW, dP, dR | deg | Orientation differences (if both endpoints snapped to points) |
| Angle XY | deg | Angle projected onto XY plane from +X axis (0–360) |
| Angle XZ | deg | Angle projected onto XZ plane from +X axis (0–360) |

#### Ruler Controls

- **Hover** near a point to see the snap indicator before clicking.
- **Escape** cancels the in-progress measurement without changing mode.
- **Delete** removes the selected measurement.

### Measuring Arcs (Arc Mode)

Switch to Arc mode by clicking the arc icon or pressing **J**.

#### Three-Click FANUC CIRC Measurement

This mirrors the FANUC controller's `C P[n] : P[n]` circular motion syntax:

1. Click the **start point**.
2. Click the **via point** (intermediate point on the arc).
3. Click the **end point**.
4. The arc appears as a golden polyline with a center marker and radius line.

#### Arc Output

The details panel shows:

| Field | Unit | Description |
|-------|------|-------------|
| Start | — | First point (P[n] if snapped) |
| Via | — | Second point |
| End | — | Third point |
| **Radius** | mm | Circle radius |
| Center | — | 3D center position |
| Arc Angle | deg | Sweep angle |
| Arc Length | mm | Path length along the arc |
| Chord | mm | Direct distance start-to-end |
| Direction | — | CW (clockwise) or CCW (counter-clockwise) |

#### Arc Controls

- **Hover** near a point to see the snap indicator before clicking.
- **Escape** cancels the in-progress measurement.
- **Delete** removes the selected measurement.

### Where Used: asking the robot instead of a backup

Where Used has two sources, and the difference is the whole point.

**Backup folder** (the default) searches `.LS` files on this PC. It costs the controllers nothing,
works while the cell is running and works when a robot is switched off - but the answer is only as
current as the backup.

**The robot itself** uses the controller's own search. It answers about what is loaded *right now*,
and it can search three things a backup search cannot:

| Search | What it covers |
|---|---|
| Programs | TP program listings |
| All registers | numeric, position and string registers |
| System variables | the `SYS*.VA` set |
| KAREL variables | KAREL program variables |

It needs the robot reachable with its web interface unlocked, and it takes **seconds, not an
instant** - the controller does not answer until it has finished searching. Twenty seconds on a busy
cell is normal.

#### The two modes match differently

A backup search matches whole addresses: `R[4]` will not report `R[47]`. The controller matches
**substrings**, so a live search for `R[1]` also returns `AR[1]`, `VR[1]` and `PR[1]`. That is the
robot's own behaviour and RUKUS does not filter it out - trimming the controller's answer would mean
showing you fewer rows than it actually found. The line under the results says so whenever a live
search returns hits, and it is why the same query can return more rows live than from a backup.

#### Reading the results

| Column | Means |
|---|---|
| **On robot** / **From backup** | the controller device (`md`, `ud1`), or the backup folder |
| **Program** | the file, without its extension |
| **Line** | the line number in the **file** |
| **Line found** | the matched text. For a program it starts with its own **TP line number** - the one the pendant shows |

So a row reading `md` / `BLOWOFF` / `34` / `11:  CALL MHBLOWOF(AR[1],AR[2]) ;` is line 34 of the
file, which is TP line 11.

#### Opening a program from a hit

Right-click a hit:

- **Open program to edit...** downloads that program off the robot, opens it in your configured
  editor, then offers **Save to Robot** / **Save to PC** / **Discard**. Save your changes in the
  editor first (Ctrl+S), then choose. Saving to the robot **overwrites** the program there, so
  reload it on the pendant before running it. Live hits only - a backup hit is already a file on
  this PC, so double-click opens it in place.
- **Copy line** puts the program, line number and text on the clipboard.

Binary files are refused rather than opened: a `.TP` is compiled, and a text editor would corrupt it.

### Exporting a Shifted Program

**Export program** writes shifted copies of the programs you have ticked - the offline cousin of the
controller's own Program Shift and Tool Offset utilities. It is the only thing the plotter does that
produces a file a robot will run, so read this section before using it.

> **Every shifted program must be verified on the robot.** Step it through in T1 at low override
> before any automatic run. CONFIG strings and turn numbers are **not** recalculated - a large shift
> can make a point unreachable or need a different configuration.

#### What it can shift

The programs you loaded **from a folder on disk**. A program downloaded from a controller is not
offered: the plotter keeps its points, not its text, so there is no file to shift and no guarantee a
cached copy still matches the controller. Load the same program from a backup folder to export it.
Anything that cannot be exported is listed with the reason.

#### The three shifts

| Mode | What it does |
|---|---|
| **Frameshift** | The points stay in the same place in space, expressed in a different user frame. |
| **Toolshift** | The flange path stays identical, run with a different tool. |
| **Offset** | Move every point by a fixed amount. XYZ moves in the parent frame; W/P/R turns each point where it stands. |

For Frameshift and Toolshift the **From** and **To** lists offer only frames the plotter actually
knows - the ones read from the backup's `sysframe.va` or that you defined when prompted. A number
with no definition is not offered, because shifting through a guessed frame moves the whole program
somewhere plausible and wrong.

Leave **Also renumber UFRAME_NUM= / UTOOL_NUM= in the program body** ticked unless you have a reason
not to. Without it the points are expressed in the new frame while the program still selects the old
one at runtime, which is the dangerous half of a shift.

**Points** narrows it to particular `P[]` numbers (`1-5,8`); blank means all of them.

#### Naming the output

By default the shifted program keeps its name plus a **Suffix** - `PICK_T1` becomes
`PICK_T1_SH.LS`. That is what you want when you are shifting several programs at once.

When exactly **one** program is ticked you can instead type a name in **Name it something
else**. A shift is usually a new program - `PICK_T1` moved onto the second fixture is
`PICK_T2`, not `PICK_T1_SH` - and naming it here saves loading it under a generated name and
renaming it on the pendant. The line under the box always says exactly what the file will be
called, including anything that had to be changed: a program name takes only letters, digits
and underscore, and is upper-cased.

The box is disabled when several programs are ticked, because one name cannot describe several
outputs. Typing the original program’s own name is refused - that would overwrite it.

#### What it will not do

- **It never touches your original.** The shifted program is a new file.
- **It renames the program inside the file** to match the new filename, whether that name came
  from the suffix or from the name box. A copy that still calls itself by the old name
  overwrites the original when you load it onto the controller.
- **It never overwrites an existing export.** Change the suffix or move the old one first.
- **It skips anything it cannot shift correctly, and names it** in the report: joint-form positions,
  positions with incomplete coordinates, and points taught in a different frame or tool from the one
  you chose to shift from.
- **It writes nothing for a program where nothing could be shifted.** A file named `_SH` that is
  identical to the unshifted program is worse than no file at all.

#### The report

Every export writes `shift_report_<date>_<time>.md` beside the programs: each point's coordinates
before and after, how far it moved, and everything that was skipped with the reason. Read it before
you load anything.

### Keyboard Shortcuts Reference

#### Always Active

| Key | Action |
|-----|--------|
| **Z** (hold) | Override rotation pivot to camera position |
| **Esc** | Cancel measurement, clear pivot, return to Select mode |
| **Delete** | Delete selected measurement |

#### Tool Mode Shortcuts (when fly mode is off)

| Key | Action |
|-----|--------|
| **V** | Switch to Select mode |
| **R** | Switch to Ruler mode |
| **J** | Switch to Arc mode |

#### Fly Mode Only

| Key | Action |
|-----|--------|
| W/S/A/D | Move forward/backward/left/right |
| E/Q | Move up/down |
| C/X | Rotate right/left |
| F/V | Rotate up/down |
| Shift | Speed boost (2.5x) |
| Ctrl | Precision mode (20x slower) |

### Mouse Controls Reference

| Input | Action |
|-------|--------|
| Left click | Tool action (select point, place measurement endpoint) |
| Middle drag | Pan camera |
| Right drag | Orbit camera |
| Ctrl + Middle drag | Dolly (move camera forward/backward) |
| Scroll wheel | Zoom toward cursor or selected point |
| Z (hold) + Right drag | Rotate in place |

### Coordinate Readout

The status bar shows the XYZ coordinates of the point under the cursor as you move the mouse over the viewport.

### Troubleshooting

**No points visible after plotting:**
- Check that the programs contain Cartesian positions (not joint-only).
- Try **Fit** to auto-frame the data.
- Check the UF filter — it may be filtering out all points.

**Labels overlap at medium zoom:**
- Labels have density culling — only non-overlapping labels are shown.
- Zoom in closer to see full labels.

**Measurement disappeared:**
- Measurements persist across filter changes but are cleared when you click **Plot 3D** again (re-render).
- Use **Delete** to remove a specific measurement.

**Window won't open:**
- Only one Position Plotter window can be open. Check if it's already open behind other windows.

**Points at extreme coordinates:**
- Use the **Fit** button to auto-frame regardless of coordinate range.
- Camera presets (Top/Front/Side) always position relative to the data centroid.

**Slow performance with many points:**
- The spatial index provides O(1) lookups, but very large programs (5000+ points) may still feel slow due to rendering.
- Use **Show Used Only** to reduce the visible point count.
---

## Robot Report

One workbook describing **everything that is set up on a robot** - I/O, registers, frames,
reference positions, payloads, mastering, EtherNet/IP and the DCS safety configuration - with one
sheet per section.

### Opening it

- **Analyse → Robot Report**.

### Two sources, one report

The switch at the top of the window chooses where the variables come from.

**Backup folder** reads a controller backup already on this PC. Point it at the **robot's own
folder** - the one holding the `.va` files - not the dated batch folder above it.

**The robot itself** pulls the variables straight off the controller over FTP into a temporary
folder, then reports on that folder exactly as it would a backup. It is the same report from either
source, not two reports that have to be kept saying the same thing.

Use the robot when the question is *what is set up on this robot right now*; use a backup when the
robot is switched off, when you want a historical answer, or when the backup came from another site
entirely.

> **It fetches every `.va` file, not a chosen few.** Which file holds which variable is FANUC's
> business and changes with the controller options installed - a hand-picked list derived from one
> robot silently drops frames or string registers on the next one. Fetching everything is a few
> megabytes and takes seconds; press **Stop** if you need the connection back.

### Choosing what goes in

Every one of the sixteen sections has its own tick, so a frames-only sheet or an I/O-only sheet is
one click rather than a sixteen-tab workbook every time. **All**, **None**, **Setup data** and
**Safety (DCS)** just move the ticks - there is no second path that builds a different report.

### Seeing the sheet before you make it

The right-hand pane shows the sheet the highlighted section will produce - column letters, row
numbers, the identity block, the headings and the first 200 rows. It is drawn from the same layout
the file is written from, so it is not an impression of the workbook, it *is* the workbook.

Two targets per row: clicking a section's **name** previews it, clicking its **checkbox** includes
it. You can read a sheet you have no intention of exporting, which is usually how you decide.

### Narrowing a sheet

**Column chips** above the grid turn columns off. The spreadsheet letters re-letter as they go, so
a dropped column is obvious. The last column cannot be dropped - a sheet with no columns is a blank
tab, not a narrower report.

**Hide unused slots** appears on sheets that have them - position registers, frames and reference
positions. A controller ships with 192 reference-position slots and 200 position registers whether
or not anybody set one up, and the report faithfully lists all of them because that is what the
robot says. Ticking this drops the slots nobody configured; unticking it gives you every row back,
exactly as the controller reports them.

> It is offered only where the robot marks a slot as never-set-up, and it reads that mark in one
> specific column. Other sheets can contain the word `Uninitialized` in a field of a perfectly live
> record - an EtherNet/IP connection that is running but has no host name typed in, for instance -
> and those rows are never hidden.

> Whatever is trimmed is **named in the footer**, next to the Generate button. A report that is
> quietly shorter than the robot's own answer is the kind of thing you find out about a year later,
> comparing two of them.

### A section that cannot be read

Some sections have a source file that may simply not be in the backup - **DCS Safe I-O Connect**
reads `DCSVRFY.DG`, which the controller only writes once somebody has run a DCS verify.

Such a section is dimmed and unticked, and its preview says so. You can still tick it: leaving it in
produces a tab explaining that nobody ran a verify, which is a different statement from having no
such tab at all. Which of those your report should make is your call.

### Saved presets

Most people generate the same two or three reports. **Save preset...** stores the current sections,
their column choices and their row filters under a name; picking one and pressing **Apply** puts
them all back.

Applying a preset unticks everything it does not name, so the same preset always means the same
report.

### Scan before you choose

Picking a folder scans it, and fetching from a robot scans what it fetched. Either way a row count
appears beside every section, so you choose against what is actually there rather than against
hope. A backup with no DCS in it should not tempt you into ticking four DCS sections.

A section that finds nothing **still gets a sheet**, saying so. "No DCS zones are set up" and "there
is no DCS tab" are different claims, and only the first is about the robot.

### Every sheet says whose data it is

Each sheet starts with the **robot**, the **cluster** where one is known, and a **source** line
carrying the warning that these values belong to one individual robot. Below that is a blank row,
then the table.

That blank row is deliberate: it keeps Excel from treating the identity lines as column headings, so
clicking any cell in the table and pressing **Sort** still does the right thing.

> **Why it is on every sheet rather than a cover page:** a tab pasted into an email arrives without
> the cover page. Reading one robot's registers for the robot beside it is the most expensive
> mistake this tool could enable.

### The robot name

In live mode the name follows the robot you picked. For a backup folder it is derived from the
folder name, and you can type anything you like over it - a backup may well have come from a robot
this installation has never heard of, off a USB stick or from another site.

Switching the source **re-points the name**, so a report from a robot can never go out headed with
the name of the folder you were looking at a moment ago.

### Reading a robot is faster, and switching robots is safe

**It fetches only the files the ticked sections read.** A registers-only report needs one 6 KB
file, not all 139 on the controller — this used to pull the lot every time.

The file list behind that is measured off a real full backup rather than taken from a manual, and
it is **checked rather than trusted**: if a ticked section comes up with nothing, RUKUS pulls the
rest of the files, reads them again, and tells you it did. So a controller whose options put a
variable somewhere unusual costs you one slow report — never a silently empty sheet.

Two sections always fetch everything, because both are about the backup as a whole: **Robot
Information** (which counts the files) and **Backup File Inventory** (which lists them).

**Switching robots clears the report.** The robot name used to follow the picker while the values
stayed behind, so a report could go out headed with one robot's name and filled with another's.
Now the pane empties and asks you to press **Fetch and scan**.

Every robot you read this session keeps its files in a temp folder, so switching *back* to one
redraws immediately — labelled with the time it was read, and with **Fetch and scan** right there
to re-read it. Closing the window deletes all of them.

---

## Point Analysis

Opened from the [Position Plotter](#position-plotter)'s own menu, Point Analysis measures the points
on the plot rather than drawing them: distances between selected points, reach envelopes, and the
numbers behind what the 3D view is showing.

---

## Check Trends

**Analyse → Check Trends** plots the history of the health, drift and comparison checks RUKUS has
run, so "three differences again this week" is visible rather than remembered.

### A row is a series, not a robot

Each row is one **robot + kind of check + what it was measured against**. Runs against a different
master backup, or comparing different data kinds, are *different series* — averaging them together
would let a narrower run read as an improvement.

### Reading it

- One bar per run, scaled within its own series.
- **Steady above zero is amber, not green.** A robot with the same three differences for a month is
  unattended, not fine.
- Zero across the series is green.

Runs recorded before 2026-08-23 each sit in their own one-run series: the comparison records used to
embed the capture time, so no two could ever line up. Fixed at the source; older records keep their
original context.

---

## KAREL .PC Inspector

**Tools → KAREL .PC Inspector...** opens a compiled KAREL program (`.PC`) and shows you what is
inside it — the container, the p-code, the symbol table, and decompiled KAREL source.

### What it produces

- **Structure** — the container's sections and what each holds.
- **Disassembly** — the p-code instructions, one row per instruction.
- **Symbols** — the program's own names for its variables and routines.
- **Source** — decompiled `.KL`, in either a plain or a laid-out form.

### How far to trust it

The decompiler is verified against a 250-program corpus, and the real test is not "does it look
right" but "does it compile back": fed to FANUC's own `ktrans`, the output recompiles **145 of 250**
corpus programs to a byte-identical `.PC`. The rest decompile to readable source that is not yet
byte-exact.

> Treat the source as something to *read*, not as a drop-in replacement for a `.KL` you have lost —
> unless you have recompiled it and compared the result yourself.

---

## One window per thing

**Changed 2026-08-25.** Windows used to stack: clicking *Settings* three times gave you three
Settings windows, each with its own idea of what the settings were, and closing the wrong one lost
your edits. Clicking eight robots' error logs gave you eight windows.

Now:

- **Global windows open once.** Settings, Smart Scan, File Generator, the Production Dashboard, the
  Error Code Lookup and the rest — asking again brings the open one forward.
- **Per-robot windows open once per robot.** Error Log, Robot Info, Current Position and Robot Web
  each get one window *per controller*, so you can have four robots' error logs side by side but
  never two of the same robot's.
- **Opening a thing that is already open re-points it.** Clicking an alarm code while the Error Code
  Lookup is open jumps that window to the new code instead of opening another.

Matching is by the robot itself, not its name — FANUC ships every controller called `ROBOT`, and a
name match would hand you the wrong robot's window.
