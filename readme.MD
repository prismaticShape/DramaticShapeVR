# Official Dramatic Shape VR for Quest 2/3 Standalone and PCVR

A standalone VR/MR application for Gen1 -- contains passthrough/diorama/first person modes.

### VR controls

Suggested onto Touch, Index and WMR controllers (rebindable in the
runtime's own binding UI); pad, keyboard and mouse all keep working
alongside.

| control | does |
| --- | --- |
| left stick | move — grid-walks the diorama, free-walks 1ST |
| A / B (X / Y on the left hand) | A / B |
| either trigger | START |
| left stick click | step to the next VR mode — DIORAMA → DIORAMA-MR → 1ST PERSON → DIORAMA (in horde mode: ask to leave) |
| right stick click | step **V-CURVE** — OFF → 1 → … → 5 → OFF (the bend shows in the dioramas) |
| left menu button | toggle the **controls card** — the gist of this table, floating in view; in game only, and it steps aside for any menu |
| right stick up / down | *tabletop* — zoom the model; *during a diorama fight* — resize the fight |
| right stick left / right | *tabletop* — open or close the **viewport**, the grip-spread's dial; *during a diorama fight* — resize the fight; *1ST* — snap-turn 45°, or turn smoothly with **SMOOTH TURN** on |
| one grip squeezed | *DIORAMA* — carry the model wherever that hand goes |
| both grips squeezed | *DIORAMA only* — turn the model with your hands, and open or close the viewport by spreading them; *during a fight* — turn and resize the fight instead (the table keeps its own turn), and the fight remembers |
| head | *1ST and battles* — look; FreeMove walks where you look |
| left hand | *every mode* — the Pokédex: menus and dialogs on its screen, and the classic battle frame (both HUDs and the text box) through a fight |


## Voice commands

Say the move and your Pokemon uses it. At the battle menu -- FIGHT / PKMN /
ITEM / RUN, or the move list under FIGHT -- "use Thundershock" picks
THUNDERSHOCK, "Pikachu, use Quick Attack!" works just as well (the name is
ignored, the move is what counts), and at the main menu "fight" and "run
away" do what they say. A move said at the main menu opens FIGHT and lands
the move by itself. Long move names can be said bare ("Thunderbolt!"); a
short one wants its "use" in front, because "cut", "dig" and "fly" are
ordinary words.

The row is **BATTLES.. > VOICE** (ON by default). The microphone is open
*only* while that menu is up: nothing is heard on the map, in a message, or
outside a battle, and nothing leaves the headset -- the recogniser is
on-device, a 3.3M-parameter keyword spotter
([sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx), Apache-2.0) that is
told the four moves on the card each turn and can decode nothing else.
Decoding runs on its own thread; the game's frame never waits for it.

On the headset the first launch asks for the microphone; say yes. The
headset's own microphone switch (the mic icon in quick settings, or
Settings > Privacy) has to be on as well: switched off, Android hands
every app silence rather than refusing, and the mod logs that once at
the first battle menu. Deny the permission
and voice stays off for that session (LÖVE puts up one "mic permission
missing" notice at the first battle menu; grant the permission in the
headset's app settings to turn it back on). On Windows the mod folder needs
the two native libraries under `voice/win-x64/` -- run
`tools/fetch_voice_libs.sh` once -- and any microphone Windows knows about
is used. The vocabulary is cut ahead of time by `tools/voice_tokens.py`
into `data/voice_tokens.lua`; on a synthesised-speech battery of every
move it hears about nine in ten, never picks the wrong move, and never
fires on unrelated speech.

## Safety

Ensure your play area is clear of obstacles.

## Licenses
It redistributes one third-party binary:

- **`assets/vr/openxr_loader.dll`** — the Khronos OpenXR loader
  (version 1.0.10.2, x64, unmodified), © The Khronos Group Inc.,
  licensed under the **Apache License 2.0**. The full license text ships
  alongside the DLL at
  [`assets/vr/LICENSE-openxr_loader.txt`](assets/vr/LICENSE-openxr_loader.txt),
  as the license requires; keep the two files together if you
  redistribute this mod. Source:
  [KhronosGroup/OpenXR-SDK](https://github.com/KhronosGroup/OpenXR-SDK).
- **`assets/voice/kws-en/`** -- the keyword-spotting model VOICE listens
  with: `sherpa-onnx-kws-zipformer-gigaspeech-3.3M-2024-01-01` by the
  k2-fsa / icefall team, trained on GigaSpeech, licensed under the
  **Apache License 2.0** (the text is at
  [`assets/voice/LICENSE-apache-2.0.txt`](assets/voice/LICENSE-apache-2.0.txt);
  keep it with the model). The Quest APK also carries the two libraries
  that run it, neither of which is in this repository:
  [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx) (Apache-2.0) and
  [ONNX Runtime](https://github.com/microsoft/onnxruntime) (MIT,
  [`assets/voice/LICENSE-onnxruntime.txt`](assets/voice/LICENSE-onnxruntime.txt)),
  fetched by `tools/fetch_voice_libs.sh` at their pinned release.
- **`lib/HandMeshData.lua`** — the GO-mode hands: the `ash_hands_webxr`
  pair (Ash-style fingerless gloves modelled onto the Godot XR Tools
  hands), baked static in their `hold` pose. Base hand mesh, rig and
  pose library **CC0 1.0** by DigitalN8m4r3 (Miodrag Sejic); the glove
  package is **CC0 1.0** as well.
- **`lib/PokeballMesh.lua`** — the Poke Ball prop the LET'S GO catch
  mode throws and GO mode holds: "Pokemon Basic Pokeball" by
  **JMGameDev**, licensed **CC BY 4.0**
  (<https://creativecommons.org/licenses/by/4.0/>), from
  [Sketchfab](https://sketchfab.com/3d-models/pokemon-basic-pokeball-9b29539199c14ddea4de7776c4d758df).
  The mesh is split at its seam into hinging halves and rewritten into
  the mod's own vertex format by `tools/glb_bake_pokeball.py`; that is a
  modification, and the attribution above must travel with it.
Everything else in this mod is original to it, except that the voxel
geometry and shape profiles are derived from the tile and sprite data of
the original game, as documented by the
[pret/pokered](https://github.com/pret/pokered) disassembly. No ROM
data, artwork or audio is included; the mod reads the assets the host
game already has.


### Acknowledgements — pret/pokestadium

The STADIUM battle models are read out of the player's own Pokémon Stadium
(US) 1.0 cartridge by original code in [`lib/`](lib) and
[`model_extract/`](model_extract). **That code exists because of
[pret/pokestadium](https://github.com/pret/pokestadium)**, the community
decompilation of that game, which is the reference this mod's reader was
written against. Specifically, it is where the following came from:

- the bone matrix chain, and the fact that scale is kept *out* of it and
  applied only at draw time (`func_800143C0`) — the single most important
  thing to get right in the whole rig, and not guessable from the data
- the rotation basis and its row-vector `Rx·Ry·Rz` order
  (`func_8000F730`, `src/F420.c`)
- the animation player's frame counter and loop-start behaviour
  (`func_80016FBC`), and the texture-animation sampler that *clamps* past
  the end of its stream rather than wrapping (`func_80017540`) — which is
  the difference between a Pokémon blinking and twitching
- the battle system's per-species animation context slots and the routines
  that select them (`func_8432B0A4`, `func_8430506C`, `func_84305A74`)
- the move-id constants the per-species move table is keyed by

**No code, data or asset from that project is included in or redistributed
by this mod**, and none is needed to build or run it. What was taken is an
understanding of the file formats, re-expressed in this mod's own Lua and
Python. If you want to reuse anything from the decompilation itself, get it
from upstream and follow that project's own terms.

No Pokémon Stadium ROM data ships here either. The models are built on the
player's own machine, from a cartridge they supply, into their own save
directory — see [Getting the models](#getting-the-models).


## Credits
Dramatic Shape and derivative code is considered source-available rather than open source. Gen1Recomp engine was forked from Gen1Recomp by bryanthaboi and BOIS CLUB GAMES, LLC. The Generation I engine, mod platform, renderer, and tooling are theirs; All code pertaining to the Virtual Reality functionality, and the mod under /DRAMATIC_SHAPE packaged within the release are owned by prismaticShape.

Follower sprites created by anima-nel. These are not included in the repository, but are shipped within the application.

## Licence in one paragraph
Two parts. Everything inherited from Gen1Recomp, and every shared engine file since, stays MIT under its original authors' copyright — that is the default. VR components portions of the APK and DRAMATIC_SHAPE mod written for this fork are source-available rather than open source (© 2026 prismaticShape): read them, run them, change your own copy, but don't redistribute or fork them.
