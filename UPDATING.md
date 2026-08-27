# Updating RUKUS

RUKUS can check this repository for a newer release. What it does when it finds one is
**your choice** — including doing nothing at all.

---

## The short version

**Settings → Updates.** Pick one of four:

| Mode | What happens |
|---|---|
| **Off** | RUKUS never contacts GitHub. Nothing is checked, nothing is sent. |
| **Notify me** | Checks on startup. If there is a newer release, a bar appears with the version and a link to the notes. Nothing is downloaded. |
| **Download, then ask** | As above, and it fetches the installer in the background and verifies it. You click when you want to install. |
| **Automatic** | Downloads, verifies, and installs on next launch without asking. |

**"Notify me" is the default, and it is the one to keep on a machine that runs a cell.** A
robot cell is not a place for a surprise version change mid-shift.

---

## What the check actually does

Once, at startup, RUKUS asks GitHub's public API for this repository's releases:

```
GET https://api.github.com/repos/Scyllasis/Robotic-Utility-Kit-User-System/releases
```

It reads the version tags, finds the newest, and compares it to the running build. That is
the whole request.

**It sends nothing about you.** No account, no licence key, no robot names, no telemetry.
It is an unauthenticated read of a public page — the same request your browser makes opening
the Releases page. There is no analytics in RUKUS at all.

**A failed check is not "you are up to date".** A machine on a plant network with no route
out to github.com is a completely normal way to run this app, and RUKUS says it could not
check rather than pretending it did.

---

## Nothing is installed unless it verifies

Every release publishes its installer's SHA-256 in the release notes. When RUKUS downloads
an update it re-computes that hash and compares.

| | |
|---|---|
| **Hash matches** | The installer is exactly what we published. It can be run. |
| **Hash does not match** | The download is **deleted** and the failure is logged. Nothing is run. |
| **Release publishes no hash** | RUKUS will not download it at all. It points you at the release page to do it by hand. |

That last row is deliberate and it is a refusal, not a warning. RUKUS installs on machines
that write to live controllers; "it came off the internet" is not provenance enough to run
something as administrator on one of those.

A failed download is deleted rather than left behind, because a file called
`RUKUS-Setup-0.9.1.exe` sitting in your temp folder is one somebody runs next week having
forgotten why it was there.

---

## Updating by hand

You never have to use the built-in check. It is entirely reasonable to leave updates **Off**
on a production machine and do this instead:

1. Open the **[Releases page](../../releases)**
2. Download the new `RUKUS-Setup-<version>.exe`
3. **Check the SHA-256** — [same as a first install](INSTALL.md#2-check-the-download-is-ours)
4. Run it. It installs over the top; you do not need to uninstall first.

**Your settings, clusters and backups are untouched by an update.** They live in
`Documents\RUKUS\`, not in the program folder.

---

## Version numbers

RUKUS uses SemVer-style versions, and during the beta they carry a pre-release suffix:

```
0.9.0-beta.1   →   0.9.0-beta.2   →   0.9.0-rc.1   →   0.9.0
```

A pre-release sorts **before** the release it leads to, so a tester on `0.9.0-beta.3` is
correctly offered the final `0.9.0` when it lands. (This is the ordering a naive string
comparison gets exactly backwards, which is why it is tested rather than assumed.)

**During the beta, every release is a pre-release.** If you want RUKUS to stay on a version
you have qualified, set updates to **Off** or **Notify me** — those are what they are for.

---

## What to do if an update breaks something

1. **Say so.** [Open an issue](../../issues) with the version you came from and the version
   you went to. A regression between two known builds is the most actionable bug report there
   is.
2. **Go back.** Every previous release stays on the Releases page. Download the older
   installer and run it over the top — the same as any other install.
3. Your data is not affected either way.

If a release turns out to be bad, it will be marked on the Releases page and the notes will
say what to do.
