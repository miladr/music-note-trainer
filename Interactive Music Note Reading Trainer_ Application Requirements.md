# Interactive Music Note Reading Trainer
## HTML/CSS/JavaScript Application Requirements

### 1. Objective

Build a desktop-first web application that teaches adult beginners how to recognize natural musical notes on a treble-clef staff.

The application must provide two complementary training modes:

1. **Staff → Note:** The application displays notes on a staff and the user identifies each note.
2. **Note → Staff:** The application gives the user a note name and octave, and the user identifies its correct vertical position on the staff.

The application should be simple, fast, visually minimal, and usable without an account or backend.

---

# 2. Technical Requirements

The application must:

- Be implemented using HTML, CSS, and JavaScript.
- Run entirely in the browser.
- Require no backend.
- Require no database.
- Be desktop-first.
- Remain reasonably usable on smaller screens.
- Use `localStorage` for settings and persistent statistics.
- Allow appropriate third-party JavaScript libraries.
- Use a music-notation library where useful rather than manually approximating musical notation.
- Use the Web Audio API or an appropriate audio library for note playback.
- Work after normal page load without requiring authentication.

The code should be modular enough that bass clef, accidentals, additional training modes, or other features could be added later.

---

# 3. Visual Design

Use a very simple educational interface.

Visual style:

- Black and white.
- White background.
- Black notation.
- Black/gray borders.
- Minimal decoration.
- Clear typography.
- Generous spacing.
- No gradients.
- No unnecessary animation.
- No game-like graphics.
- No childish visual treatment.

The musical staff should be the dominant element on the training screen.

The interface is intended primarily for adults who have little or no experience reading sheet music.

---

# 4. Musical Scope

Version 1 supports:

- Treble clef only.
- Natural notes only.
- No sharps.
- No flats.
- No key signatures.
- Multiple octaves.
- Ledger lines above and below the staff.
- Simple note heads are sufficient. Rhythmic notation is not being taught.

The supported pitch range should be wide enough to provide several ledger-line positions above and below the standard treble staff.

The exact minimum and maximum pitch should be defined centrally in configuration rather than hard-coded throughout the application.

---

# 5. Note Naming Systems

The user can select one of two naming systems before starting a round.

### Letter notation

C, D, E, F, G, A, B

### Solfège

Do, Re, Mi, Fa, Sol, La, Si

Use **Si**, not Ti.

Internally, notes should still have an unambiguous pitch identifier including octave, for example:

- C4
- D4
- B4
- C5
- A5

The naming preference affects what is displayed to the user, not the internal representation.

---

# 6. Application Structure

The main application should contain three functional areas:

1. Settings
2. Training
3. Round Results

The user should be able to repeatedly configure and start new rounds without reloading the page.

---

# 7. Training Settings

Before every round, show a settings panel.

The panel must contain:

### Training Type

- Staff → Note
- Note → Staff

### Difficulty

Selectable level:

1 through 10

Do not automatically increase or decrease difficulty.

The user always controls the level.

### Naming System

- C D E F G A B
- Do Re Mi Fa Sol La Si

### Sound

- On
- Off

### Start

Provide a prominent:

**Train Now**

button.

Starting a round locks the round's settings until that round is completed.

The user can change settings again before the next round.

Persist the most recently selected settings in `localStorage`.

---

# 8. Difficulty System

Difficulty levels must affect three things simultaneously:

1. Pitch range
2. Maximum interval/jump size
3. Randomness

Difficulty should be generated algorithmically rather than using ten static exercise lists.

## Level 1

Very beginner-friendly.

Characteristics:

- Small pitch range around the central/easy treble-clef area.
- Mostly neighboring notes.
- Strong sequential behavior.
- Primarily ascending or descending patterns.
- No difficult ledger-line jumps.

Example:

C4 → D4 → E4 → F4 → G4 → A4

## Levels 2 to 3

Gradually:

- Expand pitch range.
- Allow repeated notes.
- Introduce occasional skips.
- Reduce predictability.

Example:

E4 → F4 → G4 → G4 → A4 → C5

## Levels 4 to 5

Use the full normal staff range.

Introduce:

- Larger intervals.
- More direction changes.
- Greater randomization.

Example:

E4 → G4 → F4 → B4 → A4 → C5

## Levels 6 to 7

Introduce ledger-line notes.

Allow:

- Notes below the staff.
- Notes above the staff.
- Moderate octave movement.
- Larger interval jumps.

## Levels 8 to 9

Use a wide pitch range.

Allow:

- Multiple ledger lines.
- Large intervals.
- Frequent direction changes.
- High randomness.
- Significant octave movement.

## Level 10

Use the complete configured pitch range.

Sequence generation should be effectively unrestricted within that range.

Example:

D4 → G5 → F4 → C6 → A3 → E5

Level 10 should require actual note recognition rather than allowing the user to predict the next note from a scale pattern.

---

# 9. Round Generation

A round represents one staff.

Generate the maximum reasonable number of notes that visually fit across the available staff width while maintaining clear spacing.

Do not overcrowd the staff.

The number of notes may therefore depend on:

- available width
- note spacing
- clef width
- margins
- notation-library requirements

The layout system should calculate this rather than relying on an arbitrary fixed number if practical.

Repeated pitches are allowed.

The generated sequence must obey the selected difficulty rules.

---

# 10. Training 1: Staff → Note

## Purpose

Teach the user to look at a written note and determine its name.

## Round Initialization

At the beginning of the round:

- Draw one treble-clef staff.
- Generate the full sequence.
- Display all generated notes on the staff simultaneously.
- Include required ledger lines.
- Set the first note as active.

Only one note is answered at a time.

The user progresses from left to right.

## Active Note

Clearly indicate which note the user currently needs to identify.

Use a subtle black-and-white mechanism such as:

- outline
- pointer
- underline
- light gray highlight

Do not reveal the answer.

Previously solved notes remain visually unchanged.

Do not write their note names under the staff after they are solved.

---

# 11. Training 1 Answer Methods

Both answer methods must always be available simultaneously.

There is no setting for selecting an answer method.

## Method A: Note Buttons

Display seven buttons:

C D E F G A B

or, when solfège is selected:

Do Re Mi Fa Sol La Si

## Method B: Piano Keyboard

Display a realistic piano keyboard.

It should visually contain:

- white keys
- black keys
- realistic relative positioning

The training exercises contain natural notes only.

Black keys can remain visible for realism but should not be valid exercise answers.

Clicking a natural piano key submits that pitch class as the user's answer.

When sound is enabled, clicking a piano key should play its corresponding pitch.

The piano keyboard should cover enough octaves to look and behave naturally for the application's supported pitch range.

For Training 1, the required answer is the **note name/pitch class**, not the octave.

For example, if the staff shows C5, selecting any appropriate C answer represents `C`.

---

# 12. Training 1 Answer Logic

If the answer is correct:

1. Add `+1` to the round score.
2. Add `+1` to the session score.
3. Play the correct pitch if sound is enabled.
4. Mark the current note internally as solved.
5. Move the active indicator to the next note.

If the answer is incorrect:

1. Subtract `1` from the round score.
2. Subtract `1` from the session score.
3. Show simple negative feedback such as `Incorrect. Try again.`
4. Keep the same note active.
5. Allow another answer.

There is no limit on attempts.

The user cannot advance until the correct answer is selected.

Do not reveal the correct answer after a wrong attempt.

---

# 13. Training 2: Note → Staff

## Purpose

Teach the user to convert a written note name into its correct position on the treble staff.

## Staff

Display an empty treble-clef staff.

The staff must support clicking:

- lines
- spaces
- ledger-line positions above the staff
- ledger-line positions below the staff

The clickable vertical positions should correspond to valid diatonic note positions.

---

# 14. Training 2 Prompt

Show one target note at a time.

The target must include enough information to identify a unique staff position.

For letter notation:

**C4**

For solfège:

**Do4**

Octave is mandatory in this training because the same note name can occur at multiple positions on the staff.

Only one target appears at a time.

The next target must not be displayed until the current one is correctly solved.

---

# 15. Training 2 Interaction

The user answers by clicking/tapping the correct vertical position on the staff.

The horizontal position does not represent the answer.

The application should map the click to the nearest valid line/space position.

Use a reasonable click tolerance so the user does not need pixel-perfect precision.

The user should be able to select ledger-line positions even when no permanent ledger line is currently visible.

Provide subtle hover feedback showing which vertical position would be selected.

Do not display the note name as part of that hover feedback.

---

# 16. Training 2 Correct Answer

When the selected position is correct:

1. Add `+1` to round score.
2. Add `+1` to session score.
3. Briefly display the note at the selected position.
4. Play the pitch if sound is enabled.
5. Advance to the next target.

The previous correctly answered note should **not remain on the staff**.

Each target is an independent recognition task.

---

# 17. Training 2 Wrong Answer

When the selected position is incorrect:

1. Subtract `1` from round score.
2. Subtract `1` from session score.
3. Display `Incorrect. Try again.`
4. Keep the current target.
5. Do not reveal the correct position.
6. Allow another click.

The user remains on that note until it is correctly located.

---

# 18. Scoring

Use a deliberately simple scoring model.

Correct answer:

`+1`

Each incorrect attempt:

`-1`

Example:

A round contains 15 notes.

The user eventually solves all 15 but makes 4 mistakes.

Round score:

`15 - 4 = 11`

There is no separate first-attempt accuracy score required.

The primary metric is points earned while solving notes.

Negative round/session scores must be allowed.

Do not clamp scores to zero.

---

# 19. Session Score

A session begins when the application is opened or when a new session is explicitly started.

Maintain:

- current round score
- cumulative session score
- rounds completed
- total notes solved

During training, display at least:

**Round Score: X**

**Session Score: Y**

Keep this display compact so it does not distract from the staff.

---

# 20. Round Completion

A round ends after every generated note has been correctly solved.

Show a simple results state containing:

- Round complete
- Difficulty level
- Number of notes solved
- Number of mistakes
- Round score
- Session score

Provide:

**Train Again**

This returns the user to the settings panel.

The previous settings should remain selected.

The user can immediately start another round with the same settings or modify them.

There is no overall final exercise.

Training is effectively endless through repeated rounds.

---

# 21. Sound

Sound can be enabled or disabled in settings.

When enabled:

### Training 1

Play sound when:

- the user clicks a valid piano key
- the correct answer is confirmed

### Training 2

Play the target pitch when the user correctly locates it.

Use actual pitch/octave frequencies.

For example, C4 and C5 must sound one octave apart.

Sound should be short and clean, similar to a basic piano tone.

Avoid long sustained notes.

Audio should not overlap excessively when the user answers quickly.

---

# 22. Staff Interaction Model

Internally represent staff positions using diatonic steps rather than arbitrary pixel coordinates.

For example, define a central reference pitch and map each natural pitch to a vertical staff index.

The rendering system should understand:

- lines
- spaces
- ledger positions
- octave boundaries

Do not implement note recognition by maintaining unrelated hard-coded Y coordinates for every note.

Use a reusable mapping such as:

`pitch → diatonic staff index → Y coordinate`

and:

`click Y coordinate → nearest staff index → pitch`

This mapping should be shared by exercise generation and answer validation.

---

# 23. Ledger Lines

Ledger lines are an important part of higher difficulty levels.

When a note requires ledger lines:

- Draw only the ledger lines required for that note.
- Keep them centered around the note head.
- Do not extend them across the full staff.
- Follow conventional sheet-music appearance.

Training 2 must also allow the user to select positions where ledger lines would be required.

---

# 24. Piano Keyboard Behavior

The piano should visually resemble a conventional keyboard.

Requirements:

- Correct 2-black/3-black repeating grouping.
- White and black keys positioned realistically.
- White keys are clickable.
- Natural note keys can be used as answers.
- Black keys do not need to participate in scoring.
- Keys should provide pressed-state visual feedback.
- Sound should correspond to the clicked key when enabled.

The piano should not dominate the screen. The staff remains the primary learning interface.

---

# 25. State Model

Maintain application state similar to:

```text
settings
    trainingType
    difficulty
    namingSystem
    soundEnabled

session
    score
    roundsCompleted
    notesSolved

round
    notes[]
    currentIndex
    score
    mistakes
    completed

currentQuestion
    pitch
    pitchClass
    octave
    staffPosition
```

Exact implementation may differ, but training logic should remain separate from DOM rendering.

---

# 26. localStorage

Use `localStorage`.

Persist at least:

- last training type
- last difficulty
- naming system
- sound preference
- relevant cumulative statistics

Do not require a server.

The application must still function if persistent statistics are unavailable. In that case, use in-memory state for the current session.

Provide a simple **Reset Progress** control for clearing saved training statistics.

The action should require confirmation.

---

# 27. Random Exercise Generator

Create a dedicated exercise-generation module/function.

Input should include at least:

```text
difficulty
numberOfNotes
supportedPitchRange
```

Output:

```text
array of pitches
```

Example:

```javascript
["D4", "G5", "F4", "C6", "A3", "E5"]
```

The generator must enforce difficulty constraints.

Avoid implementing difficulty as ten unrelated hard-coded arrays.

Instead derive generator parameters from level, such as:

```text
minimumPitch
maximumPitch
maximumDiatonicJump
sequentialProbability
repeatProbability
randomness
```

These parameters should progressively change from levels 1 through 10.

---

# 28. Randomness Quality

Exercise generation should avoid obviously broken sequences.

For example:

- Do not generate the exact same sequence repeatedly.
- Do not accidentally generate an entire high-level round containing one repeated note.
- Avoid excessive repetition unless allowed by the level's generation rules.
- Low levels should feel structured.
- High levels should feel unpredictable.

Random does not need to be cryptographically secure.

---

# 29. Round Capacity

Determine how many notes can fit on the staff using available width.

Reserve space for:

- treble clef
- left/right margins
- readable note spacing

For example:

```text
usableWidth = staffWidth - clefArea - margins
noteCapacity = floor(usableWidth / minimumNoteSpacing)
```

The actual implementation can use the selected notation library's layout system.

The objective is to fill the staff with a useful number of exercises without making notation crowded.

---

# 30. Feedback

Feedback should remain minimal.

Correct:

**Correct**

Incorrect:

**Incorrect. Try again.**

Round complete:

**Round Complete**

Avoid modal dialogs during individual questions.

Feedback should appear near the training area without shifting the staff significantly.

Correct feedback can disappear automatically when moving to the next note.

Incorrect feedback remains until the next attempt.

---

# 31. Keyboard and Accessibility

Where practical:

- Buttons must be keyboard accessible.
- Controls should have visible focus states.
- Text must have sufficient contrast.
- Do not rely exclusively on color to communicate state.
- Staff interaction should have sufficiently large hit areas.
- UI controls should use semantic HTML.

The application is desktop-first, so mouse interaction is the primary staff-position interaction.

Touch support should still work where practical.

---

# 32. Responsive Behavior

Primary target is desktop.

Recommended minimum comfortable viewport:

approximately 1024 px wide.

On narrower displays:

- Allow the staff to resize.
- Reduce note capacity when necessary.
- Keep notation readable.
- Stack controls vertically where appropriate.
- Keep piano keys usable.

Do not solve small-screen layouts by shrinking the staff until notation becomes difficult to read.

---

# 33. Suggested Application Layout

A typical training screen can be structured as:

```text
----------------------------------------------------------
Music Note Trainer

Training: Staff → Note       Level: 4
Round Score: 7               Session Score: 31

----------------------------------------------------------

                   TREBLE STAFF

      ♩       ♩       ♩       ♩       ♩       ♩
----------------------------------------------------------
----------------------------------------------------------
----------------------------------------------------------
----------------------------------------------------------
----------------------------------------------------------

                    Incorrect. Try again.

             [ C ] [ D ] [ E ] [ F ] [ G ] [ A ] [ B ]

              realistic piano keyboard

----------------------------------------------------------
```

For Training 2:

```text
----------------------------------------------------------
Music Note Trainer

Training: Note → Staff       Level: 6
Round Score: 4               Session Score: 38

Find:

                         C5

----------------------------------------------------------

                     TREBLE STAFF

----------------------------------------------------------
----------------------------------------------------------
----------------------------------------------------------
----------------------------------------------------------
----------------------------------------------------------

                  Click the correct position

----------------------------------------------------------
```

These diagrams communicate structure only and are not strict visual designs.

---

# 34. Suggested Code Organization

Keep responsibilities separated.

For example:

```text
/index.html
/css/
    styles.css
/js/
    app.js
    state.js
    settings.js
    music.js
    difficulty.js
    exercise-generator.js
    staff-renderer.js
    piano.js
    audio.js
    scoring.js
    storage.js
```

A simpler structure is acceptable if code quality remains good.

Avoid placing the entire application in one large JavaScript function.

---

# 35. Music Data Model

Use a canonical internal pitch format.

Recommended:

```javascript
{
    name: "C",
    octave: 4,
    pitch: "C4",
    solfege: "Do",
    staffIndex: 0
}
```

The precise structure can differ.

The important requirement is that:

- notation name
- solfège name
- octave
- audio pitch
- staff position

all derive from the same musical representation.

This prevents rendering, scoring, and sound from disagreeing about a pitch.

---

# 36. Important Musical Rule

Middle C is:

**C4**

Treble-clef positions and octave numbering must follow standard scientific pitch notation.

Validate the mapping carefully.

Incorrect octave/staff mappings would undermine the educational purpose of the application.

---

# 37. Training 1 Acceptance Criteria

Training 1 is complete when:

- A treble staff renders correctly.
- A full round of notes is generated.
- Difficulty affects sequence generation.
- All notes appear simultaneously.
- Only one note is active at a time.
- User can answer using note buttons.
- User can answer using piano keys.
- Both methods are visible simultaneously.
- Wrong answers subtract one point.
- Wrong answers do not advance.
- Correct answers add one point.
- Correct answers advance to the next note.
- Sound plays correctly when enabled.
- Ledger-line notes render correctly.
- Round completion occurs after the last note.
- Round and session scores are shown.

---

# 38. Training 2 Acceptance Criteria

Training 2 is complete when:

- An empty treble staff renders.
- One target pitch is displayed at a time.
- Target includes octave.
- User can click every supported staff position.
- Hover/selection snaps to valid line/space positions.
- Ledger positions are selectable.
- Wrong positions subtract one point.
- Wrong positions do not advance.
- Correct positions add one point.
- Correct positions advance.
- Correct note can be briefly shown for feedback.
- Previous solved notes do not remain on the staff.
- Sound plays the correct pitch when enabled.
- Round completion occurs after all generated targets.
- Round and session scores are shown.

---

# 39. Difficulty Acceptance Criteria

Difficulty must be perceptibly different.

A test comparing Levels 1, 5, and 10 should show:

### Level 1

- Narrow range.
- Mostly neighboring notes.
- Predictable movement.
- Few or no ledger-line notes.

### Level 5

- Full normal staff.
- Moderate intervals.
- Noticeably randomized sequences.

### Level 10

- Wide pitch range.
- Multiple ledger-line positions.
- Large jumps.
- Octave changes.
- Highly randomized sequences.

Level 10 should not be solvable simply by assuming the next note follows the previous note.

---

# 40. Persistence Acceptance Criteria

After changing settings and refreshing:

- selected difficulty remains
- naming system remains
- training type remains
- sound preference remains

Saved statistics should persist according to the implemented progress model.

Reset Progress must remove saved statistics without breaking saved application defaults/settings unless explicitly intended.

---

# 41. Error Handling

The application should fail gracefully.

Examples:

- If audio cannot initialize, training still works without audio.
- If `localStorage` is unavailable, use temporary in-memory state.
- If a notation library fails to initialize, display a meaningful error rather than a blank screen.
- Prevent double-clicks or rapid inputs from advancing through multiple questions accidentally.

---

# 42. Out of Scope for Version 1

Do not implement unless required for technical reasons:

- Bass clef
- Alto clef
- Tenor clef
- Sharps
- Flats
- Key signatures
- Chords
- Multiple simultaneous notes
- Rhythm training
- Note duration exercises
- Time signatures
- MIDI input
- User accounts
- Cloud synchronization
- Backend
- Database
- Multiplayer
- Leaderboards
- Automatic difficulty changes
- Learning/tutorial mode

Keep the architecture extensible enough that some of these could be added later.

---

# 43. Primary User Flow

The expected flow is:

```text
Open application
      ↓
Configure training
      ↓
Choose training type
      ↓
Choose Level 1-10
      ↓
Choose C-D-E or Do-Re-Mi
      ↓
Choose sound On/Off
      ↓
Train Now
      ↓
Complete one full staff
      ↓
Receive +1 / -1 during exercise
      ↓
Round Complete
      ↓
See round + session results
      ↓
Train Again
      ↓
Change settings or start another round
```

This loop continues indefinitely.

---

# 44. Core Product Principle

The application should train **recognition**, not guessing.

At low difficulty, predictable sequences help beginners understand how notes move through staff lines and spaces.

As difficulty increases, sequence predictability, restricted range, and small intervals should progressively disappear.

By Level 10, the learner should need to independently recognize each pitch and its corresponding staff position.

The interface should support this goal without introducing unrelated music-theory complexity.