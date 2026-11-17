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

## Updates and releases

Signed updates need your own signing key and a release workflow. Follow
[Releasing the launcher](https://github.com/aeterna/nakara-oss/blob/main/docs/maintainers/launcher-release.md) with
your own repository, environment and secret. Without `release_repo` and `update_keys` the launcher never checks for
updates.

## Your responsibilities

- Tell players who runs your feed host and release repository:
  [what the launcher sends](https://github.com/aeterna/nakara-oss/blob/main/launcher/docs/privacy.md).
- Rockstar's Creator Platform License Agreement: no Rockstar, GTA or FiveM marks in the launcher's own name or
  branding, and no ads or sponsor slots. `operator` and `contact_email` are required and shown in the footer.
- Your build is yours. You sign it and you answer for it; the AETERNA project does not vouch for forks.

## Keeping up to date

`LAUNCHER_REF` in `.github/workflows/build.yml` names the launcher release your build uses. Change it when a new
launcher release is out.
