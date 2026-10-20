# Tokyo Xtreme Racer - Save Editor

<img src="https://raw.githubusercontent.com/rafliard23/txr-use-py/refs/heads/main/img/txr_use-main_banner.png">

Yet, another Save Game editor for Tokyo Xtreme Racer.

Another Educational tool that I made for acquiring player data. At the moment, the current scope is limited to player data and owned vehicle data.

## Dependencies
+ ### Python 3.10
+ ### [TomSchimansky/CustomTkinter](https://github.com/TomSchimansky/CustomTkinter)  
+ ### [trumank/uesave-rs [CLI Tool]](https://github.com/trumank/uesave-rs)

## Installation

### Python
You can run this project simply by clone this project, install dependencies, for ```uesave.exe``` need to be installed in ```{project_dir}/bin```, and make new dir ```{project_dir}/bin/temp```, and run the app ```gui.py```

### Binary
You can download packaged version of the program in [Releases](https://github.com/rafliard23/txr-use-py/releases/). To run the app, extract the files and start the ```txr_use.exe```


## Fork notes

**Tested with Tokyo Xtreme Racer game versions 1.5 and 1.6.**

This fork makes the editor work with the save format written by current Tokyo Xtreme Racer builds:

- **Save container**: current saves are wrapped as `CMTA` + bare `GVAS` payload + a trailing little-endian CRC32 (computed over everything between the magic and the CRC). `utils/io.py` now strips the wrapper before `uesave to-json` and rebuilds it (with a fresh CRC32) after `from-json`. Older unwrapped saves still load and are written back unwrapped.
- **Python 3.11+ crash**: `set_player_data` used `str.__sizeof__()` comparisons as an "is empty" test, which only held on Python 3.10 and raised `ValueError` on newer versions. It now checks for empty strings directly.
- **Use uesave 0.6.2**: the app expects the JSON layout emitted by `uesave` 0.6.x. The 0.7.x releases use a different schema-based layout and are **not** compatible. Download `uesave_cli-x86_64-pc-windows-msvc.zip` from [trumank/uesave v0.6.2](https://github.com/trumank/uesave/releases/tag/v0.6.2) and put `uesave.exe` in `bin/` (this folder is git-ignored).

Known limitation (also in upstream): the "Unlock vinyls/emblems/auras" patch raises `KeyError: 'UnlockAuras_0'` on saves that don't contain that key yet.

Always keep a backup of your save; the editor writes one automatically (see below). Close the game (and pause Steam Cloud sync) before editing, otherwise the game overwrites your changes.

## Features
<details>
<summary>Click this section to expand all features</summary>

## Functionality
This save game editor lets you edit several parts of user' data that contained in the game' save file. As of Early Access version of the game, this tool capable to edit player stats and adjust player' car performance upgrades from the simple GUI.

### Automatic Backup
The app automate backup process of your unmodified save file before saving new changes when you save the edit.  
Backup file located in `%LOCALAPPDATA%/TokyoXtremeRacer/Saved/SaveGamesBackup/`.  
Backup save file stored as `UserData_00.sav.bak`. To revert your save file after an edit, simply delete the `.bak` extension and replace your save file from the backup directory.

### Player Stats Edit

<img src="https://raw.githubusercontent.com/rafliard23/txr-use-py/refs/heads/main/img/txr_use-player_data.png"  width="650">

While in theory you can almost edit any value that exposed in UserData_00.sav, I limited (vars that can be edited) to several variables to encourage players play the game as intended. Variables that included in my Save Editor are the following:
- Max Money (Working, the displayed value on player garage shows your current Wallet perk value, just ignore it)
- Money (CP)
- Accumulated money (Acc. CP)
- BP point
- Win count
- Loss count
- Draw count
- Win streak count
- Player' total mileage
- Day counter
- Garage size

### Vehicle Add/Remove from Garage

<img src="https://raw.githubusercontent.com/rafliard23/txr-use-py/refs/heads/main/img/txr_use-vehicle_list.png"  width="650">

Added in v1.2.0, now you can add/remove any cars from garage. Chassis ID manually inputted to make the app more future proof and modded cars with custom ID compatible


### Vehicle Performance Upgrades

<img src="https://raw.githubusercontent.com/rafliard23/txr-use-py/refs/heads/main/img/txr_use-vehicle_upgrades.png"  width="650">

Based on tested and dump data, we know maximum upgrades levels for each type, then I tried to write the current owned upgrade levels into something player desired. Car' mileage also added (in case you want to quickly finish certain wanderers).

All vehicle upgrades type and car' mileage data are editable. Car (chassis) swap is possible!

- Tune part upgrade
- Vehicle chassis swap
- Engine Swap (Added in v1.2.0)
- Aura Swap (Added in v1.2.0)
- Vehicle mileage

### Vehicle Preset

Added in v1.2.0, it is possible to share your car preset as file and import car preset into other instance of car. This feature was under [Livery Tool](https://github.com/rafliard23/txr-livery-editor-py) and now merged into save editor. All data that shared in the preset file:

- Car body paint
- Body stickers/vinyls
- Rear wing/GT Wing stickers/vinyls
- Window stickers
- Cosmetic brakes in aero section
- Tire dimension data
- Wheel dimension data
- Equipped Aero parts
- Tuning upgrade level and engine swap data
- Tuning setting data

### Patch

Added in v1.2.0, some edit now fall in "Patch" category that mostly one time use. This includes:

- Unlock all vehicle in dealership
- Unlock all emblem/vinyls into Livery editor (useful for fresh new save)
- Force unlock Engine swap and Aura perk
- Wanderer "fix" to prevent soft lock (Kurosaki Bros rematch, and Betelguese; You need to battle them at minimum once)

### Tooltip

<img src="https://raw.githubusercontent.com/rafliard23/txr-use-py/refs/heads/main/img/txr_use-tooltip.gif"  width="650">

In addition for separate wiki, in-app now also feature tooltip for quick help for editing values.

</details>

## Disclaimer
- To ensure minimal damage, always make a backup before processing a file.
- This software released purely for educational purpose. Author is not responsible for any damage outside of intended purpose of this software.
- This software has no affiliation with Tokyo Xtreme Racer (2025), or Genki Co., Ltd.
