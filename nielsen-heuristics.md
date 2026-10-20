# Nielsen Heuristics

## Purpose

This document is a platform-neutral operational usability standard for the
**Designer/Planner LLM** when planning, specifying, and reviewing user-facing
software.

It applies to web, desktop, mobile, native, and hybrid interfaces.

It is not a one-time audit checklist. Relevant heuristics should shape the
solution before implementation begins. A final heuristic review should catch
edge cases, not discover basic failures such as missing status feedback,
irreversible actions, unclear terminology, inaccessible interaction, or errors
with no recovery.

The Coder may read this document, but the Designer/Planner owns material product
and interaction decisions. Coder-facing plans should translate relevant
heuristics into concrete observable behavior, failure states, validation, and
acceptance criteria.

The heuristics are broad principles. They do not replace product judgment,
accessibility standards, platform conventions, user research, or evidence.

---

## Planner responsibilities

For every non-trivial user-facing task:

1. Identify which heuristics materially apply.
2. Use them while forming the solution, not after the design is already fixed.
3. Resolve material UX decisions before coder handoff.
4. Specify observable behavior rather than merely naming a heuristic.
5. Define meaningful idle, pending, success, empty/unavailable, and error states
   when asynchronous or long-running work exists.
6. Define focus, keyboard, pointer/touch, cancel/escape, navigation/history, and
   recovery behavior where relevant.
7. Prevent choices the product cannot honestly support.
8. Choose validation that can actually prove the observable requirements.
9. Review the implemented result against the relevant heuristics before
   acceptance.

Do not add a boilerplate ten-heuristic section to every plan.

Bad plan language:

> Follow Nielsen's heuristics.

Useful plan language:

> Recording begins with immediate visible status and live input activity. The
> floating status panel does not steal focus. When recording stops, the panel
> changes to a distinct processing state and always resolves to success or a
> recoverable error.

---

# The ten heuristics, operationalized

## 1. Visibility of system status

The system should keep users informed about what is happening, what state they
are in, and whether their action succeeded.

### Planning requirements

For asynchronous or long-running interactions, define the states that genuinely
apply:

```text
idle
pending / active
success
empty / unavailable
error
```

As relevant:

- acknowledge user actions promptly;
- show meaningful progress or activity during waits;
- distinguish active work from a frozen interface;
- preserve stable layout/context when possible;
- prevent stale content from masquerading as current content;
- keep selected/active/expanded/filtering state visible;
- use platform-appropriate busy/status semantics;
- announce meaningful completion or failure accessibly when useful;
- ensure every pending state has a clear exit on success, cancellation, or error.

A spinner is not automatically the right answer. Choose feedback that matches
what the user needs to understand.

---

## 2. Match between the system and the real world

Use the user's language, familiar concepts, and a natural order of information.

### Planning requirements

- Prefer domain/user language over implementation terminology.
- Use familiar units and formats.
- Put consequences and task-relevant meaning before internal methodology.
- Keep visible labels consistent with what the system actually does.
- Do not imply precision, capability, permanence, or success the system cannot
  support.
- Translate technical states into concise human language where appropriate.
- Keep internal identifiers, protocol names, model names, and framework jargon
  out of ordinary UI unless the user genuinely needs them.

---

## 3. User control and freedom

Users need clear exits, cancellation where practical, and recovery from
accidental actions.

### Planning requirements

Where relevant, define:

- cancel/stop behavior;
- Escape or equivalent platform dismissal;
- Back/Forward/history behavior;
- close behavior;
- reset/clear behavior;
- undo or reversal for consequential changes;
- what happens if the user changes context while work is pending;
- preservation of useful input/context after failure.

Do not trap users in a state that requires restarting the app/page when a normal
recovery path is possible.

---

## 4. Consistency and standards

Follow platform conventions and existing product patterns unless there is a
material reason not to.

### Planning requirements

- Use standard platform controls and expected interaction mechanics.
- Keep the same term for the same concept.
- Reuse established status, error, selection, typography, spacing, and control
  patterns where they are sound.
- Follow expected keyboard, pointer/touch, focus, menu, clipboard, navigation,
  and window behavior for the platform.
- Do not introduce a second interaction pattern for the same task without a
  concrete benefit.

Consistency does not require preserving a bad pattern. If a weak pattern must
change, bound the migration deliberately.

---

## 5. Error prevention

Prevent predictable mistakes rather than merely explaining them afterward.

### Planning requirements

- Offer only actions the system can support truthfully.
- Validate input before costly or irreversible work.
- Disable, omit, or constrain impossible actions where that is clearer than
  letting them fail later.
- Prevent accidental duplicate activation while an operation is already active.
- Make likely consequences clear before consequential actions.
- Confirm destructive actions when the cost of error warrants it.
- Distinguish absence/unavailability from a valid zero/empty value.
- Design gestures and controls to reduce accidental activation.

---

## 6. Recognition rather than recall

Keep necessary context visible or easy to retrieve so users do not have to
remember it.

### Planning requirements

- Keep current mode/state/selection visible when it matters.
- Use persistent labels when placeholders or icons alone would be ambiguous.
- Keep relevant units, scope, filters, and comparison basis near the data they
  explain.
- Preserve useful context across navigation or drilldown.
- Put definitions/help near unfamiliar concepts.
- Do not require users to remember a value or state from a previous screen when
  the current task depends on it.
- Tooltips may supplement meaning; critical information should not depend on
  hover alone.

---

## 7. Flexibility and efficiency of use

Support efficient repeat use without making the primary path harder.

### Planning requirements

Where useful:

- provide keyboard shortcuts or direct actions;
- preserve standard clipboard and system interactions;
- support search in genuinely large option sets;
- use sensible defaults;
- remember benign preferences only when that actually reduces repeated work;
- provide direct/shareable state where the platform and task benefit from it;
- use progressive disclosure for advanced controls.

Do not add shortcuts, modes, filters, preferences, or configuration merely to
make the product appear powerful.

---

## 8. Aesthetic and minimalist design

Every visible element should support comprehension, status, or action.

### Planning requirements

- Remove redundant copy and controls.
- Avoid decorative UI that competes with the user's task.
- Prefer hierarchy and spacing over extra containers and ornament.
- Keep technical detail out of primary task surfaces unless needed.
- Do not multiply dashboards, charts, settings, or modes without a user problem.
- Keep transient status UI appropriately lightweight.

Minimalism must not remove necessary status, context, accessibility,
recoverability, or uncertainty information.

---

## 9. Help users recognize, diagnose, and recover from errors

Errors should be understandable and provide a useful next step.

### Planning requirements

A meaningful error should answer, proportionately:

1. What happened?
2. What was affected?
3. What can the user do next?

As relevant:

- preserve the user's valid input/work;
- distinguish permission, invalid input, unavailable capability, empty result,
  temporary failure, and internal failure;
- provide retry, alternate path, reset, manual recovery, or clear next step;
- avoid raw stack traces, framework/model errors, opaque identifiers, and
  generic messages that provide no recovery.

Do not turn every failure into a modal dialog. Match the error treatment to the
scope and consequence of the problem.

---

## 10. Help and documentation

Make the interface understandable without requiring documentation, then provide
help where genuine complexity remains.

### Planning requirements

- Prefer self-explanatory controls and labels.
- Put concise explanation near unfamiliar concepts.
- Use progressive disclosure for deeper technical detail.
- Document important assumptions, privacy behavior, or limitations when users
  need them to make decisions.
- Keep help task-oriented and searchable when the product is large enough to
  need formal documentation.

Documentation must not compensate for a confusing or unsupported interaction.

---

# Asynchronous and long-running operation standard

For any user-triggered operation with a meaningful wait, explicitly define the
parts that apply.

## Trigger

What starts the operation?

## Immediate feedback

What changes promptly enough that the user knows the action registered?

## Active/pending state

How can the user tell the system is still working rather than frozen?

For input/recording systems, this may include live activity feedback. For data
loads, it may include stable placeholders or progress. Match the feedback to
the task.

## Preserved context

What remains stable while work is active?

Examples:

```text
focus
current selection
window/layout position
user input
target application/context
previous valid data when appropriate
```

## Duplicate-action policy

Can the action be triggered again while active? If not, prevent or ignore
unsafe duplicate activation.

## Completion

What visibly changes when the operation succeeds?

## Empty / unavailable

What happens when work succeeds but there is no meaningful result or the
capability is unavailable?

Do not use an error state for valid unavailability.

## Failure and recovery

What message and recovery path are available?

## Accessibility

As relevant, define:

- focus behavior;
- keyboard operation;
- platform accessibility labels/roles;
- screen-reader/VoiceOver announcements;
- reduced-motion treatment;
- non-color status cues;
- platform-appropriate busy/progress semantics.

---

# Interaction mechanics review

For changed interactive surfaces, explicitly consider the mechanics that apply.

## Keyboard

Check:

- expected activation keys;
- shortcuts;
- Escape/cancel behavior;
- focus visibility;
- focus movement and return;
- whether a global shortcut/gesture conflicts with ordinary platform behavior.

## Pointer and touch

Check:

- adequate target size where a visible target exists;
- accidental activation;
- click/press/hold thresholds;
- drag vs click distinction;
- preservation of ordinary system/app behavior when introducing global gestures;
- no critical hover-only action.

## Focus and windows

Check:

- whether transient UI steals focus;
- whether dialogs/popovers return focus appropriately;
- whether background utilities preserve the user's active application/context;
- whether modal behavior is truly necessary.

## Navigation and history

When navigation exists, check:

- Back/Forward;
- deep-link/share behavior where useful;
- accidental navigation recovery;
- preservation of meaningful state.

## Selection and modes

Check:

- current selection/mode is recognizable;
- pending work does not silently erase it;
- clear/reset behavior exists where useful;
- selection is not conveyed by color alone.

## Destructive or costly actions

Check:

- consequence is clear before activation;
- confirmation is proportionate;
- undo/recovery exists when practical;
- repeated activation is prevented.

---

# Accessibility

Accessibility is part of the interaction design, not a final bolt-on.

Use the platform's native accessibility model and established standards.

As relevant, consider:

```text
semantic roles/names
keyboard access
focus order and visibility
VoiceOver/screen-reader output
status announcements
dynamic type/text scaling
contrast
non-color cues
reduced motion
pointer/touch target size
alternative representations for visual-only information
```

Do not apply web-specific ARIA mechanics to native applications or native-only
semantics to web applications. Use the correct platform mechanism.

---

# Coder-facing plan standard

For non-trivial user-facing work, include a compact section such as:

```text
Heuristic-aware interaction requirements
```

Include only requirements that matter to that task, expressed as observable
behavior.

Example:

```text
- Starting the operation immediately makes its active state visible.
- The status surface does not steal focus from the user's current task.
- The operation cannot be triggered twice while already active.
- Success and failure both resolve the pending state.
- Failure preserves recoverable user output when possible.
- Status is not communicated by color or animation alone.
```

Do not tell the Coder to resolve unsettled product decisions by "applying
Nielsen's heuristics."

---

# Validation

Tests protect observable requirements; they do not prove a heuristic in the
abstract.

Use:

```text
heuristic finding
→ user risk
→ settled observable requirement
→ automated assertion when deterministic
→ rendered/runtime inspection when visual or OS behavior requires judgment
→ usability research when actual comprehension or behavior is the question
```

Good automated targets often include:

- duplicate activation prevention;
- state transitions;
- keyboard mechanics;
- focus behavior when testable;
- cancel/reset behavior;
- error vs empty/unavailable distinction;
- preservation of selected state;
- deterministic recovery behavior.

Rendered/runtime inspection is appropriate for things automation may not judge
well:

- status technically exists but is easy to miss;
- focus feels stolen or jumps unexpectedly;
- layout/status presentation appears unstable;
- animation is distracting;
- visual hierarchy obscures the primary task;
- a platform-standard interaction looks or behaves unexpectedly.

Do not turn screenshots or test counts into theater.

---

# Review standard

During implementation review, require revision for material heuristic failures
that affect task completion, comprehension, control, trust, accessibility, or
recovery, such as:

- no feedback during a meaningful wait;
- interface appears frozen while work occurs;
- accidental or duplicate activation;
- focus theft that breaks the user's task;
- irreversible action without proportionate recovery;
- unsupported choices;
- inconsistent terminology or control behavior;
- inaccessible interaction;
- critical meaning conveyed only by color, hover, or animation;
- errors with no usable recovery path;
- stale/mismatched state that can mislead the user.

Do not reopen materially sound work for theoretical violations that do not
create a concrete user problem.

---

## References

Jakob Nielsen / Nielsen Norman Group:

- *10 Usability Heuristics for User Interface Design*
- *Heuristic Evaluation Workbook*

Primary reference:

https://www.nngroup.com/articles/ten-usability-heuristics/
