# Guide Command Implementation Specification (Enhanced)

## Overview
Implement a `guide` command as a shortcut to interactive mode, with a subtle hint appearing after three consecutive errors (including invalid commands) to maintain the project's philosophy while providing a more welcoming path for struggling users.

## Core Requirements

### 1. Error Tracking
Track consecutive failed attempts for:
- `./future.sh` without proper flags
- Any invalid/unknown commands (e.g., `blahblah`, `help me`, etc.)

### 2. Progressive Error Messages

#### First and Second Errors (Standard)

**For future.sh errors:**
```
[FATAL] Core components missing. Future cannot be initialized.
        Required flags: --vision, --commit

USAGE:
  ./future.sh --vision "a world where..." --commit "what you will do now"

# Tip: A vision without a commit is just a dream.
```

**For invalid commands:**
```
zsh: command not found: blahblah
```

#### Third Error (With Hint)

**For future.sh errors:**
```
[FATAL] Core components missing. Future cannot be initialized.
        Required flags: --vision, --commit

USAGE:
  ./future.sh --vision "a world where..." --commit "what you will do now"

# Tip: A vision without a commit is just a dream.

💡 New here? Type 'guide' for beginner-friendly mode!
```

**For invalid commands:**
```
zsh: command not found: blahblah

💡 New here? Type 'guide' for beginner-friendly mode!
```

### 3. Guide Command
- Command: `guide` (English) or `ガイド` (Japanese)
- Action: Starts interactive mode immediately
- No additional parameters needed

## Implementation Details

### State Management
```javascript
// Add to global variables
let consecutiveErrors = 0;
```

### Error Tracking Logic
```javascript
// In processCommand function:

// For future.sh errors
if (futureMatch && (!visionMatch || !commitMatch)) {
    consecutiveErrors++;
    runErrorSequence(() => {
        if (consecutiveErrors >= 3) {
            showGuideHint();
        }
        setTimeout(createInputLine, 200);
    });
}

// For invalid commands (command not found)
else if (command && !isValidCommand(command)) {
    consecutiveErrors++;
    addLine(`zsh: ${t('commandNotFound')}: ${escapeHtml(command.split(' ')[0])}`);
    if (consecutiveErrors >= 3) {
        showGuideHint();
    }
    createInputLine();
}

// Reset counter on any successful/valid command
else if (isValidCommand(command) || (futureMatch && visionMatch && commitMatch)) {
    consecutiveErrors = 0;
    // ... continue with command processing
}
```

### Helper Functions
```javascript
// Check if a command is valid
function isValidCommand(command) {
    const cmd = command.split(' ')[0];
    const validCommands = [
        'ls', 'cat', 'clear', 'pwd', 'whoami', 'help', 'lang', 
        'guide', 'ガイド',
        'future', './future', 'future.sh', './future.sh'
    ];
    return validCommands.includes(cmd);
}

// Show guide hint as a separate line
function showGuideHint() {
    addLine('');  // Empty line for separation
    addLine(`<span class="log-info">💡 ${t('newUserHint')}</span>`);
}
```

### Modified Error Sequence (for future.sh)
```javascript
function runErrorSequence(onComplete) {
    let delay = 0;
    const errorLines = getErrorLines();
    errorLines.forEach((line, index) => {
        setTimeout(() => {
            const lineElement = addLine(line.text);
            if (index === errorLines.length - 1 && onComplete) {
                onComplete();
            }
        }, delay);
        delay += 100;
    });
}

// Keep the original error lines without hint
function getErrorLines() {
    return [
        { text: `<span class="log-fatal">${t('fatal')}</span>` },
        { text: t('requiredFlags') },
        { text: '' },
        { text: t('usage') },
        { text: t('usageExample') },
        { text: '' },
        { text: `<span class="log-tip">${t('tip')}</span>` }
    ];
}
```

### Guide Command Implementation
```javascript
// In processCommand function, add before checking for future.sh:

if (cmd === 'guide' || cmd === 'ガイド') {
    consecutiveErrors = 0; // Reset error counter
    startInteractiveMode();
    return;
}
```

### i18n Updates
```javascript
// English
en: {
    // ... existing keys ...
    newUserHint: "New here? Type 'guide' for beginner-friendly mode!",
    // ...
}

// Japanese
ja: {
    // ... existing keys ...
    newUserHint: "初めてですか？'ガイド'と入力すると初心者向けモードが始まります！",
    // ...
}
```

### CSS for Guide Hint
```css
.log-info {
    color: var(--prompt-color);
    font-weight: bold;
}
```

### Help Command Update
Update the help command to include the guide option:

```javascript
// In help command output
addLine('  guide                                                     - Start beginner-friendly guided mode');
// Japanese: 
addLine('  ガイド                                                     - 初心者向けガイドモードを開始');
```

## User Experience Flow

### Scenario 1: Mixed Invalid Commands
```bash
$ blahblah
zsh: command not found: blahblah

$ helpme
zsh: command not found: helpme

$ what
zsh: command not found: what

💡 New here? Type 'guide' for beginner-friendly mode!

$ guide
[INTERACTIVE MODE] Welcome to the guided future boot process!
...
```

### Scenario 2: Mixed future.sh and Invalid Commands
```bash
$ ./future.sh
[FATAL] Core components missing. Future cannot be initialized.
...

$ blahblah
zsh: command not found: blahblah

$ ./future
[FATAL] Core components missing. Future cannot be initialized.
...
# Tip: A vision without a commit is just a dream.

💡 New here? Type 'guide' for beginner-friendly mode!
```

### Scenario 3: Direct Guide Access
```bash
$ guide
[INTERACTIVE MODE] Welcome to the guided future boot process!
...
```

### Scenario 4: Error Counter Reset
```bash
$ blahblah
zsh: command not found: blahblah (error count: 1)

$ ls
future.sh   README.md   vision.txt (error count: reset to 0)

$ invalid
zsh: command not found: invalid (error count: 1, no hint yet)
```

## Testing Checklist

1. **Error Counter (All Commands)**
   - [ ] First invalid command shows standard message
   - [ ] Second invalid command shows standard message
   - [ ] Third consecutive error shows hint on new line
   - [ ] Fourth+ errors continue showing hint

2. **Mixed Error Types**
   - [ ] future.sh errors count toward total
   - [ ] Invalid commands count toward total
   - [ ] Mix of both types triggers hint at 3 total

3. **Counter Reset**
   - [ ] Any valid command resets counter
   - [ ] Successful future.sh command resets counter
   - [ ] Guide command resets counter

4. **Guide Command**
   - [ ] `guide` starts interactive mode
   - [ ] `ガイド` starts interactive mode (Japanese)
   - [ ] Guide command works regardless of error count

5. **Language Support**
   - [ ] Hint appears in correct language
   - [ ] Japanese mode shows 'ガイド' in hint
   - [ ] English mode shows 'guide' in hint

6. **Visual Design**
   - [ ] Hint appears on separate line with empty line above
   - [ ] Hint uses info color (blue) with emoji
   - [ ] Hint is clearly visible and separated

## Design Philosophy Alignment

This enhanced implementation maintains the project's core philosophy while being more welcoming:

1. **Inclusive Friction**: All types of errors lead to discovery, not just future.sh attempts
2. **Universal Struggle**: Recognizes that users might be lost and trying random commands
3. **Clear Guidance**: Hint appears prominently on its own line after genuine confusion
4. **Maintained Journey**: Users still must actively type 'guide' - preserving intentionality

## Edge Cases

1. **Empty Commands**: Pressing Enter with no text doesn't increment counter
2. **Valid Command Variants**: All variations of future.sh are recognized as attempts
3. **Whitespace**: Commands with only whitespace don't increment counter
4. **Case Sensitivity**: Commands are case-sensitive (standard shell behavior)
5. **Language Switching**: Error counter persists across language changes
6. **Page Reload**: Error counter resets (acceptable limitation)