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

A RUKUS version tells you what the build is. Three numbers:

```
26 . 91 . 13026
 │    │    └──────── the day the work started, then the issue number:  13 and #026
 │    └───────────── the month the work started, then the kind of change:  September, 1
 └────────────────── the year:  2026

                     the kind of change:  1 feature · 2 bug fix · 3 maintenance · 4 docs · 9 a release
```

So `26.91.13026` is a **feature**, for **issue #26**, that was started on **13 September 2026**.
To read the second number, split off the last digit: `91` is September, feature; `101` is
October, feature. To read the third, split off the last three digits: `13026` is day 13, issue
26; `5004` is day 5, issue 4.

A **release** has 9 as its kind and just a release number: `26.99.1` is September 2026's first
release, `26.99.2` the second. A release is always a higher number than the working builds of its
month, so an update to it is always offered.

```
0.9.0-beta.3   →   26.9.13026.1   →   26.91.13026   →   26.92.19005   →   26.99.1   →   26.101.2031   →   27.13.5040
```

(`26.9.13026.1`, with the kind as a fourth number, was the form used for three days in September
2026.)

The numbers compare as numbers, left to right, so a new month is always newer than anything
from the month before, and every one of them is newer than the `0.9.0-beta.n` builds that came
first. RUKUS only ever offers you a version higher than the one you have, and a release is
checked against everything already published before it goes out. (A naive string comparison
would get `26.10.2031.1` vs `26.9.19005.2` backwards, which is why the ordering is tested
rather than assumed.)

RUKUS is still a beta; the number says what a build is and when the work began, not that it has left beta. If you
want RUKUS to stay on a version you have qualified, set updates to **Off** or **Notify me** —
those are what they are for.

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
