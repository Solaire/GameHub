# GLC
## (GameLauncher Console)

This is a simple program that will scan the system for video games, then allow the user to launch any of these games from a single location - without having to store the icons on the desktop, or launching a dedicated client such as Steam or Epic. The program supports the following platforms:
- Amazon Games;
- Battle.net;
- Bethesda.net Launcher;
- Big Fish Games;
- Epic Games Launcher;
- GOG Galaxy;
- Indiegala Client;
- itch;
- Origin;
- Steam;
- Ubisoft Connect (formerly Uplay).

You can also launch custom programs (see below for instructions).

![](GLConsole.gif)

## Features

An interactive console allows the user to navigate via menus using the arrows and Enter (by default). An in-app help provides a list of other keys for other functions, e.g.:
- Launch a game;
- Flag as favourite;
- Create Desktop shortcut;
- Uninstall a game.
  - Uninstall not currently available for Epic, Indiagala, or itch games.

## Using
The app does not require installation, simply download the .exe and run it, however we would recommend putting the executable in a dedicated folder because several support files and folders are automatically created in the same location.

To manually add programs, place file shortcuts or .exe executables in the ".\customGames" folder found in the same directory as the application, and choose the rescan option to load them into the program. It is recommended to use shortcuts instead of .exes, as executables often require external assets and therefore will not work properly.

To customize images, place files with the same title in the ".\customImages" folder (supports ICO, BMP, PNG, GIF, JPEG, and TIFF formats).

You can edit glc-cfg.json to change the default configuration options, including custom keys, colours, text, input, layout, sorting, images, etc.

## Known Issues
- There are a few problems with shells not based on conhost.exe (including Windows Terminal)
   - No console output: PowerCmd;
   - Interactive mode not supported: Git Bash, Mingw, MSYS2 (though games can still be entered on the command-line);
   - Arrow keys not supported: ConEmu with Take Command (tcc);
   - Icons not supported: Console2, FireCMD, Take Command (tcmd), Windows Terminal;
   - Several shells don't respect colour settings.
- If you switch between grid and list view after an in-app search with no matches, there will sometimes be selection/keypress issues

## Building from source
Clone the repo and build. The program uses following nuGet packages:
- System.Text.Json for JSON parsing;
- System.Data.SQLite for parsing the itch database;
- Costura.Fody for merging the binary file with dlls.

## Contributing
You can support the project in the following ways:
- Identify and raise bugs and issues;
- Raise suggestions;
- Fork the repo, make some changes and submit a PR.

## Changelog
### Version 1.1.0
CHANGES:
- Change name back to "GameLauncher Console," change .exe to GLC for convenience
  - Note that tkashkin/GameHub (a Linux GUI game launcher) pre-existed the GameHub name change
- New icon (although it was based on the idea of a Game "Hub," so it should be re-thought)
- Improve Epic, Origin, and Steam implementations by reading library files instead of, or in addition to, registry
- Use grid view by default, now dynamic based on console width and height, and split into pages
- By default, sort by frequency (as before), but also secondarily by alphabet

NEW FEATURES:
- New platforms:
  - Amazon Games;
  - Big Fish Games.
- New functions:
  - Search;
  - Create Desktop shortcut;
  - Uninstall (not currently available for Epic, Indiegala, or itch).
- Screen paging with new navigation keys:
  - Home/End for first/last entry;
  - PgUp/PgDn between pages.
- New command-line parameters:
  - Enter a game name on the command-line to search (quotations marks not required for last parameter)
  -  /<#>  : e.g., "/2"; if there were multiple results in the prior search, run the game with a specified number;
  -  /1    : Replay the previously launched game;
  -  /S    : Rescan your games;
  -  /C    : Toggle command-line only mode;
  -  /P    : Add glc.exe location to your path;
  -  /?|/H : Display help.
- A settable alias for use with searching for each game (a default is set by stripping article (a/an/the), spaces, punctuation, and symbols from folder or .exe name)
- Show icons/images for games and platforms, either the icon from the detected .exe, or from a custom image placed in ".\customImages" folder
  - Note this only works with shells using conhost, e.g., cmd or PowerShell, but not Windows Terminal and some third party shells.
  - The following extensions are recognized for custom images:
    - .ico;
    - .bmp;
    - .png;
    - .gif;
    - .jpg / .jpe / .jpeg;
    - .tif / .tiff.
- Attempt to recognize unsupported shells (see known issues above)
- Add config .json file for user-configurable settings
  - Custom keys (2 for each function);
  - Custom colours (2 sets, for dark/light mode);
  - Custom text;
  - Settings for input, layout, sorting, images, etc.

### Version 1.2.0 alpha
- New platforms:
  - Indiegala Client;
  - itch.
- New feature: Export all games of a platform as shortcuts to a given folder [ISSUE #9]
- New feature: In-app search
  - Working fairly well, but switching between grid and list after a search with no matches sometimes causes selection/keypress issues
- Ensure multiple library locations are supported and remove reliance on Windows uninstall registry where possible [ISSUE #18]
  - Note: Migrating an Epic library requires you to move or update the .item files in "C:\ProgramData\Epic\EpicGamesLauncher\Data\Manifests"
- Better handling of missing/malformed configuration entries (including changes between revisions)
  - Add missing entries and set to defaults;
  - Note a version number has been added to aid in this;
  - Ensure there are no duplicate/invalid titles, aliases, and hotkeys (also test for conflicts with built-ins, e.g., F11).
- For configuration, switch to .ini instead of .json

## In progress changes:
- For games, switch to SQLite database instead of .json [ISSUE #6]
- Small icons to left in single-column list mode
  - Disabled currently, as icons are randomly missing, especially after going to previous page or back to previous menu.
- Live switch between sorting modes
- Hide/unhide game (instead of current behavior of removing it from the database)
  - Disabled currently, as hidden games mess up enumeration.
- Remove article (a/an/the) for alphabetic sort
- Code clean-up; better comments; finish updating function summaries