# Share Prompt Implementation Specification

## Overview
Add an interactive prompt after successful future boot to ask users if they want to share their future. This creates a more intentional sharing experience and respects user privacy.

## User Experience Flow

### 1. After Successful Boot
```
Future process started successfully. (PID: 20250703)
Daemonizing... Your future is now running in the background. Keep committing.
Generating a shareable snapshot...

Would you like to share your future with others?
  > 1. Yes
    2. No
```

### 2. Visual Selection Feedback
The selected option is highlighted with `>` marker and accent color:

```
# When "Yes" is selected:
Would you like to share your future with others?
  > 1. Yes     ← Highlighted in green (--accent-color)
    2. No

# When "No" is selected:
Would you like to share your future with others?
    1. Yes
  > 2. No      ← Highlighted in green (--accent-color)
```

## Interaction Methods

### 1. Arrow Keys
- `↑` / `↓`: Navigate between options
- `Enter`: Confirm current selection

### 2. Number Keys
- `1`: Select and confirm "Yes"
- `2`: Select and confirm "No"

### 3. Shortcut Keys
- `y` / `Y`: Select and confirm "Yes"
- `n` / `N`: Select and confirm "No"
- Note: Shortcuts work in both English and Japanese modes

## Language Support

### English
```
Would you like to share your future with others?
  > 1. Yes
    2. No
```

### Japanese
```
あなたの未来を誰かと共有しますか？
  > 1. はい
    2. いいえ
```

## Behavior Specifications

### When "Yes" is Selected
1. Display the current share container
2. Show the generated screenshot
3. Display download and social share buttons
4. Scroll to top to show share options

### When "No" is Selected
1. Display confirmation message:
   - English: "Understood. Your future is already running.\nKeep committing!"
   - Japanese: "了解しました。あなたの未来はすでに動き始めています。\nコミットし続けてください！"
2. Return to normal command prompt
3. Store the snapshot data for potential later sharing

### Timeout Behavior
- After 3 minutes (180 seconds) of inactivity, automatically select "No"
- Display a subtle message: "Auto-selected: No (timeout)"

## Share-Last Command

### Command: `share-last`
Allows users to share their most recently booted future.

#### Usage
```bash
$ share-last
```

#### Behavior
1. Check if there's a stored future snapshot
2. If exists:
   - Display the share prompt again
   - If "Yes", show the share container with the stored snapshot
3. If not exists:
   - Display: "No future to share. Boot your future first with ./future.sh"

#### Storage
- Store the last successful boot data in memory:
  - Vision text
  - Commit text
  - Timestamp
  - Generated screenshot data

## Implementation Details

### New i18n Keys
```javascript
// English
en: {
    sharePrompt: 'Would you like to share your future with others?',
    shareYes: 'Yes',
    shareNo: 'No',
    shareDeclined: 'Understood. Your future is already running.\nKeep committing!',
    shareTimeout: 'Auto-selected: No (timeout)',
    shareLastNoData: 'No future to share. Boot your future first with ./future.sh',
    shareLastPrompt: 'Share your last booted future?',
    // ...
}

// Japanese
ja: {
    sharePrompt: 'あなたの未来を誰かと共有しますか？',
    shareYes: 'はい',
    shareNo: 'いいえ',
    shareDeclined: '了解しました。あなたの未来はすでに動き始めています。\nコミットし続けてください！',
    shareTimeout: '自動選択: いいえ（タイムアウト）',
    shareLastNoData: '共有する未来がありません。まず ./future.sh で未来を起動してください。',
    shareLastPrompt: '最後に起動した未来を共有しますか？',
    // ...
}
```

### State Management
```javascript
// Global variables to add
let sharePromptActive = false;
let sharePromptSelection = 1; // 1 for Yes, 2 for No
let lastBootedFuture = null; // Store last boot data
let shareTimeoutId = null; // Timeout reference
```

### Key Functions to Implement

#### 1. `showSharePrompt()`
Display the interactive share prompt with proper formatting

#### 2. `handleSharePromptKeydown(e)`
Handle keyboard input during share prompt:
- Arrow keys for navigation
- Number keys for direct selection
- Y/N shortcuts
- Enter for confirmation

#### 3. `updateSharePromptDisplay()`
Update the visual display of the prompt based on current selection

#### 4. `executeShareChoice(choice)`
Execute the selected action (share or decline)

#### 5. `storeLastBootedFuture(vision, commit, imageData)`
Store the boot data for potential later sharing

#### 6. `handleShareLastCommand()`
Handle the `share-last` command

### CSS Styling
```css
.share-prompt {
    margin-top: 1em;
    margin-bottom: 1em;
}

.share-option {
    padding-left: 2em;
    position: relative;
}

.share-option.selected {
    color: var(--accent-color);
}

.share-option.selected::before {
    content: '>';
    position: absolute;
    left: 0.5em;
}
```

## Testing Checklist

1. **Basic Flow**
   - [ ] Share prompt appears after successful boot
   - [ ] Visual selection indicator works correctly
   - [ ] Selecting "Yes" shows share container
   - [ ] Selecting "No" returns to command prompt

2. **Input Methods**
   - [ ] Arrow keys navigation
   - [ ] Number key selection (1, 2)
   - [ ] Y/N shortcuts
   - [ ] Enter key confirmation

3. **Language Support**
   - [ ] English mode displays correct text
   - [ ] Japanese mode displays correct text
   - [ ] Language persists through the flow

4. **Timeout**
   - [ ] 3-minute timeout auto-selects "No"
   - [ ] Timeout message appears
   - [ ] User can still interact before timeout

5. **Share-Last Command**
   - [ ] Command works after booting a future
   - [ ] Error message when no future exists
   - [ ] Shares the correct snapshot
   - [ ] Works across language modes

## Edge Cases

1. **Rapid Key Presses**
   - Debounce or queue key inputs appropriately

2. **Multiple Boots**
   - Only the most recent boot is stored for `share-last`

3. **Page Reload**
   - `share-last` data is lost (stored in memory only)
   - Consider localStorage for persistence (future enhancement)

4. **Interactive Mode**
   - Share prompt should also appear after interactive mode completion