# Intro Animation Specification

## Overview
When users access the-future.sh, they should immediately see an automated demonstration of someone attempting to boot their future and failing, which teaches them what they need to do.

## Animation Sequence

### 1. Initial State (0ms)
- Show only the prompt: `$ ` with blinking cursor
- Terminal is otherwise empty
- No user interaction possible yet

### 2. Auto-typing Phase (500ms - 2500ms)
Using Typed.js or similar animation:

```
Timeline:
- 500ms: Wait (build anticipation)
- 500-1100ms: Type "./boot_" (50ms per character)
- 1100-1400ms: Type "future.sh" (30ms per character, slightly faster)
- 1400-1900ms: Cursor blinks (show hesitation)
- 1900ms: Simulate Enter key press
```

Visual details:
- Cursor should be visible and move with typing
- Each character appears one at a time
- The typing speed variation (slower for "./boot_", faster for "future.sh") creates a natural feeling

### 3. Enter Key Animation (1900ms - 2000ms)
- The typed command moves from input state to executed state
- The line becomes: `$ ./boot_future.sh` (no longer editable)
- Brief flash or subtle animation to indicate execution

### 4. Error Sequence Display (2000ms - 3000ms)
Display error messages with staggered timing:

```
2000ms: [FATAL] Core components missing. Future cannot be initialized.
2100ms:         Required flags: --vision, --commit
2200ms: (blank line)
2300ms: USAGE:
2400ms:   ./boot_future.sh --vision "a world where..." --commit "what you will do now"
2500ms: (blank line)
2600ms: # Tip: A vision without a commit is just a dream.
```

Each line should appear instantly (not character by character) but with timing delays between lines.

### 5. Ready State (3000ms+)
- Show fresh prompt: `$ ` with blinking cursor
- User can now type their own commands
- System is fully interactive

## Technical Implementation Notes

### Using Typed.js
```javascript
const typed = new Typed('#typing-element', {
  strings: ['./boot_future.sh'],
  typeSpeed: 40,
  startDelay: 500,
  showCursor: true,
  cursorChar: '█',
  onComplete: function() {
    // Trigger enter key animation and error sequence
  }
});
```

### Custom Implementation Alternative
If not using Typed.js, implement character-by-character insertion:
```javascript
const command = './boot_future.sh';
let index = 0;

function typeCharacter() {
  if (index < command.length) {
    element.textContent += command[index];
    index++;
    setTimeout(typeCharacter, index < 7 ? 50 : 30); // Vary speed
  } else {
    setTimeout(simulateEnter, 500);
  }
}
```

## Styling Considerations

### Cursor Animation
```css
.cursor {
  animation: blink 1s infinite;
}

@keyframes blink {
  0%, 49% { opacity: 1; }
  50%, 100% { opacity: 0; }
}
```

### Command Execution Transition
When Enter is pressed, consider:
- Slight fade of the cursor
- Brief highlight of the entire command line
- Smooth transition from "input" to "executed" state

## User Experience Goals

1. **Immediate Understanding**: Users instantly see what they're supposed to do
2. **Zero Friction**: No confusion about where to start typing
3. **Emotional Connection**: Seeing the error first normalizes failure
4. **Anticipation**: The automated sequence builds curiosity about success

## Important Notes

- This animation plays **every time** the page loads (no localStorage checks)
- During the animation, user input should be disabled to prevent confusion
- The animation should feel deliberate but not slow (total time: ~3 seconds)
- Ensure mobile users also get the full experience (no reduced motion)

## Accessibility Considerations

While the animation enhances the experience, ensure:
- Screen readers announce the final state clearly
- Users can press ESC to skip to the ready state (optional)
- The error message remains visible for reference