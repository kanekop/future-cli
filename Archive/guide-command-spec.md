# Guide Command Implementation Specification

## Overview
Implement a `guide` command as a shortcut to interactive mode, with a subtle hint appearing after three consecutive errors to maintain the project's philosophy while providing a path for struggling users.

## Core Requirements

### 1. Error Tracking
Track consecutive failed attempts to execute `./future.sh` without proper flags.

### 2. Progressive Error Messages

#### First and Second Errors (Standard)
```
[FATAL] Core components missing. Future cannot be initialized.
        Required flags: --vision, --commit

USAGE:
  ./future.sh --vision "a world where..." --commit "what you will do now"

# Tip: A vision without a commit is just a dream.
```

#### Third Error (With Hint)
```
[FATAL] Core components missing. Future cannot be initialized. 💡 New here? Type 'guide' for beginner-friendly mode!
        Required flags: --vision, --commit

USAGE:
  ./future.sh --vision "a world where..." --commit "what you will do now"

# Tip: A vision without a commit is just a dream.
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
// In processCommand function, when handling future.sh errors:

// When a proper command fails (missing --vision or --commit)
if (!visionMatch || !commitMatch) {
    consecutiveErrors++;
    runErrorSequence(() => {
        setTimeout(createInputLine, 200);
    }, consecutiveErrors >= 3); // Pass hint flag
}

// Reset counter on successful command or non-future.sh commands
if (visionMatch && commitMatch) {
    consecutiveErrors = 0;
    // ... continue with success
}

// Also reset on other successful commands
if (cmd !== 'future' && cmd !== './future' && cmd !== 'future.sh' && cmd !== './future.sh') {
    consecutiveErrors = 0;
}
```

### Modified Error Sequence
```javascript
function runErrorSequence(onComplete, showHint = false) {
    let delay = 0;
    const errorLines = getErrorLines(showHint);
    errorLines.forEach((line, index) => {
        setTimeout(() => {
            const lineElement = addLine(line.text);
            // Note: Remove shareable-log class as per previous spec
            if (index === errorLines.length - 1 && onComplete) {
                onComplete();
            }
        }, delay);
        delay += 100;
    });
}

function getErrorLines(showHint = false) {
    const fatalMessage = showHint 
        ? `<span class="log-fatal">${t('fatal')} 💡 ${t('newUserHint')}</span>`
        : `<span class="log-fatal">${t('fatal')}</span>`;
    
    return [
        { text: fatalMessage },
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
    newUserHint: "初めてですか？'guide'と入力すると初心者向けモードが始まります！",
    // ...
}
```

### Help Command Update
Update the help command to include the guide option:

```javascript
// In help command output
addLine('  guide                                                     - Start beginner-friendly guided mode');
// Japanese: 
addLine('  guide                                                     - 初心者向けガイドモードを開始');
```

## User Experience Flow

### Scenario 1: Persistent Beginner
```bash
$ ./future.sh
[FATAL] Core components missing. Future cannot be initialized.
...

$ ./future.sh --vision
[FATAL] Core components missing. Future cannot be initialized.
...

$ ./future
[FATAL] Core components missing. Future cannot be initialized. 💡 New here? Type 'guide' for beginner-friendly mode!
...

$ guide
[INTERACTIVE MODE] Welcome to the guided future boot process!
...
```

### Scenario 2: Direct Guide Access
```bash
$ guide
[INTERACTIVE MODE] Welcome to the guided future boot process!
...
```

### Scenario 3: Error Counter Reset
```bash
$ ./future.sh
[FATAL] ... (error count: 1)

$ ls
future.sh   README.md   vision.txt (error count: reset to 0)

$ ./future.sh
[FATAL] ... (error count: 1, no hint yet)
```

## Testing Checklist

1. **Error Counter**
   - [ ] First error shows standard message
   - [ ] Second error shows standard message
   - [ ] Third consecutive error shows hint
   - [ ] Fourth+ errors continue showing hint

2. **Counter Reset**
   - [ ] Successful future.sh command resets counter
   - [ ] Other commands (ls, cat, etc.) reset counter
   - [ ] Guide command resets counter

3. **Guide Command**
   - [ ] `guide` starts interactive mode
   - [ ] `ガイド` starts interactive mode (Japanese)
   - [ ] Guide command works regardless of error count

4. **Language Support**
   - [ ] Hint appears in correct language
   - [ ] Guide command works in both language modes

## Design Philosophy Alignment

This implementation maintains the project's core philosophy:

1. **Friction First**: Users still encounter the error message and must fail before discovering the easier path
2. **Earned Discovery**: The hint only appears after genuine struggle (3 attempts)
3. **Intentional Choice**: Users must actively type 'guide' - it's not automatic
4. **Learning Journey**: The error → struggle → hint → guide flow mirrors real learning

## Edge Cases

1. **Mixed Commands**: Only future.sh variants count toward consecutive errors
2. **Language Switching**: Error counter persists across language changes
3. **Page Reload**: Error counter resets (acceptable limitation)
4. **Quick Succession**: No time-based reset - only command-based