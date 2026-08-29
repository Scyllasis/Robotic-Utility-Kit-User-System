# Installing RUKUS

Windows 10 (1809 or later) or Windows 11, 64-bit. Nothing else to download first — the
installer carries the .NET runtime and the Windows App Runtime and puts both in place for
you.

The download is **195 MB**. A first install uses about **478 MB** of disk: 312 MB for RUKUS,
and 166 MB for the Windows App Runtime — a Microsoft component installed once and shared
with any other app that uses it, and skipped entirely if the machine already has it.

---

## 1. Download

Get `RUKUS-Setup-<version>.exe` from the **[Releases page](../../releases)**.

Take it from the Releases page and nowhere else. If somebody sends you a RUKUS installer
directly, check it against the release before you run it — see step 2.

## 2. Check the download is ours

**RUKUS is not code-signed for the beta.** That means Windows cannot vouch for who built the
installer, so you should. Every release publishes the installer's SHA-256 in its notes.

Open PowerShell where you downloaded it and run:

```powershell
Get-FileHash .\RUKUS-Setup-*.exe -Algorithm SHA256
```

Compare the `Hash` it prints against the one in the release notes. They are case-insensitive.

| Result | What it means |
|---|---|
| **Matches** | The file is byte-for-byte what we published. Carry on. |
| **Does not match** | Do not run it. Delete it and download again. If a fresh download also mismatches, [open an issue](../../issues) — do not run it in the meantime. |

> **Why this matters more than usual here.** RUKUS is installed on a machine that talks to
> live robot controllers. An installer of unknown provenance running as administrator on that
> machine is not the same risk as on a laptop, and the checksum is thirty seconds.

## 3. SmartScreen will warn you

Because the installer is not signed, Windows shows a blue **"Windows protected your PC"**
box. This is expected, and it is not a virus warning — SmartScreen is saying it does not
recognise the publisher, which is true.

**Once you have checked the hash in step 2:**

1. Click **More info**
2. Click **Run anyway**

If you would rather not do that, you can unblock the file first: right-click it →
**Properties** → tick **Unblock** at the bottom → **OK**.

> **Do not skip step 2 and then do this.** The hash check is the thing that actually tells you
> the file is genuine. Clicking through SmartScreen is just telling Windows you already know.

Code signing is on the list for the first non-beta release, which will remove this step.

## 4. Run the installer

Standard Windows installer. It will ask for administrator rights, because it writes to
Program Files, installs the Windows App Runtime, and creates the Start Menu entry.

| Prompt | Notes |
|---|---|
| **Install location** | Default is fine. |
| **Desktop shortcut** | Optional. |
| **Start Menu folder** | Default is fine. |

### It will pause on "Installing the Windows App Runtime"

**This is the slow part, and it is not a hang.** RUKUS is built on Microsoft's Windows App
SDK, and that runtime is installed as a system component rather than dropped in the program
folder. The installer runs Microsoft's own installer for it, silently, and waits for it to
finish. On a machine that does not already have it, expect **up to a minute or two** on that
step with no visible progress.

Leave it alone while it runs. If the machine already has the runtime it goes past in
seconds.

The installer places RUKUS and a small companion, `RUKUS.Scheduler.exe`, in the same folder.
**Do not delete or move the scheduler** — it is what runs your scheduled backups when RUKUS
itself is closed. Without it beside the app, scheduled backups only fire while RUKUS is open.

## 5. Notifications — worth thirty seconds now

RUKUS raises Windows notifications for things that finish while you are looking elsewhere:
a backup finishing or **failing**, a new alarm on a watched robot, a long transfer
completing, a watched value changing.

The one worth caring about is a **scheduled backup failing overnight**. If notifications are
not getting through, that failure tells nobody until somebody goes looking.

**Settings → Connection → Send a test notification.** A toast should appear at the
bottom-right of your screen.

| What you see | What to do |
|---|---|
| A RUKUS notification appears | Done. |
| Nothing appears | Check **Focus Assist / Do Not Disturb** is off, and that RUKUS is allowed in **Windows Settings → System → Notifications**. RUKUS cannot see any of those — Windows swallows the toast without telling the app — which is why the test button exists. |
| A warning bar says notifications are unavailable | The app could not register with Windows at all. [Open an issue](../../issues) and attach the diagnostics bundle. |

## 6. First launch

RUKUS opens on an empty cluster. Two ways to get robots into it:

- **Tools → Smart Scan** — sweeps a subnet and finds controllers for you. Easiest if you do
  not have a list of IPs.
- **Add Robot** on the main screen — if you know the address.

Then see the **[User Guide](USER-GUIDE.md)**, which starts from here.

### Where RUKUS keeps your data

Everything user-facing lives in `Documents\RUKUS\`:

| | |
|---|---|
| `Backups\` | Robot backups, by cluster and date |
| `Clusters\` | Your cluster and robot definitions |
| `Logs\` | Application logs, one per day |
| `WriteAuditLog.jsonl` | Every value RUKUS has written to a controller |
| `AppSettings.json` | Your settings |

None of this is inside Program Files, so it survives an uninstall — see below.

---

## Uninstalling

**Settings → Apps → Installed apps → RUKUS → Uninstall**, or use the Start Menu entry.

The uninstaller asks a couple of optional questions about why you are removing it. Answering
is entirely optional and nothing is sent anywhere — it writes a note next to the app for you
to send on if you want to.

**Your data in `Documents\RUKUS\` is left alone.** Backups you have taken are yours, and an
uninstaller that deletes a folder of robot backups is not one anybody should ship. Delete
that folder by hand if you want it gone.

---

## Troubleshooting an install

| Symptom | Cause and fix |
|---|---|
| **"Windows protected your PC"** | Expected — see step 3. |
| **Installer will not start, no message** | The download was blocked. Right-click → Properties → **Unblock**. |
| **"This app can't run on your PC"** | 32-bit Windows, or Windows too old. RUKUS needs 64-bit Windows 10 1809+. |
| **Scheduled backups never fire when RUKUS is closed** | `RUKUS.Scheduler.exe` is missing from the install folder. Reinstall rather than copying it in. |
| **App starts, then closes immediately** | Check `Documents\RUKUS\CrashLog.txt` and attach it to an issue. |
| **Installs fine, then will not start at all** | The Windows App Runtime step did not complete — usually the install was interrupted, or a policy blocks installing system components. Run the installer again and let the runtime step finish. |
| **Installer sits on "Installing the Windows App Runtime"** | Expected on a machine without it — see step 4. Give it a minute or two. |
| **No notifications ever appear** | See step 5. It is almost always Focus Assist or Windows' per-app notification settings, neither of which RUKUS can see. |
| **Cannot reach any robot** | Almost always the network adapter, not RUKUS. Check the **Adapter** dropdown on the main screen is the NIC on the robot subnet. |

### If you need to report it

**System → Collect Diagnostics…** builds a zip with the logs, your settings and the crash log
in it, and offers to show you the file. Attach that to your issue — it is the difference
between a report somebody can act on and one that needs three rounds of questions.

It contains your robot names and IP addresses. It does **not** contain robot passwords.
