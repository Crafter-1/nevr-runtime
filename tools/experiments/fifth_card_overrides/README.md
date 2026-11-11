# Fifth-card override set (dev-only experiment)

Evidence for the design in issue 1857, plan step 2: a fifth main-menu card (PLUGINS) added as data, with no runtime
code change. These 12 files are the recipe. They are not shipped, not read by any build, and not a design for the
final card.

## What the files are

Each file is a resource override for the main-menu level (`mnu_master`, name hash `0xac360e41e4ede056`), named
`0x<resource type>.0x<level name>` as the resource override registry expects (`_overrides/` next to `echovr.exe`).
Together they clone a card actor into one of the scene's 57 actor-less entity slots:

- 11 resource edits: transform (the card sits at x 0, y 2.7, z 5.85, facing front, above the row of four tiles),
  blackboard, bounding sphere, canvas UI, pointer interface, pointer mesh, page element, texture override, actor data
  and two script entries.
- the scene resource (type `0x29edbd196a3c7457`) with the new actor's hash in the entity table, which is sorted by
  signed 64-bit hash with the slot staying with the hash. The same replaced hash also sits in a 114-entry array and
  a 55-entry table of the scene resource, and all three are changed.

## SHA-256

```
aaf003710a9c5ea48c53c7b352d59a5931bc459d1863f43e446176922428cd35  0x138348caeb778898.0xac360e41e4ede056
3a4f109c3f7f577ad0bc0d027de600d8e0c088f8f04d2414fab1ab1204ebb60f  0x24795e5c32f2ce06.0xac360e41e4ede056
dd629f0e29f58821d47f8b6438a82e2550bc7618baeaef6614665e3ef0161967  0x29edbd196a3c7457.0xac360e41e4ede056
736a955e97baef3801996b51de04422f6da16ae92fb72048a7e38b2364a176f5  0x33a2f9f78c9884c4.0xac360e41e4ede056
d527031b07053b8837317f311762e2274db8c58955ce681ab9fd48fefa70bd38  0x3585dd9b44cb17ac.0xac360e41e4ede056
9d76dc9594a8f7c089661d8d3c7a4d0cbcd2ea8ad957d83591bca2c41094239b  0x4e711e5bf26532e7.0xac360e41e4ede056
f2592dee6a258d468efe2a4648e3c6381af757222512d06c09da02dcc85b4a8b  0x75e5993f80940cd0.0xac360e41e4ede056
2fdc211294e3b574d5e2bf295b0089b40ead90d5fcdfb4798a1c3278111bd451  0x95cb063ce78ab998.0xac360e41e4ede056
482f43fa75f110747729c7cb88f3b2bd4c7ff700bfe331d5b22f9b326f4073a4  0x9c72178d05adb686.0xac360e41e4ede056
677da8b23afc342da37f104ca7ade1f90cc680f4b8df8f1fef61bbd8851ddf27  0xa61c15604ed2e050.0xac360e41e4ede056
6ba12fc772d92a3e44db4985e5abe3ffd04bb937480871395b5fb213930ee0f7  0xa691ec467180d00c.0xac360e41e4ede056
b1a2783e0a8ddff2313cd52962f902afe1e6ac9b1ec57f1dfcd8ff94d4a7c74b  0xb374f7689b3f8f24.0xac360e41e4ede056
```

## How it was used

Copied into `_overrides/` of a throwaway copy of the game install, with a dev build (`mingw-plugin-dev`) of the
runtime and a production sign-in. Result: `overrides_registered=12 overrides_failed=0`, the card shows with its text,
CREATE GAME, JOIN VIA CODE and PLAY still work. Pressing the card starts the tutorial, because the clone keeps the
TUTORIALS card's press action.

## Known open

- The level resource (`CGameLevelResource`) still holds the replaced hash once, at byte offset 1912.
- No lobby baseline run without the card, on the same build, has been compared yet.
- Whether 256 entries is a limit, and which of the 11 edits the card needs, are untested.
