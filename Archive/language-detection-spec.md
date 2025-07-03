# Language Detection and Switching Specification

## Overview
Implement automatic language detection at page load to ensure the entire experience is in the user's preferred language from the very beginning, including the initial error message.

## Language Detection Priority

The system should check for the user's preferred language in the following order:

1. **URL Parameter** (Highest priority)
   - Check for `?lang=ja` or `?lang=en` in the URL
   - Allows sharing links in a specific language
   - Example: `https://the-future.sh/?lang=ja`

2. **LocalStorage** (User's previous choice)
   - Check for `preferredLanguage` in localStorage
   - Persists user's language preference across sessions
   - Updates whenever language is detected/changed

3. **Browser Language** (System default)
   - Use `navigator.language` or `navigator.languages[0]`
   - Check if it starts with 'ja' for Japanese
   - Examples: 'ja', 'ja-JP' → Japanese; 'en-US', 'en' → English

4. **Default** (Fallback)
   - Default to English if none of the above apply

## Implementation Details

### 1. Language Detection Function
```javascript
function detectUserLanguage() {
    // 1. Check URL parameter
    const urlParams = new URLSearchParams(window.location.search);
    const urlLang = urlParams.get('lang');
    if (urlLang === 'ja' || urlLang === 'en') {
        return urlLang;
    }
    
    // 2. Check localStorage
    const storedLang = localStorage.getItem('preferredLanguage');
    if (storedLang === 'ja' || storedLang === 'en') {
        return storedLang;
    }
    
    // 3. Check browser language
    const browserLang = navigator.language || navigator.languages[0];
    if (browserLang.startsWith('ja')) {
        return 'ja';
    }
    
    // 4. Default to English
    return 'en';
}
```

### 2. Initialization Update
```javascript
// At the very beginning of the script
let currentLanguage = detectUserLanguage();

// Save to localStorage for next time
localStorage.setItem('preferredLanguage', currentLanguage);

// Apply language immediately
document.documentElement.lang = currentLanguage;
```

### 3. Update Intro Animation
The intro animation should use the detected language:

```javascript
function runIntroAnimation() {
    // ... existing code ...
    
    // When showing the error, use the current language
    runErrorSequence(() => {
        createInputLine();
    });
}
```

### 4. Language Switching Command (Optional Enhancement)
Add a command to manually switch languages:

```javascript
// Add to command processing
if (cmd === 'lang') {
    if (parts[1] === 'ja' || parts[1] === 'en') {
        currentLanguage = parts[1];
        localStorage.setItem('preferredLanguage', currentLanguage);
        document.documentElement.lang = currentLanguage;
        addLine(`Language switched to ${parts[1] === 'ja' ? 'Japanese' : 'English'}`);
        addLine('Reload the page to see the full effect.');
    } else {
        addLine('Usage: lang [ja|en]');
    }
    createInputLine();
    return;
}
```

### 5. Help Command Update
Update the help command to be language-aware from the start:

```javascript
// The help command already uses t() function, so it will work automatically
// Just ensure currentLanguage is set before any command is processed
```

## User Experience Flow

### First-time Visitor (Japanese browser)
1. Browser language detected as 'ja'
2. Initial prompt shows in Japanese
3. Error message appears in Japanese
4. All commands respond in Japanese
5. Language preference saved for next visit

### Returning Visitor
1. localStorage contains 'ja' from previous visit
2. Interface immediately shows in Japanese
3. No need to re-detect language

### Shared Link
1. Friend shares `https://the-future.sh/?lang=ja`
2. Recipient sees Japanese interface regardless of their browser settings
3. If they interact, their preference is saved

## Edge Cases

### 1. Mixed Language Input
Current behavior (detecting language from vision/commit) should remain as a fallback, but the initial UI language should be set by the detection logic above.

### 2. Unsupported Languages
If browser language is neither English nor Japanese (e.g., 'fr', 'es'), default to English.

### 3. Language Switch Mid-Session
If user inputs Japanese text on an English interface:
- Keep the current behavior of switching for the success sequence
- Optionally, prompt user: "Switch interface to Japanese? (y/n)"
- Save their preference

## Implementation Checklist

- [ ] Add `detectUserLanguage()` function
- [ ] Call detection before any UI rendering
- [ ] Update `document.documentElement.lang` attribute
- [ ] Save language preference to localStorage
- [ ] Test URL parameter functionality
- [ ] Test browser language detection
- [ ] Test localStorage persistence
- [ ] Add `lang` command (optional)
- [ ] Update documentation about language support

## Benefits

1. **Immediate Recognition**: Japanese users see Japanese text instantly
2. **Shareability**: Can share links that force a specific language
3. **Persistence**: Language choice remembered across sessions
4. **Flexibility**: Users can override automatic detection
5. **True Internationalization**: Not just translated output, but fully localized experience