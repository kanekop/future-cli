# Cat Command Enhanced Specification

## Overview
Implement a realistic `cat` command with tab completion and special handling for sensitive files, maintaining the shell authenticity.

## Core Features

### 1. Basic Cat Functionality

```bash
$ cat README.md
# Displays README content

$ cat vision.txt
Your vision awaits. Use ./boot_future.sh to define it.

$ cat
cat: missing operand
Try 'cat --help' for more information.
```

### 2. Tab Completion Implementation

#### Behavior
When user types `cat ` (with space) and presses Tab:

```bash
$ cat [TAB]
boot_future.sh   README.md   vision.txt
```

When user types partial filename and presses Tab:

```bash
$ cat v[TAB]
# Auto-completes to:
$ cat vision.txt

$ cat b[TAB]
# Auto-completes to:
$ cat boot_future.sh

$ cat R[TAB]
# Auto-completes to:
$ cat README.md
```

#### Multiple Matches
If multiple files match the prefix:

```bash
$ cat boot[TAB]
# Shows:
boot_future.sh

$ cat [TAB][TAB]
# Shows all available files:
boot_future.sh   README.md   vision.txt
```

### 3. Special File Handling

#### boot_future.sh - Permission Denied
```bash
$ cat boot_future.sh
cat: boot_future.sh: Permission denied (source is protected)
```

Alternative messages (choose one that fits best):
- `cat: boot_future.sh: Operation not permitted (binary file)`
- `cat: boot_future.sh: Cannot display binary file`
- `cat: boot_future.sh: Access restricted by future-daemon`

## Implementation Details

### File System Structure
```javascript
const fileSystem = {
  'README.md': `# the-future.sh

A tiny CLI simulator to boot your future.

Run ./boot_future.sh --vision "your vision" --commit "your action"`,
  
  'vision.txt': 'Your vision awaits. Use ./boot_future.sh to define it.',
  
  'boot_future.sh': null  // Special handling - cannot be displayed
};
```

### Tab Completion Logic
```javascript
function handleTabCompletion(currentInput) {
  const parts = currentInput.split(' ');
  
  if (parts[0] === 'cat' && parts.length === 2) {
    const partial = parts[1];
    const files = ['boot_future.sh', 'README.md', 'vision.txt'];
    
    // Find matching files
    const matches = files.filter(file => file.startsWith(partial));
    
    if (matches.length === 0) {
      // No matches - do nothing
      return { action: 'none' };
    } else if (matches.length === 1) {
      // Single match - auto-complete
      return { 
        action: 'complete',
        value: `cat ${matches[0]}`
      };
    } else {
      // Multiple matches - show options
      return {
        action: 'show-options',
        options: matches
      };
    }
  }
}
```

### Event Handling
```javascript
inputElement.addEventListener('keydown', (e) => {
  if (e.key === 'Tab') {
    e.preventDefault();
    
    const result = handleTabCompletion(inputElement.value);
    
    switch(result.action) {
      case 'complete':
        inputElement.value = result.value;
        break;
      case 'show-options':
        displayOptions(result.options);
        break;
    }
  }
});
```

## Visual Specifications

### Tab Completion Display
When showing multiple options:
```
$ cat [TAB]
boot_future.sh   README.md   vision.txt
$ cat █
```
- Options appear on a new line
- Prompt is redisplayed below
- Current input is preserved

### File Content Display
- README.md: Show actual markdown content (simplified version is fine)
- vision.txt: Show the philosophical message
- boot_future.sh: Show permission error in red

## Edge Cases

### 1. No Input After Cat
```bash
$ cat[TAB]
# No space after cat - no completion
```

### 2. Complete Command
```bash
$ cat README.md[TAB]
# Command is complete - no action
```

### 3. Non-existent File
```bash
$ cat nonexistent.txt
cat: nonexistent.txt: No such file or directory
```

### 4. Multiple Spaces
```bash
$ cat    README.md
# Should work normally (trim spaces)
```

## User Experience Goals

1. **Authentic Shell Feel**: Behaves like a real terminal
2. **Discoverability**: Tab completion helps users explore
3. **Mystery Preservation**: boot_future.sh remains "protected"
4. **Error Clarity**: Clear messages that maintain immersion

## Testing Checklist

- [ ] Tab completion with no input
- [ ] Tab completion with partial filenames
- [ ] Tab completion with exact matches
- [ ] Cat with valid files
- [ ] Cat with boot_future.sh (permission denied)
- [ ] Cat with non-existent files
- [ ] Cat with no arguments
- [ ] Multiple tab presses behavior
- [ ] Case sensitivity handling