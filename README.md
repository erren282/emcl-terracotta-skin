# EMCL Terracotta Skin

Custom Terracotta multiplayer kernel used by the EMCL launcher. Based on
[Terracotta 0.4.2](https://github.com/burningtnt/Terracotta), extended with room-wide
skin and cape sharing.

## What this fork adds

- `POST /csl/skin` — accepts a `name + model + skin PNG + cape update` binary request
- Room skin store keeps an optional cape per player and serves it to CustomSkinLoader
  through the standard `CustomSkinAPI` profile (`cape` field + `/csl/textures/<hash>`)
- Guest mirrors reconcile locked names and cape changes from the host manifest
- `/meta` advertises `"skin_sync": 2` so the launcher can require a compatible kernel

The launcher (EMCL) installs CustomSkinLoader into the game instance, points it at the
local `/csl/` source and uploads the player's chosen skin and cape.

## Files

| File | SHA-512 |
| --- | --- |
| `terracotta/terracotta-0.4.2-skin.2-windows-x86_64-pkg.tar.gz` | `21fed78d05360145f4821cd6383da19d1b8c90dad4e9d77697023b9af6f528f61c827c679ae24a63c24669ddc2246f0f8f4a992124aa76a6edc0fe9b5f4a7d13` |

The package is a flat `tar.gz` containing:

- `terracotta-0.4.2-skin.2-windows-x86_64.exe`
- `VCRUNTIME140.DLL`

## Download sources

- GitHub raw: `https://raw.githubusercontent.com/erren282/emcl-terracotta-skin/main/terracotta/terracotta-0.4.2-skin.2-windows-x86_64-pkg.tar.gz`
- Mainland mirror: `https://ghfast.top/https://raw.githubusercontent.com/erren282/emcl-terracotta-skin/main/terracotta/terracotta-0.4.2-skin.2-windows-x86_64-pkg.tar.gz`

These addresses are built into EMCL's `assets/terracotta.json`, so a fresh launcher
downloads and installs this kernel automatically when Terracotta is first opened.

## Manual install

1. Extract the package into `.hmcl/dependencies/terracotta/0.4.2-skin.2/`.
2. Create `.hmcl/terracotta-kernel.json` with:

```json
{"version":"0.4.2-skin.2"}
```

3. Restart the launcher. Multiplayer then runs the skin-capable kernel.

## Notes

- Windows x64 only. Other platforms use the official kernel.
- The official `terracotta-update` plugin compares numeric versions only, so it treats
  `0.4.2-skin.2` as `0.4.2`; installing an official 0.4.3 through that plugin would
  replace this kernel until it is re-selected.
- Upstream project: [burningtnt/Terracotta](https://github.com/burningtnt/Terracotta).
