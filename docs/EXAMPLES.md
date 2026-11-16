# Workflow examples

## Start from the current scene

1. Establish whether the application is already open at the relevant point.
2. Select and verify the correct window.
3. Observe a stable button, an object or visible text.
4. Branch on the actual result.
5. Apply the appropriate action.
6. Verify the changed state rather than assuming a sent click completed the task.

## Follow an object

Create a local object example, annotate it tightly, inspect the Alpha mask and try instant tracking. Test motion and disappearance. Select the resource in Find image, Continue when found and check `Target_Found` before acting. Never reuse a stale coordinate after a missing target.

## Read a value and decide

Read `Price_Text`, inspect its format and convert with `NUMBER(TRIM(Price_Text))`. Handle failures deliberately with `IFERROR` if appropriate. Validate the accepted range in a condition before sending data or performing an action.

## Reach a coordinate

Verify local coordinate OCR and establish axis signs. From an illustrative 815,869 position, 815,880 is north only if Y increases northward. Test a nearby target first, configure actual turning keys, then add waypoints and appropriate limits. Obstacles require route design; longer timeouts do not fix a wall or a wrong heading.

## Structure a large flow

Use Preparation, Main task, Shared check, Recovery and Result verification groups. Share common work with Call group/Return. Use bounded loops and explicit failure paths rather than duplicating long sequences.

All examples must be adapted and tested in the target environment. They are not guarantees of compatibility with a particular game or application.
