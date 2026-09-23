# Masked Mischief

A single-player Unity 3D stealth-and-exploration game in which the player navigates domestic and campground environments, interacts with objects, manages movement and traps, and evades or manipulates hostile family members to complete objectives and win the round.

## 🌟 System Architecture & Core Stack

This project is a Unity-based gameplay prototype built around scene-driven runtime composition, C# controller logic, AI state machines, and serialized game assets. The architecture is intentionally modular but remains assembled in a single-project runtime rather than a multi-service backend.

| Layer | Primary Files / Paths | Responsibility |
| --- | --- | --- |
| Engine & Platform | `ProjectSettings/ProjectVersion.txt`, `Packages/manifest.json` | Unity 2022.3.44f1 runtime, package registry, core editor dependencies |
| Input | `Assets/InputManager/PlayerInputActions.inputactions`, `Assets/InputManager/PlayerInputActions.cs`, `Assets/Scripts/GameInput.cs` | Keyboard/gamepad movement, sprint, jump, interact, hotbar, use-item bindings |
| Player Runtime | `Assets/Scripts/Player.cs`, `Assets/Scripts/CharacterScripts/PlayerScripts/PlayerRunner.cs` | Player locomotion, camera follow, jump physics, trap placement, animation coordination |
| Movement FSM | `Assets/Scripts/CharacterScripts/PlayerScripts/StateMachines/Movement/` | Player movement states: idle, walk, run, jump, land, fall, stop, trap placement |
| AI & Sensing | `Assets/Scripts/FamilyMembers/`, `Assets/Scripts/FamilyMembers/FamilyStateMachine/`, `Assets/Scripts/FamilyMembers/FieldOfView.cs` | Patrol logic, player detection, search behavior, freeze/activate states |
| World Interaction | `Assets/Scripts/Item Interaction Scripts/`, `Assets/Scripts/Item Interaction Scripts/PlayerInteract.cs` | Doors, furniture, cabinets, level transitions, interaction-specific flow |
| Tools & Combat | `Assets/Scripts/Traps/`, `Assets/Scripts/ThrowObject.cs`, `Assets/Scripts/ThrowableTool.cs`, `Assets/Scripts/KatanaDamage.cs` | Banana traps, smoke bombs, throwable tools, damage events |
| UI & Scene Flow | `Assets/Scripts/UI Scripts/`, `Assets/Scenes/MainMenu.unity`, `Assets/Scenes/Campground.unity`, `Assets/Scenes/House.unity` | Menus, pause flow, fade transitions, win/lose UX, tutorial prompts |
| Audio | `Assets/Scripts/Music/`, `Assets/Sounds/` | Ambient and scene music state management |
| Art / Environment | `Assets/Art Assets/`, `Assets/Campground.asset`, `Assets/Scenes/*` | Characters, environment props, terrain, prefabs, scene data |

### Runtime execution flow

1. The Unity editor boots a scene such as `MainMenu` or `Campground`.
2. `GameInput` listens to Unity Input System actions generated from `PlayerInputActions.inputactions`.
3. `Player` and `PlayerRunner` translate input into movement, camera motion, and state changes.
4. The player can trigger interactions through `PlayerInteract` and object-specific interactable scripts.
5. `FamilyMember` and associated state machines perform patrol, search, and alert behavior using NavMesh and field-of-view sensing.
6. UI scripts manage transitions between menu, gameplay, and Win/Lose outcomes.
7. Music and sound scripts hook into gameplay states to provide contextual audio.

### Core technologies and conventions

- Unity Engine: `2022.3.44f1`
- Primary language: `C#`
- Input system: `UnityEngine.InputSystem`
- AI navigation: `UnityEngine.AI`
- Animation: Unity Animator + .anim controllers
- Scene and asset serialization: Unity scene files and prefabs
- Package support: Cinemachine, Shader Graph, TextMeshPro, AI Navigation

This repository does not include a backend service, database layer, or authentication system. Runtime state is scene-local and object-driven, which is appropriate for a single-player game prototype.

## 🛠️ Module Breakdown & Feature Matrix

| Module | Exact Path(s) | Structural Responsibility | Status |
| --- | --- | --- | --- |
| Root Project Setup | `ProjectSettings/`, `Packages/manifest.json`, `.vsconfig`, `.gitignore` | Unity project configuration, editor settings, dependency manifest, ignore rules | Built |
| Main Menu & Scene Entry | `Assets/Scenes/MainMenu.unity`, `Assets/Scripts/UI Scripts/MainMenuBehavior.cs` | Bootstraps the game flow and transitions to the active gameplay scene | Built |
| Player Controller | `Assets/Scripts/Player.cs`, `Assets/Scripts/GameInput.cs`, `Assets/Scripts/CharacterScripts/PlayerScripts/PlayerRunner.cs` | Read player input, drive movement, jump, camera, trap setup, animation callbacks | Built, prototype-level logic |
| Player State Machine | `Assets/Scripts/CharacterScripts/PlayerScripts/StateMachines/Movement/` | Encapsulates movement behavior as discrete states: idle, walk, run, jump, landing, stopping, trap placement | Built |
| AI Family Logic | `Assets/Scripts/FamilyMembers/`, `Assets/Scripts/FamilyMembers/FamilyStateMachine/` | Patrol, search, freeze, and activated states for NPCs; player pursuit and detection | Built |
| Field-of-View Detection | `Assets/Scripts/FamilyMembers/FieldOfView.cs` | Detects the player and blocks line-of-sight using masking and obstruction logic | Built |
| Interaction System | `Assets/Scripts/Item Interaction Scripts/`, `Assets/Scripts/Item Interaction Scripts/PlayerInteract.cs` | Generic object interactivity, door logic, chair/cabinet interactions, win/lose triggers | Built |
| Tools & Traps | `Assets/Scripts/Traps/`, `Assets/Scripts/ThrowObject.cs`, `Assets/Scripts/ThrowableTool.cs` | Banana trap and smoke bomb deployment patterns; throwable objects | Built |
| UI Management | `Assets/Scripts/UI Scripts/`, `Assets/Scripts/UI Scripts/FadeManager.cs`, `Assets/Scripts/UI Scripts/PauseScript.cs` | Pause overlay, transitions, tutorial prompts, feedback | Built |
| Win/Lose Flow | `Assets/Scripts/WinLoseControl.cs`, `Assets/Scripts/Item Interaction Scripts/WinInteractable.cs` | End-of-round evaluation and outcome triggers | Built |
| Audio / Music System | `Assets/Scripts/Music/`, `Assets/Sounds/` | Houses and tutorial soundtrack behavior; audio contextualization for scenes | Built |
| World Assets & Scenes | `Assets/Art Assets/`, `Assets/Scenes/Campground.unity`, `Assets/Scenes/House.unity`, `Assets/Campground.asset` | Game environment and scene composition | Built |
| Mock / Unused / Experimental Scripts | `Assets/Scripts/Unused Scripts/` | Legacy or exploration-era logic retained in the repo | Mixed / likely experimental |

### Module observations

- The project is structured as a gameplay sandbox rather than a layered service architecture.
- Many systems are driven by Unity GameObjects and scripts connected at runtime in scene setup.
- Some modules show transitional or prototype-level implementation details: debug logging, commented legacy blocks, and mixed conventions around movement/animation logic.
- The project is clearly oriented toward a stealth, exploration, and family-simulation gameplay loop, with scene progression as a central design pattern.

### Key files to understand the production flow

- `Assets/Scripts/Player.cs`
- `Assets/Scripts/GameInput.cs`
- `Assets/InputManager/PlayerInputActions.inputactions`
- `Assets/Scripts/CharacterScripts/PlayerScripts/StateMachines/Movement/PlayerMovementStateMachine.cs`
- `Assets/Scripts/FamilyMembers/FamilyMember.cs`
- `Assets/Scripts/FamilyMembers/FamilyStateMachine/FamilyStateMachine.cs`
- `Assets/Scripts/Item Interaction Scripts/PlayerInteract.cs`
- `Assets/Scripts/Traps/BananaTrap.cs`
- `Assets/Scripts/UI Scripts/MainMenuBehavior.cs`
- `Assets/Scenes/Campground.unity`

## 🔒 Security & Data Management

This repository is a local game project and does not expose a user authentication system, API layer, credential store, or persistent production database.

### Current data boundaries

- Runtime data is largely scene-local and serialized by Unity.
- Gameplay state is managed in memory via C# objects, GameObject references, and Unity scene serialization.
- Assets and scripts live in the repository and are treated as source-controlled game content.
- No external secrets, environment files, or runtime credentials are present in this codebase.

### Governance recommendations

- Keep all game configuration in Unity scene and prefab assets rather than hardcoded credentials or external secret stores.
- Treat generated Unity project metadata (`.meta`, project cache, library files) as build artifacts rather than source of truth.
- Avoid committing user-specific editor settings or local machine state.
- If the project later expands into a networked or online title, isolate runtime services behind a dedicated backend layer and enforce strict validation before game state mutates.

### Assets and project safety

- `.gitignore` is present for Unity project hygiene.
- Unity-generated metadata and editor caches are excluded or should remain managed appropriately.
- There is no evidence of database access control, authorization model, or user data storage.

## 🚀 Quickstart & Local Execution

### Prerequisites

- Unity Hub
- Unity Editor `2022.3.44f1`
- A local clone of this repository
- A supported desktop OS for Unity development

### Local setup

1. Install Unity Hub and add Unity Editor `2022.3.44f1`.
2. Clone the repository to a local working directory.
3. Open Unity Hub.
4. Click `Open` and select the repository root folder.
5. Let Unity import the project and resolve dependencies from `Packages/manifest.json`.
6. When prompted, ensure the project loads with the correct editor version.

### Recommended scene entry points

- `Assets/Scenes/MainMenu.unity` — main entry scene
- `Assets/Scenes/Campground.unity` — primary gameplay sandbox
- `Assets/Scenes/House.unity` — alternate environment / progression scene

### Run the game

1. In Unity, open the desired scene from `Assets/Scenes/`.
2. Use the Unity Play button in the editor.
3. Or, if the project is configured in Unity Hub, open the root project and start from the menu scene.

### Controls

The input mappings are defined in `Assets/InputManager/PlayerInputActions.inputactions` and are surfaced through `Assets/Scripts/GameInput.cs`.

| Action | Keyboard | Gamepad |
| --- | --- | --- |
| Move | `W`, `A`, `S`, `D` | Left stick |
| Jump | `Space` | `A` / South |
| Sprint | `Left Shift` / `Right Shift` | Left/Right trigger |
| Interact | `F` | `B` / East |
| Place Trap | `T` | not explicitly mapped in runtime hook |
| Toggle Hotbar Left | `Q` | Left shoulder |
| Toggle Hotbar Right | `E` | Right shoulder |
| Pause / Controls | `Tab` | Start |

### Dependency notes

- No external API keys, database credentials, or .env files are required for local gameplay.
- The project relies on Unity package resolution and in-editor object configuration.
- Asset content is scene-data driven, so reimporting or changing scenes may affect runtime object references.

### Operational summary

This repository is best understood as a Unity 3D gameplay prototype focused on experimentation, stealth mechanics, scene exploration, NPC behavior, and interaction-driven progression. It is not a web application and does not currently implement a backend, service layer, or persistent data model. Its architecture is optimized for rapid gameplay iteration inside the Unity editor rather than for production-scale deployment.

---

The project is a compact, asset-heavy Unity game codebase with a clear gameplay loop, a layered runtime scripting structure, and a sizeable environment/character content pipeline. It is well-suited for iterative gameplay development in a single-player, scene-driven architecture.
