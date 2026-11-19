# JAQ Macro

**Visual workflow automation, object tracking, coordinate navigation and MCP-assisted authoring for Windows.**

JAQ Macro brings **15 visual workflow modules**, **32 built-in formula functions**, multiple screen-reading methods and an AI-assisted editor into one application. A workflow can identify the right window, wait for a visual state, act on what it finds and verify the result before proceeding. It can also follow a route using live coordinates or ask a compatible AI assistant to build and revise its decision logic.

**Current download · 2.0.27 for Windows x64** · [Download the Windows ZIP](https://github.com/fatihyilmas/JAQ-Macro/releases/download/v2.0.27/JAQ-MACRO-2.0.27-win-x64.zip) · [Release notes and checksums](https://github.com/fatihyilmas/JAQ-Macro/releases/tag/v2.0.27)

![English JAQ Macro workflow editor with an example window check, wait and conditional branch](docs/images/workflow-studio.webp)

[Module reference](#all-15-workflow-modules) · [Visual detection and tracking](#visual-detection-and-tracking) · [Coordinate navigation](#coordinate-navigation) · [Windows and input](#windows-and-input) · [Conditions and formulas](#conditions-variables-and-formulas) · [MCP and AI](#mcp-and-ai-assisted-authoring) · [Get started](#get-started)

## What makes JAQ Macro different

| Automation need | Fixed input replay | JAQ Macro |
| --- | --- | --- |
| A button moves or appears late | Replays the recorded position and delay | Searches the current frame and acts when the target is visible. |
| The same dialog changes position, color or content | Repeats the same action sequence | Can look for an image, a trained object, a pixel color, a color coverage range or recognized text. |
| The current screen determines what comes next | Replays the recorded path | Uses true/false branches, bounded loops, result variables and reusable groups. |
| Movement must reach an in-app destination | Replays timed input | Reads coordinates and adjusts movement, checks arrival and can resume verified waypoint progress. |
| You want an AI assistant to edit the workflow | The recording is a sequence of events | MCP gives compatible assistants a module contract and scoped tools to inspect and patch an actual project. |

For example, a desktop workflow can identify an open launcher, check for an **Update** button, wait for a visual confirmation, then click **Start** and verify a new window. The same project can keep the readiness check in a reusable group and route a missing or unexpected screen to a recovery group.

This comparison covers input replay as a method. Compare other products feature by feature with the same workflow and target environment.

## All 15 workflow modules

| Module | Operations |
| --- | --- |
| **Window** | Identify, activate, check or wait for a window. Minimize, maximize, restore and change always-on-top state. |
| **Find image** | Search for templates, trained objects, pixel colors or color coverage. Branch or perform a mouse action from the result. |
| **Program and files** | Check for a local file, read text, create a folder or new text file, and launch an executable with optional arguments, working directory and exit-code capture. |
| **Loop / While** | Repeat nested steps while a condition holds, with an explicit iteration limit. |
| **Variable** | Store text, a number, a Boolean or a calculated result for later steps. |
| **Condition / If** | Run true or false branches based on a formula, a linked step outcome, or a window, process or file check. |
| **Stop** | Finish a workflow deliberately from the normal path or a recovery branch. |
| **Mouse** | Click, double-click, move, move and click, scroll, drag and drop, or press and release a button explicitly. |
| **Coordinates** | Navigate by ground clicks or keyboard using OCR position feedback, calibrate movement, or record and replay a route. |
| **Key** | Queue key presses, combinations, held keys, explicit down/up events, literal text and variable text. |
| **Read text** | Read text or locate a phrase, save results to variables and optionally enter recognized text at a configured location. |
| **Go to group** | Transfer control to another group. |
| **Call group** | Run a reusable group and continue from the calling step. |
| **Return** | End the call and resume the caller. |
| **Wait** | Pause for a fixed or calculated duration in milliseconds. |

## Visual detection and tracking

### Four search modes

| Mode | What it measures | Example |
| --- | --- | --- |
| **Image template** | Compares a gallery image with pixels in the current capture. An optional color-isolated shape mode can reduce the influence of the surrounding background. | Find a toolbar icon inside the target application. |
| **Trained object** | Uses an object library entry and its configured tracking engine. Candidate selection can require a unique result, choose the highest confidence or prefer the object nearest the search center. | Locate the same target after it moves to another part of the screen. |
| **Pixel color** | Looks for a chosen RGB color within a selected area, with configurable tolerance and minimum matching pixel count. | Find a status indicator whose color changes. |
| **Color coverage** | Measures how much of a selected area matches a color and checks it against a minimum and maximum percentage. | Branch when the colored portion of a progress bar reaches a threshold. |

- Scope the search to a window or the screen, and narrow it to a region where needed. Configure confidence, retry interval, attempt count and timeout for a bounded search. **Wait until visible** keeps observing until a match appears or the run is stopped or interrupted.
- Separate **found** and **missing** behavior. On a match, continue, move to it, click, right-click, middle-click, stop or go to another group. A missed search can continue, stop or transfer control according to the configured outcome.
- A matching step can supply a location to a subsequent Mouse step. Live actions check the target window and capture geometry before delivering input.
- The search output prefix provides values such as `Target_Found`, `Target_X`, `Target_Y` and `Target_Score`. Color modes also expose `Target_Percent`. An independently named step outcome, such as `Scan`, records success and exposes `Scan_Applied` for an action that was actually sent.

### Object library and live tracking

- **Instant exemplar tracking.** Annotate target examples and use VOT Alpha to test or track supported targets directly from reference samples.
- **YOLO11 ONNX.** Import a compatible detector or prepare training data in the object library. Inference can use DirectML on supported GPUs, with CPU fallback when appropriate.
- **Auto Hybrid.** Can combine model detections with local exemplar tracking and stabilize the displayed bounding box, depending on the model and available samples.
- **Training tools.** Collect positive examples and backgrounds or negatives, edit boxes, separate the foreground with GrabCut and alpha masks, and generate Copy-Paste training examples with rotation and blending options. Appearance filtering is available for distinguishing a specific target.
- **Visual testing.** Inspect a result and its bounds, test changing backgrounds or angles, and view a tracking overlay in a controlled scene. Recognition quality depends on target visibility, examples and the detector.

![Actual detection in JAQ's original demo scene with a box around the target](docs/images/object-tracking.webp)

### OCR and text-driven actions

**Read text** supports Windows OCR, PaddleOCR and optional provider-backed AI OCR through Gemini or OpenRouter. Select a region, choose a language and configure image preprocessing such as scaling, inversion or color filtering, where supported. Use full-text mode to read a label or a number, or phrase location to get the coordinates of a match.

Results include `Readout_Text`, `Readout_Found` and `Readout_Ms`, plus `Readout_X` and `Readout_Y` when a phrase is located. Use those values in a condition, normalize text with formulas or pass it to the Key module. Optional OCR transfer can enter recognized text at a chosen location using a configured sequence.

## Coordinate navigation

The **Coordinates** module reads an application’s position display with OCR and uses that feedback to guide movement. It works with a combined coordinate display or separate X and Y regions. Set the coordinate-axis directions, read the current position into the editor, set a destination and choose the arrival tolerance.

| Movement method | How it works | Configuration highlights |
| --- | --- | --- |
| **Ground clicks** | Calibrates the relationship between clicks around an anchor and changes in world coordinates. Uses subsequent readings to check progress. | Anchor, mouse button and hold time, permitted click area, excluded UI regions, probe radius, maximum click radius, settling delay and recalibration limits. |
| **Keyboard with coordinate change** | Measures how position changes during a forward movement and estimates heading again as the route progresses. Corrects sustained drift and reduces motion near the goal. | Forward, back and side keys, turn versus strafe behavior, calibration, key pulse lengths, movement limits and reading intervals. |
| **Keyboard with direction arrow + coordinates** | Reads the heading marker and OCR location together. Steers by comparing the detected heading to the bearing toward the next point. | Arrow capture region, learned shape or imported arrow image, color, reference heading, angle offset, axis directions and movement keys. |
| **Route recording and playback** | Records keyboard and mouse events with their timing, then replays the route in the foreground. | Countdown, scan codes, mouse-move sampling, playback speed, optional timing and position variation, checkpoints and optional coordinate verification. |

- **Waypoints and resume.** Add intermediate points to guide turns on a long route. On restart the module can skip already verified waypoints when saved progress still matches the target window, route and current position. Editing the route, relocating the target or completing the destination can invalidate the checkpoint.
- **Action keys.** Optionally press a key before moving, set its hold time and add a wait before motion begins. An optional arrival key runs only after verified arrival in guided navigation.
- **Reading and movement limits.** Configure stable-read count, reading age, maximum coordinate jump, movement timeout, maximum destination distance and movement budget. Review the OCR and heading preview before running a long path.
- **Results for branching.** A navigation prefix can expose values such as `Route_X`, `Route_Y`, `Route_Distance`, `Route_Reached`, `Route_Moves`, `Route_Status` and `Route_Reason`. Recorded routes can also report the current checkpoint.

Use waypoints to handle corners or obstacles in the target environment. Coordinate navigation follows configured feedback and route points.

[![Coordinate navigation inspector in English](docs/images/coordinate-navigation.webp)](docs/images/coordinate-navigation.webp)

## Windows and input

### Window module

- Choose a window from the open-window list or match by **process name, window title, window class, handle or process ID**. The project can have a default target and individual steps can override it.
- Check whether the target exists or is active. Wait for it to appear, become active or close. Use a timeout or a linked condition to handle an unexpected state.
- Bring the target forward, minimize it, maximize it, restore it, or turn always-on-top on or off.
- A Window or Find image step can serve as a **Background Supervisor**. Configure the polling interval, priority, required confirmations and cooldown. A matching supervisor can redirect the workflow to a recovery group while the flow is running.

### Mouse and keyboard

- **Mouse positions.** Pick a client-area point, calculate it with X/Y expressions or use a recent visual target. Window-relative coordinates use the target’s client area. Mouse actions can also use relative motion where supported.
- **Mouse controls.** Select left, right, middle or side buttons, click counts, hold duration, repeats, intervals, wheel notches and drag endpoints. Use separate button-down and button-up events when an interaction spans multiple steps.
- **Keyboard queues.** Mix key combinations, held keys, literal text and variable content in a single step. Configure the duration and delay for individual items, and common timing between items.
- **Delivery.** Foreground input is available for active-window interaction. Compatible windows can receive supported mouse and keyboard actions through background window messages. Foreground-dependent actions such as coordinate-keyboard navigation, route recording/playback and held key sequences require foreground mode.

Capture and background input behavior depend on the target application and an interactive Windows session.

## Conditions, variables and formulas

**If** supports nested **Then** and **Else** branches. Evaluate an expression, link directly to a previous step’s success, negative result or applied action, or probe whether a window is open/closed or active/inactive, a process is running/stopped, or a file exists/is missing. A step can also have its own run condition. Use **While** with an iteration limit, and split repeated logic into groups and subgroups using **Call group**, **Return** or **Go to group**.

Variables store numbers, text and Booleans. Use arithmetic, comparisons, Boolean expressions and built-in functions with observed values. Named step outcomes distinguish an observation, such as finding an image, from sending an action, such as clicking it.

| Function family | Complete list |
| --- | --- |
| Logic and recovery | `IF`, `IFERROR`, `AND`, `OR`, `NOT`, `DEFINED` |
| Math | `SUM`, `AVERAGE`, `MIN`, `MAX`, `ABS`, `ROUND`, `FLOOR`, `CEILING`, `MOD`, `CLAMP` |
| Text | `LEN`, `TRIM`, `LOWER`, `UPPER`, `CONTAINS`, `STARTSWITH`, `ENDSWITH`, `CONCAT`, `LEFT`, `RIGHT`, `REPLACE` |
| Conversion and types | `NUMBER`, `TEXT`, `ISNUMBER`, `ISTEXT`, `ISBOOL` |

For example, `IFERROR(NUMBER(TRIM(Price_Text)); -1)` converts an OCR value to a number with a fallback. A condition such as `AND(Price_Found; PriceValue >= 0; PriceValue <= 100)` can then gate a following action. English names and supported Turkish aliases are available in the formula editor.

## Studio, execution and accounts

- **Gallery and search.** Import images in batches, view them in a four-column grid, open a full preview and remove unused images individually or together. Pick existing images from a module. The shared search box filters images in Gallery and finds workflow steps by module, name or indexed setting in Flows.
- **Workflow organization.** Manage groups and subgroups, reuse helper groups and assign group shortcuts. Test a step, run a group or start the full workflow.
- **Run monitoring.** Inspect dry-run and execution logs. A draggable panel displays status, start/pause/stop controls and shortcuts during full-flow and group runs. The studio can stay in the notification area and optionally open there at Windows sign-in.
- **Account and transfer.** The account area includes licensing, MCP access, support and available transfer controls. File import/export is subject to the applicable license entitlement.
- **Updates.** Check for a new Windows release in the application. The updater verifies the publisher-signed descriptor and package contents. Applying the update requires separate confirmation.

| Image Gallery | Workflow search |
| --- | --- |
| [![Gallery showing four reusable visual assets](docs/images/image-gallery.webp)](docs/images/image-gallery.webp) | [![Workflow search results in the English interface](docs/images/workflow-search.webp)](docs/images/workflow-search.webp) |

## MCP and AI-assisted authoring

Connect a compatible client such as **Codex, Claude Code, Kiro or Antigravity** through remote MCP over Streamable HTTP. Copy the connection quick-start from JAQ Macro. Client setup and service access depend on the client and account.

1. Start with `get_authoring_context` to discover the module catalog, current device/project context and active design checkpoints.
2. Read relevant module fields and defaults through `get_program_contract`, then inspect the relevant part of the workflow.
3. Ask the user to capture any missing visual references in JAQ Macro. Reuse the images, steps and groups already present in the project.
4. Draft conditions, formulas and recovery behavior. Apply focused changes and check delivery status. A design checkpoint can preserve the next action while a local capture is pending.
5. Review and start the workflow in the Windows application. MCP manages workflow metadata and design. Visual asset bytes stay local, and running the flow requires a local start.

Discovery, module documentation and status checks can be used without a project-read charge. Project inspection and edits use JAQ credits under the account’s usage rules. AI provider usage is handled separately.

> My launcher is already open. Check its window, detect whether Update is present, and only click Start after the ready screen is verified. Reuse my existing images. If a reference is missing, tell me which area to capture in JAQ Macro. Add a bounded retry and show me how to test the group.

## Get started

1. Download the Windows x64 ZIP and extract all files to a writable folder.
2. Keep the `lang` folder next to `JAQ-MACRO.exe`, then open the executable and approve the Windows administrator request. English and Turkish are included.
3. Select a target window, add an observation and action, and test the result before running the full flow.

JAQ Macro is proprietary software. This repository provides public documentation and Windows downloads. The downloadable release assets and the GitHub-generated repository archives serve different purposes.

[Feature guide](docs/FEATURES.md) · [Workflow examples](docs/EXAMPLES.md) · [Downloads and updates](docs/UPDATES.md) · [License](LICENSE.md) · [Security](SECURITY.md)
