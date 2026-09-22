---
icon: lucide/layers
---

# Modal Stack

Everything that pops over the current screen runs through one stack. Bottom to top, top only receives input.

## Stack Pushing

The caller constructs a modal and awaits `ShowAsync`. The service appends it, attaches itself, refreshes which entry counts as top, fires the change event, and returns a task completing on close. The generic overload then reads the result the modal set before closing.

The host rebuilds layered views on every change, so later entries overlay earlier ones. Only the top layer accepts pointer input. The main window mirrors the stack to drive backdrop visibility and to save the underlying screen for restore.

The screenshot viewer hides the standard backdrop because it paints its own opaque background.

## Modal Inputs

Routing sends every command to the top modal verbatim. The base closes on Back and ignores everything else. Concrete behaviour per modal:

- Confirmations move with left and right, confirm on Activate, cancel on back.
- Game details moves between actions, enters panes, and leaves them.
- The viewer steps with left and right, closes on back.
- The profile picker moves, switches versions sideways, and opens management on Details. See Profiles.

Modals underneath the top hide their hint bars, so stacked prompts never show competing hints.

## Modal Results

| Modal | Result | Meaning |
|---|---|---|
| Confirmation | true, false, null | Confirm, cancel, dismissed with back. Callers treat null as stay |
| Disc selector | disc number or null | Null aborts the launch with nothing written |
| Patch download | none - the caller refreshes unconditionally on return | Caller refreshes its cached entry and rebuilds rows |
| Settings exit | save, discard, stay | Save writes, discard reloads from disk, stay keeps the pane open |

The rule is always the same: set the result, then close. The generic channel guarantees the opener observes the value.

## Nested Modals

Because pushing is awaitable, any modal or pane can suspend its flow, push a child, and resume after the child pops and disposes:

- The screenshots pane opens the viewer over the game modal, then resumes with the modal untouched.
- The patches pane opens downloads, then refreshes its rows on return.
- Content, patch, settings, and profile flows confirm through the shared confirmation builder, then act or stay based on the answer.
- The profile picker opens full management on top and reloads its rows on return.

Each push and pop refreshes top flags and fires the change event, so hints, hit-testing, and backdrops restore symmetrically.

## Closing Rules

Modals are created fresh per open and disposed on close. No state carries between opens. Disposal overrides release pane disposables, self-scanned thumbnails, and viewer full-resolution images.

!!! warning "Pop order"
    Only the top modal can pop. Closing a buried modal logs a warning and changes nothing. Flows must always close top-first.
