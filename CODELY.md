# CODELY.md — U3 SDK (Unturned) Project Context

## Project Overview

**Unturned** — a free-to-play open-world zombie survival sandbox game developed by [Smartly Dressed Games](https://smartlydressedgames.com/). The U3 SDK is the open-source codebase for the game, built in Unity.

- **Unity Version:** 2022.3.62f3 (LTS), revision `96770f904ca7`
- **Render Pipeline:** Built-in / Legacy (with Post Processing Stack v2)
- **Target Platforms:** Windows (32/64-bit), macOS 64-bit, Linux 64-bit (dedicated server builds supported)
- **Steam App ID:** 304930
- **Company:** Smartly Dressed Games Ltd.
- **License:** U3 SDK License Agreement — mods must be free and non-commercial; PC platforms only (no consoles/mobile). See `LICENSE.txt`.

## Key Scenes & Entry Point

| Scene | Path | Purpose |
|-------|------|---------|
| **GameStartup** | `Assets/GameStartup.unity` | Main entry point — open this scene and press Play |
| GlazierPoolTest | `Assets/Tests/Scenes/GlazierPoolTest.unity` | UI framework pool testing |
| GlazierTest | `Assets/Tests/Scenes/GlazierTest.unity` | UI framework testing |
| GlazierUIToolkitTest | `Assets/Tests/Scenes/GlazierUIToolkitTest.unity` | UI Toolkit integration testing |
| Sky | `Assets/Tests/Scenes/Sky.unity` | Sky/weather visual testing |

## Getting Started

1. Clone this repository
2. Install [Unity Hub](https://unity.com/download) and the **Unity 2022.3.62f3** editor
3. *Optional:* Install Visual Studio with **Game development with Unity** + **.NET desktop development** workloads
4. Ensure **Steam is running** and [Unturned](https://store.steampowered.com/app/304930/Unturned/) is installed (large binary files and mods are loaded from the Steam install)
5. Open the project in Unity Hub
6. Open the `Assets/GameStartup.unity` scene
7. Press **Play**

## Core Architecture

### Assembly Boundaries (asmdef)

The project is split into multiple assemblies:

| Assembly | Path | Purpose |
|----------|------|---------|
| **Assembly-CSharp** | `Assets/Runtime/Assembly-CSharp/` | Main game code (no asmdef — Unity default assembly) |
| **SDG.NetPak** | `Assets/Runtime/SDG.NetPak/` | Network packet serialization |
| **SDG.NetTransport** | `Assets/Runtime/SDG.NetTransport/` | Network transport abstraction (SteamNetworkingSockets, SystemSockets, UNet, Loopback) |
| **SDG.Glazier** | `Assets/Runtime/SDG.Glazier/` | UI abstraction layer (IMGUI, uGUI, UI Toolkit backends) |
| **SDG.HostBans** | `Assets/Runtime/SDG.HostBans/` | Host banning system |
| **UnityEx** | `Assets/Runtime/UnityEx/` | Unity engine extensions |
| **SystemEx** | `Assets/Runtime/SystemEx/` | .NET system extensions |
| **UnturnedDat** | `Assets/Runtime/UnturnedDat/` | .dat file format parser for game data |
| **LiveConfig** | `Assets/Runtime/LiveConfig/` | Runtime configuration |
| **Assembly-CSharp-Editor** | `Assets/Editor/Assembly-CSharp-Editor/` | Editor-only tools, importers, build scripts |
| **UnityEditorEx** | `Assets/Editor/UnityEditorEx/` | Unity editor extensions |
| **Jenkins.Editor** | `Assets/Editor/Jenkins/` | CI/CD integration |
| **SteamCmd.Editor** | `Assets/Editor/SteamCmd/` | SteamCMD automation |
| **com.rlabrecque.steamworks.net** | `Assets/com.rlabrecque.steamworks.net/` | Steamworks.NET (Steam API wrapper) |

### Main Code Structure (`Assets/Runtime/Assembly-CSharp/`)

```
Assembly-CSharp/
├── Unturned/                 # Core game logic (namespace: SDG.Unturned)
│   ├── Animals/              # Animal AI and behavior
│   ├── Attachments/          # Gun attachment system
│   ├── Bundles/              # Asset loading, types, and definitions
│   ├── Characters/           # Character system
│   ├── Command/              # Server console commands (70+ commands)
│   ├── Constants/            # Shared constants (color palette, etc.)
│   ├── Damage/               # Damage types and handling
│   ├── Decals/               # Decal system
│   ├── Edit/                 # In-game editor
│   ├── Files/                # File system helpers
│   ├── Game/                 # Game modes (Survival, Arena), MainCamera
│   ├── Interactable/         # All interactable objects (doors, storage, vehicles, NPCs, etc.)
│   ├── Inventory/            # Item/inventory system, blueprints, crafting
│   ├── Level/                # Level/world system (nodes, volumes, spawns, weather, roads, lighting)
│   ├── Loading/              # Loading screens
│   ├── Managers/             # Core managers (Barricade, Item, Structure, Vehicle, Zombie, Chat, etc.)
│   ├── Menu/                 # Main menu UI and logic
│   ├── ModHooks/             # Modding event hooks and spawner components
│   ├── Network/              # Network snapshot buffer, rate limiting
│   ├── Player/                # Player system (life, movement, inventory, skills, quests, clothing, etc.)
│   ├── Provider/             # Service provider abstraction
│   ├── Regions/              # Region/tile management
│   ├── ServerListCuration/   # Server list filtering
│   ├── Settings/             # Game settings (graphics, controls, audio, etc.)
│   ├── Sleek/                # Custom UI framework (Glazier-based)
│   ├── Throwables/           # Throwable items
│   ├── Tools/                # Utility tools (damage, physics, highlighting, pooling, etc.)
│   ├── UI/                   # In-game UI (menu, player, edit)
│   ├── Useable/              # Useable item behaviors (gun, melee, consume, barricade, etc.)
│   ├── Utils/                # General utilities
│   ├── Zombies/              # Zombie AI and behavior
│   └── UnturnedNexus.cs      # Module nexus — registers all asset & useable types
├── Framework/                # Framework layer (namespace: SDG.Framework)
│   ├── Debug/                # Debug tools
│   ├── Devkit/               # Developer toolkit
│   ├── Extensions/          # Extension methods
│   ├── Foliage/             # Foliage system
│   ├── IO/                  # File I/O helpers
│   ├── Landscapes/          # Landscape/terrain tools
│   ├── Modules/             # Module system (IModuleNexus)
│   ├── Rendering/           # Rendering helpers
│   ├── Utilities/           # General utilities
│   ├── Water/               # Water system
│   └── FrameworkNexus.cs    # Framework module entry point
├── General/                  # General-purpose MonoBehaviours
├── Provider/                 # Provider service interfaces
├── SteamworksProvider/      # Steam-specific provider implementation
├── NetGen/                  # Network code generation (enums, invokables)
├── NetInvokable/            # Network invokable RPC system
├── NetMessaging/            # Network messaging
├── NetPak/                  # Network packet system
├── NetTransport_*/          # Network transport implementations
├── Glazier*/                # Glazier UI backends (IMGUI, uGUI, UIToolkit)
├── ItemStore/               # Economy/item store
├── CustomPostProcess/       # Custom post-processing effects
└── SteamworksProvider/      # Steamworks integration
```

### Key Entry Points

- **`UnturnedNexus.cs`** — Registers all built-in asset types (Gun, Food, Vehicle, etc.) and useable types (Gun, Melee, Consumeable, etc.) at startup via `IModuleNexus`.
- **`MainCamera.cs`** — Central camera singleton with instance/availability change events.
- **`Player.cs`** — Core player class composing all player sub-systems (life, movement, inventory, skills, etc.).
- **`Level.cs`** — Level/world management singleton.
- **`Assets.cs`** — Asset registry and loading system.

### Networking Layer

The game uses a custom networking stack (not Unity Netcode or Mirror):

- **NetPak** — Binary packet serialization
- **NetInvokable** — RPC-style network invokables (client→server, server→client)
- **NetTransport** — Abstraction over multiple transports:
  - `NetTransport_SteamNetworkingSockets` (primary)
  - `NetTransport_SteamNetworking` (legacy)
  - `NetTransport_SystemSockets` (raw TCP)
  - `NetTransport_UNetLLAPI` (legacy UNet)
  - `NetTransport_Loopback` (single-player)

### Asset System

Assets are loaded from **Master Bundles** (Unity AssetBundles) and **.dat data files**:

- `UnturnedDat` assembly parses the `.dat` format
- Asset types are registered in `UnturnedNexus.initialize()` and loaded via `Assets`/`AssetsWorker`
- Vanilla content is loaded from the Steam install, not from this repo

## Core Assets

### Resources (`Assets/Resources/`)

| Folder | Purpose |
|--------|---------|
| `Build/` | Build-related resources |
| `Characters/` | Character prefabs/materials |
| `Edit/` | Editor resources |
| `Fonts/` | Game fonts |
| `Level/` | Level-related resources |
| `Materials/` | Shared materials (including `AlwaysInclude/`) |
| `Physics/` | Physics materials |
| `Shapes/` | Debug/editor shapes |
| `Sounds/` | Audio clips |
| `UI/` | UI textures/sprites |

### Game Content Sources (`Assets/Game/Sources/`)

Additional game content sources for the editor.

## Building & Running

### Editor

1. Open `Assets/GameStartup.unity` in Unity
2. Press **Play**
3. Steam must be running with Unturned installed

### CLI / Batchmode Build

Build scripts are in `Assets/Editor/Assembly-CSharp-Editor/Tools/BuildMethods.cs`:

```bash
Unity -batchmode -quit -projectPath . \
       -executeMethod BuildMethods.PerformBuild \
       -buildTarget Win64 -logFile build.log
```

Supported `EUnturnedBuildTarget` values:
- `Windows32`, `Windows64`, `Windows64DedicatedServer`
- `Mac64`
- `Linux64`, `Linux64DedicatedServer`
- `Test`

Build output goes to `Builds/` with per-platform subdirectories.

### Dedicated Server

Headless dedicated server builds are supported for Windows 64-bit and Linux 64-bit.

## Testing

- **Unity Test Framework** (`com.unity.test-framework` v1.1.33) is included
- Test assemblies:
  - `Assets/Tests/UnturnedDat/` — `.dat` parser tests
  - `Assets/Tests/SDG.NetPak/` — Network packet tests
- Test scenes in `Assets/Tests/Scenes/` (Glazier UI tests, Sky test)
- Editor tests in `Assets/Editor/Assembly-CSharp-Editor/Tests/`

## Development Conventions

### Code Style (`.editorconfig`)

- **Indentation:** Tabs (not spaces)
- **Line endings:** CRLF (Windows)
- **File encoding:** UTF-8
- **Guideline column:** 120 characters
- **Namespace style:** File-scoped (`namespace Foo;` — IDE0160)
- **`var` usage:** Avoid — use explicit types (`int x = 5` not `var x = 5`)
- **Access modifiers:** Always explicit (`private`, `public`, etc.)
- **Braces:** Allman style (opening brace on new line for multiline blocks)
- **Auto-properties:** Do not convert to auto-props (`dotnet_style_prefer_auto_properties = false`)
- **Switch:** Prefer switch statements over switch expressions
- **Null checks:** Use `is null` pattern, `?.Invoke()` for delegates
- **Using directives:** Outside namespace, ungrouped

### Naming Conventions

- **Classes/Methods:** `PascalCase` (e.g., `PlayerLife`, `ZombieManager`)
- **Private fields:** `camelCase` (e.g., `recentIshPosition`)
- **Static fields:** `_camelCase` with underscore prefix (e.g., `_instance`)
- **Enums:** `E` prefix (e.g., `EItemType`, `EPlayerStance`, `EZombieSpeciality`)
- **Asset type strings:** `PascalCase` or `Snake_Case` (e.g., `"Gun"`, `"Vehicle_Repair_Tool"`)
- **Namespaces:** `SDG.Unturned`, `SDG.Framework`, `SDG.NetPak`, etc.

### Folder Organization

```
Assets/
├── Runtime/                    # Game runtime code
│   ├── Assembly-CSharp/        # Main game assembly (no asmdef)
│   ├── SDG.*/                  # Separate assemblies (NetPak, NetTransport, Glazier, etc.)
│   ├── UnityEx/               # Unity extensions assembly
│   └── SystemEx/              # .NET extensions assembly
├── Editor/                     # Editor-only code
│   ├── Assembly-CSharp-Editor/# Editor tools, build scripts, importers
│   ├── UnityEditorEx/        # Editor extensions
│   ├── Jenkins/              # CI integration
│   └── SteamCmd/             # SteamCMD automation
├── Resources/                  # Runtime-loaded assets
├── Tests/                     # Test scenes and test assemblies
├── UI Toolkit/               # UI Toolkit assets (USS/UXML)
├── Plugins/                   # Native plugins
├── Game/                      # Game content sources
└── com.rlabrecque.steamworks.net/  # Steamworks.NET library
```

### File Headers

All C# source files include the standard header:
```csharp
////////////////////////////////////////////////////////////////////////////////////////
// This file is part of the U3 SDK: https://github.com/smartlydressedgames/u3-sdk/    //
// Please refer to the included LICENSE.txt for copyright notice and license details. //
////////////////////////////////////////////////////////////////////////////////////////
```

## Package & Dependency List

From `Packages/manifest.json`:

| Package | Version | Purpose |
|---------|---------|---------|
| `com.unity.burst` | 1.8.21 | Burst compiler (performance) |
| `com.unity.collections` | 2.5.7 | Native collections |
| `com.unity.postprocessing` | 3.4.0 | Post-processing stack v2 |
| `com.unity.textmeshpro` | 3.0.7 | Text rendering |
| `com.unity.ugui` | 1.0.0 | Unity UI (uGUI) |
| `com.unity.2d.sprite` | 1.0.0 | 2D sprite support |
| `com.unity.editorcoroutines` | 1.0.0 | Editor coroutines |
| `com.unity.ide.visualstudio` | 2.0.22 | VS integration |
| `com.unity.memoryprofiler` | 1.1.11 | Memory profiling |
| `com.unity.test-framework` | 1.1.33 | Unity Test Framework |
| `com.unity.toolchain.win-x86_64-linux-x86_64` | 2.0.6 | Linux cross-compilation toolchain |
| `com.unity.modules.*` | 1.0.0 | Standard Unity modules (animation, physics, audio, terrain, vehicles, video, cloth, particles, UI, etc.) |

**Third-party (in Assets):**
- **Steamworks.NET** (`com.rlabrecque.steamworks.net`) — Steam API C# wrapper
- **UnityEx** — Community Unity extensions
- **SystemEx** — Community .NET extensions

## Version-Control Tips

### Should be committed (tracked):
- `Assets/` (all source code, scenes, resources, `.meta` files)
- `ProjectSettings/` (all project configuration)
- `Packages/manifest.json`
- `README.md`, `LICENSE.txt`, `THIRDPARTYNOTICES.txt`
- `.editorconfig`, `.gitattributes`, `.vsconfig`
- `steam_appid.txt`
- `Builds/Shared/Maps/` (vanilla map data only — see `.gitignore` exceptions)

### Should be ignored (in `.gitignore`):
- `Library/`, `Logs/`, `Temp/`, `obj/`, `.vs`
- `UserSettings/` (per-user editor preferences)
- `*.csproj`, `*.sln`, `*.userprefs` (IDE-generated)
- `Builds/*/Unturned_Data/`, `Builds/*/MonoBleedingEdge/` (platform binaries)
- `Builds/Shared/Sandbox/`, `Builds/Shared/Maps/*` (except vanilla maps)
- `Builds/Shared/Cloud`, `Builds/Shared/Logs`, `Builds/Shared/Servers`, `Builds/Shared/Worlds` (user save data)
- `Assets/TextMesh Pro/` (auto-imported)
- `*.exe`, `*.pdb`, `*.x86`, `*.x86_64`, `*.app` (built executables)

## CI/CD

- **Jenkins** integration via `JenkinsBootstrapper/` (C# bootstrap project) and `Assets/Editor/Jenkins/`
- Build methods in `BuildMethods.cs` support command-line switches for Steam upload, beta branches, etc.
- `Editor_Test_Config/` contains test configuration files (expected maps, server test maps, news test)

## Steam Integration

- **Steam App ID:** 304930 (stored in `steam_appid.txt`)
- Uses **Steamworks.NET** for Steam API calls
- Steam must be running during development for the game to function
- Large binary assets (models, textures, audio) are loaded from the Steam install, not from this repo
- `SteamworksProvider/` implements the `IProvider` interface for Steam-specific services

## TODO / Open Questions

- **No formal test runner script** — tests must be run through Unity's Test Runner window or CLI batchmode
- **BattlEye Anti-Cheat** — references in `.gitignore` (`BEService.exe`, `Unturned_BE.exe`) suggest BE integration, but source may be external
- **Razer Chroma** — runtime-generated `.chroma` animation files are ignored; integration may be compiled separately
- **Content pipeline** — the full asset bundle build workflow is documented in `BuildMethods.cs` and `MasterBundleTool.cs` but requires Steam-installed vanilla content to test
