# Feature guide

## Visual modules

| Module | Purpose |
|---|---|
| Window | Identify, activate, wait for or inspect target windows |
| Wait | Fixed or expression-driven millisecond delays |
| Variable | Typed numbers, text and booleans |
| Condition | True/false paths from checks, results or expressions |
| Loop | Repeat child actions with an explicit iteration limit |
| Go to group | Transfer execution without creating a return point |
| Call group | Reuse a helper workflow with a return point |
| Return | Resume the caller |
| Stop | End execution deliberately |
| Mouse | Click, move, drag, scroll and button-down/up actions |
| Keyboard | Keys, combinations, text and variable text in a queue |
| Find image | Templates, objects, pixel colors and color coverage |
| Program and files | Local .exe launch, file checks, reads and new outputs |
| Read text | Windows/Paddle local OCR and optional remote AI OCR |
| Coordinates | Feedback-driven movement and recorded route playback |

## Object tracking

Use instant exemplar tracking, a prepared YOLO11 ONNX model, or a suitable hybrid path. DirectML can use supported GPU hardware; CPU inference remains environment-dependent. Annotation, Alpha masks and GrabCut help separate the target from background. Copy-Paste and rotation options prepare synthetic examples, while appearance filters and selection rules help choose among detections.

An example does not create unseen object detail or guarantee recognition under every angle, scale or occlusion. Evaluate positive scenes, absent targets and similar distractors before attaching input actions.

## Coordinate navigation

Read world position with local OCR and establish axis signs and movement keys. Select ground clicks, keyboard guidance or route recording. Keyboard guidance can infer heading from coordinate changes or read a direction arrow. Configure tolerance, waypoints and practical movement limits. Optional arrival actions happen only after verified arrival.

## Gallery, search and run controls

Use Gallery to browse, import and preview project images. The Find image module can pick from the same library. The shared search box filters image names in Gallery and finds workflow steps by module or setting in Flows.

Use the draggable run panel to monitor a full flow or a group and access the configured stop shortcuts. The tray menu keeps those controls accessible when the studio is hidden.

## Workflow structure and formulas

Combine nested true/false branches, bounded loops, groups/subgroups, calls, returns, transfers and supported supervisors. Built-ins include IF, IFERROR, AND, OR, NOT, DEFINED, SUM, AVERAGE, MIN, MAX, ABS, ROUND, FLOOR, CEILING, MOD, CLAMP, LEN, TRIM, LOWER, UPPER, CONTAINS, STARTSWITH, ENDSWITH, CONCAT, LEFT, RIGHT, REPLACE, NUMBER, TEXT, ISNUMBER, ISTEXT and ISBOOL.

Expressions use bounded built-ins, not arbitrary Python/C# execution. Initialize variables and verify their types before evaluating them.

## MCP and resources

Compatible assistants can inspect/edit workflow metadata and use current module guidance and task checkpoints. Images remain local. Cloud save, queued delivery, local loading and local execution are distinct states. File import/export requires the relevant entitlement; a local training/model library has a different scope from linked PNG templates.
