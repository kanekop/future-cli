## Screenshot Generation Scope Modification Specification

### Current Issue
The shareable screenshot currently captures the entire terminal session, including:
- Initial error messages from the intro animation
- Failed command attempts
- All user interactions
- The successful command and its output

This results in screenshots that are too long and include unnecessary context.

### Required Change
Limit the screenshot to capture **only** the successful command execution and its output.

### Scope Definition

#### What to INCLUDE in the screenshot:
1. The user's successful command line:
   ```
   $ ./future.sh --vision "xxxxx" --commit "yyyyy"
   ```
2. All output generated after this command:
   - "Initializing future with new parameters..." (or Japanese equivalent)
   - Vision locked message
   - Commit registered message
   - Boot sequence complete
   - Process started message
   - Daemonizing message
   - Snapshot generation message

#### What to EXCLUDE from the screenshot:
- Any previous error messages
- The intro animation sequence
- Failed command attempts
- Any commands/output before the successful execution
- Interactive mode prompts (if using --interactive)

### Implementation Details

#### Current Implementation (Lines ~1190)
The current code uses the class `shareable-log` to mark lines for inclusion in the screenshot. This class is being added to too many elements.

#### Required Changes:

1. **Remove `shareable-log` class from**:
   - Error sequence lines in `runErrorSequence()`
   - Intro animation command line
   - Any failed command attempts

2. **Add `shareable-log` class only to**:
   - The successful command line (when it has valid --vision and --commit)
   - All lines generated by `runSuccessSequence()`
   - The "generating snapshot" message

3. **Modify the command processing**:
   ```javascript
   // In processCommand function
   // Only add shareable-log class when command is valid
   if (visionMatch && commitMatch) {
       lineElement.classList.add('shareable-log');
       // ... rest of success handling
   }
   ```

### Visual Example

#### Before (Current - Too Long):
```
$ ./future.sh
[FATAL] Core components missing...
        Required flags: --vision, --commit
USAGE:
  ./future.sh --vision "a world where..." --commit "what you will do now"
# Tip: A vision without a commit is just a dream.
$ ./future.sh --vision "a" --commit "b"  ← Start screenshot here
Initializing future with new parameters...
[INFO] Vision locked: "a"
[INFO] First commit registered: "b"
[ OK ] Boot sequence complete.
Future process started successfully. (PID: 20250703)
Daemonizing... Your future is now running in the background. Keep committing.
Generating a shareable snapshot...
```

#### After (Desired - Concise):
```
$ ./future.sh --vision "a" --commit "b"  ← Start screenshot here
Initializing future with new parameters...
[INFO] Vision locked: "a"
[INFO] First commit registered: "b"
[ OK ] Boot sequence complete.
Future process started successfully. (PID: 20250703)
Daemonizing... Your future is now running in the background. Keep committing.
Generating a shareable snapshot...
```

### Testing Requirements

1. **Standard Mode**: Screenshot should only include successful command and its output
2. **Interactive Mode**: Screenshot should only include the final command and output, not the Q&A prompts
3. **Failed Commands**: No screenshot generated for invalid commands
4. **Multiple Attempts**: Only the successful attempt should be in the screenshot

### Code Locations to Modify

1. **runIntroAnimation()** (~line 520): Remove `shareable-log` class
2. **runErrorSequence()** (~line 430): Remove `shareable-log` class
3. **processCommand()** (~line 800): Add `shareable-log` only for valid commands
4. **handleInteractiveInput()** (~line 1080): Ensure only final command has `shareable-log`

### Expected Outcome
Users will receive clean, focused screenshots that show only their successful future boot process, making them more shareable and visually appealing on social media.