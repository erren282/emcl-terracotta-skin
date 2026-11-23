# EMCL Terracotta Skin

Custom Terracotta multiplayer kernel used by the EMCL launcher. Based on
[Terracotta 0.4.2](https://github.com/burningtnt/Terracotta), extended with room-wide
skin and cape sharing and per-member client information.

## What this fork adds

- `POST /csl/skin` — accepts a `name + model + skin PNG + cape update` binary request
- Room skin store keeps an optional cape per player and serves it to CustomSkinLoader
  through the standard `CustomSkinAPI` profile (`cape` field + `/csl/textures/<hash>`)
- Guest mirrors reconcile locked names and cape changes from the host manifest
- `/meta` advertises `"skin_sync": 2` so the launcher can require a compatible kernel
- **Member details** (new in `0.4.2-skin.3`): every member publishes its game version,
  mod loader, mods, shaders and resource packs, which `/state` reports as a `details`
  array so the launcher can show what each player runs. Documents travel through the
  room, so they are rebuilt from a fixed schema on receipt (`src/member.rs`): unknown
  fields are dropped, strings and list lengths are capped, and a document that does not
  fit in 64 KiB is rejected outright.
- **`POST /member/details`** and **`POST /member/player?name=`**: the local launcher
  publishes its own details and in-game name.
- **Host/guest name adoption**: a room created from the web page has no in-game name, so
  the kernel registers the machine under a placeholder and the skin store keys uploads by
  that placeholder, which made every upload fail. The launcher now publishes the real name
  and the room renames its own profile on receipt.

The launcher (EMCL) installs CustomSkinLoader into the game instance, points it at the
local `/csl/` source and uploads the player's chosen skin and cape.

## Files

| File | SHA-512 |
| --- | --- |
| `terracotta/terracotta-0.4.2-skin.3-windows-x86_64-pkg.tar.gz` | `f73a2987785079cb319deb540b88ac9199385b4ae2186943d1d35f3afbe9b7f3c90e804afbd7e07f4ff07ae572c82b8e047575c063ce497491d3da467cde44cc` |
| `terracotta/terracotta-0.4.2-skin.2-windows-x86_64-pkg.tar.gz` | `21fed78d05360145f4821cd6383da19d1b8c90dad4e9d77697023b9af6f528f61c827c679ae24a63c24669ddc2246f0f8f4a992124aa76a6edc0fe9b5f4a7d13` |

Both packages are flat `tar.gz` archives; the launcher only extracts the files the
manifest names.

`0.4.2-skin.3` contains:

- `terracotta-0.4.2-skin.3-windows-x86_64.exe` — `9d758e12585fd5f35ee3a5e5bef60afd80444bb9cbb20a11f45f33b0a5e1c13fd230e7e56226d3f31aae8dfdb165bae01ecb99156e4c2b11efedab0a9af1be24`

It is built with the `x86_64-pc-windows-gnu` target and links only the Universal CRT
(`api-ms-win-crt-*`), which Windows 10 and later provide, so it ships without
`VCRUNTIME140.DLL`. Windows 10 is already the baseline the launcher enforces for
multiplayer.

`0.4.2-skin.2` (previous release) contains:

- `terracotta-0.4.2-skin.2-windows-x86_64.exe`
- `VCRUNTIME140.DLL`

## Download sources

- GitHub raw: `https://raw.githubusercontent.com/erren282/emcl-terracotta-skin/main/terracotta/terracotta-0.4.2-skin.3-windows-x86_64-pkg.tar.gz`
- Mainland mirror: `https://ghfast.top/https://raw.githubusercontent.com/erren282/emcl-terracotta-skin/main/terracotta/terracotta-0.4.2-skin.3-windows-x86_64-pkg.tar.gz`

These addresses are built into EMCL's `assets/terracotta.json`, so a fresh launcher
downloads and installs this kernel automatically when Terracotta is first opened.

## Manual install

1. Extract the package into `.hmcl/dependencies/terracotta/0.4.2-skin.3/`.
2. Create `.hmcl/terracotta-kernel.json` with:

```json
{"version":"0.4.2-skin.3"}
```

3. Restart the launcher. Multiplayer then runs the skin-capable kernel.

## Notes

- Windows x64 only. Other platforms use the official kernel.
- The official `terracotta-update` plugin compares numeric versions only, so it treats
  `0.4.2-skin.3` as `0.4.2`; installing an official 0.4.3 through that plugin would
  replace this kernel until it is re-selected.
- Upstream project: [burningtnt/Terracotta](https://github.com/burningtnt/Terracotta).
