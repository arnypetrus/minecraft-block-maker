# Minecraft Block Maker

A small point-and-click tool for making Fabric mods that add **new decorative blocks** to Minecraft. Upload textures, assign them to block faces, set a few properties, optionally add a crafting recipe, and export a mod you can build into a `.jar`.

**Supported versions:** Minecraft 26.3 and 1.21.8 (Fabric). This may change in the future.

## Requirements

- A modern browser (to use the tool).
- To build the jar: a **JDK** and an internet connection (the first build downloads Minecraft and Fabric, which can take several minutes).
  - Minecraft **1.21.8** needs **JDK 21** or newer.
  - Minecraft **26.3** needs **JDK 25** or newer.
- To play: Minecraft with **Fabric Loader** and **Fabric API** installed for the same Minecraft version.

## Quick start

1. Open `minecraft-block-maker.html`.
2. Click **New**. Enter a mod name, a mod ID (lowercase letters, digits and `_`, starting with a letter), pick the Minecraft version, and optionally an author.
3. Click **+ Upload** under *Textures* and add one or more PNG files. Texture names come from the file names.
4. Click **+** next to *Blocks*. Set the name, ID and block type, then assign a texture to each face.
5. Adjust properties and, if you want, add a crafting recipe.
6. Click **Save .mcbm** any time so you can come back later.
7. Click **Export mod**. This downloads `<modid>-mod-project.zip`.
8. Build the jar (see below).
9. Put the jar in your Minecraft `mods` folder.

## Building the jar

### Option A: the Packager (recommended)

1. Put `Packager.java` in the **same folder** as the exported zip.
2. Open a terminal in that folder.
3. Run:

   ```
   java Packager.java
   ```

4. When it finishes, `<modid>-1.0.0.jar` appears next to the zip.

Notes:

- It uses the newest `*-mod-project.zip` in the folder. To pick one explicitly: `java Packager.java path/to/file.zip`.
- It builds inside a `<zipname>-build` folder next to the zip. Re-running after a new export replaces the old blocks and reuses the downloaded Minecraft files, so rebuilds are much faster than the first one.
- It reads the JDK version the project needs. If you launch it with an older Java, it stops and tells you which JDK to install and how to run it with that one.
- If the build fails, Gradle's full error output is printed in the terminal.

### Option B: by hand

Unzip the project, then run this in the unzipped folder:

- Windows: `gradlew.bat build`
- macOS / Linux: `./gradlew build`

The jar is in `build/libs/<modid>-1.0.0.jar` (ignore the `-sources` jar). You can also test straight from the project with `gradlew runClient`. The exported zip includes a `BUILD.txt` with the same steps.

## Features

### Block types

- **Full block**: a normal cube with up to six different face textures.
- **Pillar**: log-style, rotates depending on which way it's placed. Has *Ends* and *Sides* textures.
- **Directional**: faces the player when placed (like a furnace). Has a *Front* texture plus the other faces.
- **Slab**: bottom, top and double slab, with *Top*, *Bottom* and *Sides* textures.
- **Stairs**: all shapes and orientations, with *Top*, *Bottom* and *Sides* textures.

### Textures

- Upload multiple PNGs at once. Reuse the same texture across as many faces and blocks as you like.
- **Set all faces** applies one texture to every face of a block.
- Any face you leave unassigned reuses the block's first assigned texture.
- Deleting a texture unassigns it from every face that used it.

### Block properties

- **Hardness** and **blast resistance**
- **Light emission** (0-15)
- **Sound** (stone, wood, gravel, grass, sand, metal, glass, wool, snow, and more)
- **Best tool** (pickaxe, axe, shovel, hoe, or any) and **needs correct tool** to drop items
- **Transparent**: glass-like blocks with see-through pixels
- **Unbreakable**

Every block drops itself when broken (slabs drop two when broken as a double slab), and shows up in the **Building Blocks** creative tab.

### Crafting recipes

Tick **Craftable** on a block to open the recipe editor:

- Choose **shaped** (pattern matters) or **shapeless** (any arrangement).
- Set the result amount.
- Fill the 3x3 grid. Leave cells empty for blank slots. Shaped recipes are trimmed to the smallest pattern, so a 2x2 in the corner becomes a 2x2 recipe.

Ingredient formats:

- Item IDs: `stone` or `minecraft:iron_ingot` (the `minecraft:` prefix is optional).
- Tags: `#minecraft:planks`.
- Your own blocks: `<modid>:<block_id>`. If you later rename a block's ID, recipes that use it are updated automatically.

Need an item ID? The recipe card links to <https://crafty.gg/tools/item-ids>, where you can pick your Minecraft version.

Recipes work in a crafting table but are not registered with recipe-book unlock advancements, so they won't appear in the recipe book until you've discovered the ingredients.

### Projects (`.mcbm`)

- **Save .mcbm** stores everything (blocks, settings, recipes, and your textures) in one file.
- **Open** loads it so you can keep adding blocks.
- The Minecraft version can be changed later from the dropdown on the project card; the same project exports for either version.

## What Export does

Export produces a complete, ready-to-build Fabric project: the official Fabric example-mod template for the chosen version, plus generated Java, block models, blockstates, item definitions, loot tables, recipes, mining-tool tags, English names, and your textures. Nothing is uploaded anywhere; everything happens in your browser.

## Known limitations

- **No direct `.jar` from the browser.** A jar needs compiled Java and the Minecraft toolchain, which a web page can't do. That is why the Packager exists.
- Decor blocks only: no fences, walls, doors, block entities, custom creative tabs, or 3D preview.
- Textures should be PNG, normally 16x16 (or another power-of-two size).

## Troubleshooting

| Problem | Try this |
|---|---|
| `No *-mod-project.zip found` | Run the Packager from the folder that contains the exported zip. |
| "This mod needs JDK 25" (or 21) | Install that JDK and launch with it: `<path-to-jdk>/bin/java Packager.java`. |
| First build takes a long time | Normal; it downloads Minecraft and Fabric once. Later builds are fast. |
| Block shows a purple-black checkerboard | A texture is missing or a name doesn't match. Re-check the face assignments and re-export. |
| Game won't start with the mod | Make sure Fabric Loader and Fabric API for the **same** Minecraft version are installed. |
| Export says a block has no textures | Assign at least one texture to that block. |
| Export says two blocks share an ID | Give each block a unique ID. |

## For full transparency:

This tool was created using Claude Code.
