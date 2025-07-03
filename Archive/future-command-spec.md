# Future Command Name Change Specification

## Overview
Change the main command from `boot_future.sh` to `future.sh`, while accepting multiple variations for better user experience.

## Command Variations to Accept

All of the following should work identically:
- `future`
- `./future`
- `future.sh`
- `./future.sh`

## Implementation Details

### 1. Update Command Recognition Regex

**Current regex:**
```javascript
const bootRegex = /^(?:\.\/)?(boot_future\.sh)(.*)$/;
```

**New regex:**
```javascript
const futureRegex = /^(?:\.\/)?(?:future(?:\.sh)?)(.*)$/;
```

This regex matches:
- `future` 
- `./future`
- `future.sh`
- `./future.sh`

### 2. Update File System

**Current:**
```javascript
const fileSystem = {
    'README.md': { /* ... */ },
    'vision.txt': { /* ... */ },
    'boot_future.sh': {
        content: null,
        protected: true
    }
};
```

**New:**
```javascript
const fileSystem = {
    'README.md': { /* ... */ },
    'vision.txt': { /* ... */ },
    'future.sh': {  // Changed from boot_future.sh
        content: null,
        protected: true
    }
};
```

### 3. Update Available Files for `ls` Command

**Current:**
```javascript
const availableFiles = ['boot_future.sh', 'README.md', 'vision.txt'];
```

**New:**
```javascript
const availableFiles = ['future.sh', 'README.md', 'vision.txt'];
```

### 4. Update All Display Text

Search and replace throughout the codebase:
- `./boot_future.sh` → `./future.sh`
- `boot_future.sh` → `future.sh`

Key locations to update:
- Error messages
- Usage examples
- Help text
- Interactive mode instructions
- README content
- Tab completion suggestions

### 5. Update Intro Animation

**Current:**
```javascript
const command = './boot_future.sh';
```

**New:**
```javascript
const command = './future.sh';
```

### 6. Update Command History

**Current:**
```javascript
commandHistory.push('./boot_future.sh');
```

**New:**
```javascript
commandHistory.push('./future.sh');
```

### 7. Update i18n Translations

Update all language strings:

**English:**
```javascript
usageExample: '  ./future.sh --vision "a world where..." --commit "what you will do now"',
// ... update all other references
```

**Japanese:**
```javascript
usageExample: '  ./future.sh --vision "〜な世界" --commit "今すぐやること"',
// ... update all other references
```

### 8. Update Tab Completion

Ensure tab completion works for all variations:
```javascript
const commands = ['cat', 'ls', 'clear', 'pwd', 'whoami', 'help', 'future', './future', 'future.sh', './future.sh'];
```

### 9. Update Process Output

When showing the process has started:
```javascript
// Should work with any variation but display consistently
'Future process started successfully. (PID: 20241225)'
```

## Testing Checklist

- [ ] `future --vision "test" --commit "test"` works
- [ ] `./future --vision "test" --commit "test"` works
- [ ] `future.sh --vision "test" --commit "test"` works
- [ ] `./future.sh --vision "test" --commit "test"` works
- [ ] `future --help` shows correct usage
- [ ] `future --interactive` starts interactive mode
- [ ] Tab completion suggests `future` variants
- [ ] `ls` shows `future.sh` (not boot_future.sh)
- [ ] `cat future.sh` shows permission denied
- [ ] Intro animation types `./future.sh`
- [ ] Error messages show correct command
- [ ] Help text uses `future.sh` examples
- [ ] Japanese translations are updated
- [ ] README.md content is updated

## Migration Notes

For consistency:
1. Always display as `./future.sh` in output (following shell conventions)
2. Accept all variations in input (user-friendly)
3. The file in `ls` should be `future.sh` (with extension)
4. In shared images, show `./future.sh` for educational value

## Example Usage After Change

```bash
$ ls
future.sh   README.md   vision.txt

$ future --vision "a connected world" --commit "learn networking"
# Works! Internally treated as ./future.sh

$ ./future.sh --vision "a connected world" --commit "learn networking"  
# Also works! The "proper" way

$ cat fu[TAB]
$ cat future.sh
cat: future.sh: Permission denied (source is protected)
```