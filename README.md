# Your server's launcher

A template for FiveM and Roblox server owners who want their own copy of the AETERNA game launcher
(MIT, source in [aeterna/nakara-oss](https://github.com/aeterna/nakara-oss/tree/main/launcher)). You edit one
file; GitHub builds the Windows exe for you.

## Steps

1. **Use this template** → create your own repository.
2. Edit `server.toml`: your server's id, name, game, address, operator and contact email. Every field is
   explained in [the owner guide](https://github.com/aeterna/nakara-oss/blob/main/launcher/docs/server-owners.md).
3. Commit. The `build` workflow checks the profile and builds the launcher. Download `launcher.exe` from the run's
   artifacts and try it.
4. Errors in the profile show up in the "Check server.toml" step with the field that is wrong.

A profile is data only: it cannot run code on a player's PC, and the launcher refuses any host the profile does not
name.

## Signed updates

Optional. With these steps, your players' launchers update themselves from your repository's releases, and refuse
any file not signed with your key. No Rust needed.

1. Download `launcher-sign-windows-x64.exe` from the
   [launcher release](https://github.com/aeterna/nakara-oss/releases?q=launcher-v) your `LAUNCHER_REF` names, and
   check it against that release's `SHA256SUMS`.
2. Make your key, once, on your own PC (not in a repository):
   ```
   launcher-sign-windows-x64.exe keygen --out my-launcher.key
   ```
   It prints your **public** key (64 hex characters). `my-launcher.key` is the private key: keep a backup offline.
3. In `server.toml`, set `release_repo = "<your-user>/<this-repo>"` and `update_keys = ["<the public key>"]`, and commit.
4. In this repository: Settings → Environments → New environment `launcher-release`. Add yourself as a required
   reviewer. Add the environment secret `LAUNCHER_SIGNING_KEY` with the contents of `my-launcher.key`.
   Or with the GitHub CLI: `gh secret set LAUNCHER_SIGNING_KEY --env launcher-release < my-launcher.key`.
5. Actions → **release** → Run workflow. It checks steps 3 and 4 first and says which one is missing. The release it
   makes is a draft: download it, try it on Windows, then publish it.

GitHub cannot create the key or the secret for you: a workflow token cannot write secrets, and the public key must be
in `server.toml` before the build. That is why steps 2–4 are by hand, once.

## Your responsibilities

- Tell players who runs your feed host and release repository:
  [what the launcher sends](https://github.com/aeterna/nakara-oss/blob/main/launcher/docs/privacy.md).
- Rockstar's Creator Platform License Agreement: no Rockstar, GTA or FiveM marks in the launcher's own name or
  branding, and no ads or sponsor slots. `operator` and `contact_email` are required and shown in the footer.
- Your build is yours. You sign it and you answer for it; the AETERNA project does not vouch for forks.

## Keeping up to date

`LAUNCHER_REF` in `.github/workflows/build.yml` and `release.yml` is the commit of the launcher release your build
uses (a commit, so a moved tag cannot change it). When a new launcher release is out, change both to its commit.
