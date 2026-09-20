# voidcode-windows

**Windows installers for [VoidCode](https://github.com/). This repository holds no source code.**

Every release here is a `.exe` installer built from the VoidCode monorepo and a `SHA256SUMS.txt`
listing its checksums. Nothing else. The application, its issue tracker and its history are in the
main repository; this one exists so that downloading VoidCode for Windows is one page with one file
on it.

→ **[Latest release](../../releases/latest)**

## Which file

| Your PC | File |
|---|---|
| Most PCs (64-bit Intel or AMD) | `VoidCode-<version>-win-x64.exe` |
| Snapdragon, Surface Pro X | `VoidCode-<version>-win-arm64.exe` |

Settings → **System → About**, under **System type**. ARM applies only to Snapdragon and Surface Pro
X machines; if you are not sure, you want x64.

## Installing

1. Run the `.exe`. It installs for your user account — **no administrator rights needed**.
2. SmartScreen appears: *"Windows protected your PC — Microsoft Defender SmartScreen prevented an
   unrecognised app from starting."*
3. There is no **Run anyway** button visible. It is behind **More info**.

Nothing else is needed: the application carries its own Python runtime, so there is no separate
install and nothing to add to `PATH`.

### Why that prompt appears

The installer is **not code-signed**. SmartScreen is a *reputation* check, not detection: it says
the same thing about every unsigned installer that nobody has downloaded yet, and the warning fades
as a file accumulates downloads. Signing would silence it without making the file any different.

A code-signing certificate costs money annually. SignPath is free for open-source projects and is
the intended fix; until then the prompt is documented rather than hidden, which seems better than
pretending it does not happen.

If you would rather not click through it, build from source — the main repository has the commands.

## Verifying what you downloaded

An unsigned installer means you cannot tell who built it from the file itself. The checksums are the
substitute, and they are worth thirty seconds because they are the only provenance available.

```powershell
cd ~\Downloads
Get-FileHash VoidCode-*.exe -Algorithm SHA256 | Format-List
```

Compare the `Hash` against the matching line in `SHA256SUMS.txt`. PowerShell prints uppercase hex
and the file lists lowercase — the same value, and the comparison is case-insensitive. To check it
without reading two columns by eye:

```powershell
$want = (Select-String -Path SHA256SUMS.txt -Pattern 'win-x64\.exe').Line.Split(' ')[0]
$got  = (Get-FileHash VoidCode-*-win-x64.exe -Algorithm SHA256).Hash
if ($got -ieq $want) { "OK" } else { "MISMATCH — do not install this" }
```

Anything other than `OK` means do not install it.

## Releases are drafts until a human publishes them

CI attaches the installers here as a **draft**. A person reads the build, checks it, and publishes.
That is deliberate: the download page reads `releases/latest`, which excludes drafts, so a bad build
cannot become the thing visitors are handed before anyone has looked at it.

## What is not here

- **No source code.** It is in the main repository, under Apache-2.0.
- **No installers in git.** They are release assets. A repository carrying binaries grows without
  bound and makes `git clone` a download of every version ever shipped.
- **No macOS or Linux builds.** macOS is in the macOS repository; Linux (AppImage and `.deb`) is
  attached to the main repository's own releases.

## Licence

Apache-2.0. See [LICENSE](LICENSE) and [NOTICE](NOTICE).
