# Copy Button Implementation Guide

## Overview
Replace the current "Download Image" button with a "Copy to Clipboard" button that copies the generated future snapshot directly to the user's clipboard.

## Implementation Steps

### 1. Update i18n Translations

Add new translation keys for the copy button:

```javascript
// In i18n object
en: {
    // ... existing keys ...
    copyButton: 'Copy to Clipboard',
    copySuccess: 'Copied!',
    copyFailed: 'Failed to copy. Please try downloading instead.',
    copyTooltip: 'Right-click the image to save it',
    // ... rest of keys
}

ja: {
    // ... existing keys ...
    copyButton: 'クリップボードにコピー',
    copySuccess: 'コピーしました！',
    copyFailed: 'コピーに失敗しました。ダウンロードをお試しください。',
    copyTooltip: '画像を右クリックで保存できます',
    // ... rest of keys
}
```

### 2. Add Copy Function

Add this function after the existing helper functions:

```javascript
// Copy image to clipboard function
async function copyImageToClipboard(canvas) {
    try {
        // Convert canvas to blob
        const blob = await new Promise(resolve => canvas.toBlob(resolve, 'image/png'));
        
        // Check if Clipboard API is available
        if (!navigator.clipboard || !window.ClipboardItem) {
            throw new Error('Clipboard API not supported');
        }
        
        // Write to clipboard
        await navigator.clipboard.write([
            new ClipboardItem({
                'image/png': blob
            })
        ]);
        
        return true;
    } catch (err) {
        console.error('Failed to copy image:', err);
        return false;
    }
}

// Show copy feedback
function showCopyFeedback(button, success) {
    const originalText = button.textContent;
    const originalBg = button.style.backgroundColor;
    
    if (success) {
        button.textContent = `✓ ${t('copySuccess')}`;
        button.style.backgroundColor = '#28a745';
    } else {
        button.textContent = t('copyFailed');
        button.style.backgroundColor = '#dc3545';
    }
    
    // Reset after 2 seconds
    setTimeout(() => {
        button.textContent = originalText;
        button.style.backgroundColor = originalBg;
    }, 2000);
}
```

### 3. Modify generateShareableImage Function

Update the share container HTML generation inside `generateShareableImage`:

```javascript
// Find this section in generateShareableImage function
html2canvas(snapshotContainer, { logging: false, backgroundColor: null }).then(canvas => {
    // ... existing watermark code ...
    
    const imageUrl = canvas.toDataURL('image/png');
    document.body.removeChild(snapshotContainer);

    headerContainer.innerHTML = '';

    const shareContainer = document.createElement('div');
    shareContainer.id = 'share-container';
    
    const shareText = currentLanguage === 'ja' 
        ? `未来を起動しました！\n\nビジョン: ${vision}\nコミット: ${commit}\n\nあなたも未来を起動しよう: ${window.location.href}\n#BootYourFuture`
        : `I've initiated my future!\n\nVision: ${vision}\nCommit: ${commit}\n\nBoot your own future at ${window.location.href}\n#BootYourFuture`;
    const twitterUrl = `https://twitter.com/intent/tweet?text=${encodeURIComponent(shareText)}`;
    const facebookUrl = `https://www.facebook.com/sharer/sharer.php?u=${encodeURIComponent(window.location.href)}`;
    const linkedinUrl = `https://www.linkedin.com/sharing/share-offsite/?url=${encodeURIComponent(window.location.href)}`;

    // NEW: Updated HTML with copy button instead of download
    shareContainer.innerHTML = `
        <p>${t('shareText')}</p>
        <div class="share-buttons">
            <button id="copyButton" class="share-button" title="${t('copyTooltip')}">📋 ${t('copyButton')}</button>
            <a href="${twitterUrl}" target="_blank" class="share-button">Xで共有</a>
            <a href="${facebookUrl}" target="_blank" class="share-button">Facebookで共有</a>
            <a href="${linkedinUrl}" target="_blank" class="share-button">LinkedInで共有</a>
        </div>
        <img src="${imageUrl}" alt="A snapshot of the initiated future in the-future.sh">
    `;
    
    headerContainer.appendChild(shareContainer);
    shareContainer.style.display = 'block';
    
    // NEW: Add copy button event listener
    const copyButton = document.getElementById('copyButton');
    copyButton.addEventListener('click', async () => {
        const success = await copyImageToClipboard(canvas);
        showCopyFeedback(copyButton, success);
    });
    
    window.scrollTo(0, 0);
});
```

### 4. Update CSS (Optional Enhancement)

Add hover state for better feedback:

```css
/* Add to existing CSS */
.share-button:disabled {
    opacity: 0.6;
    cursor: not-allowed;
}

.share-button[title]:hover::after {
    content: attr(title);
    position: absolute;
    bottom: 100%;
    left: 50%;
    transform: translateX(-50%);
    background-color: rgba(0, 0, 0, 0.8);
    color: white;
    padding: 4px 8px;
    border-radius: 4px;
    font-size: 12px;
    white-space: nowrap;
    margin-bottom: 5px;
}

.share-button {
    position: relative; /* For tooltip positioning */
}
```

### 5. Update share-last Command

Ensure the `share-last` command also uses the new copy functionality:

```javascript
// In the share-last command handler
// The generateShareableImage function will automatically handle it
// since we're modifying the core function
```

### 6. Browser Compatibility Fallback

For browsers that don't support the Clipboard API, you might want to show the download link as a fallback:

```javascript
// In generateShareableImage, check for API support
const supportsClipboard = navigator.clipboard && window.ClipboardItem;

if (supportsClipboard) {
    shareContainer.innerHTML = `
        <!-- Copy button version (as shown above) -->
    `;
} else {
    // Fallback to download button
    shareContainer.innerHTML = `
        <p>${t('shareText')}</p>
        <div class="share-buttons">
            <a href="${imageUrl}" download="the-future-sh.png" class="share-button">${t('downloadButton')}</a>
            <a href="${twitterUrl}" target="_blank" class="share-button">Xで共有</a>
            <a href="${facebookUrl}" target="_blank" class="share-button">Facebookで共有</a>
            <a href="${linkedinUrl}" target="_blank" class="share-button">LinkedInで共有</a>
        </div>
        <img src="${imageUrl}" alt="A snapshot of the initiated future in the-future.sh">
    `;
}
```

## Testing Checklist

1. **Basic Functionality**
   - [ ] Click copy button - image copies to clipboard
   - [ ] Paste in various apps (Twitter, Slack, Discord, etc.)
   - [ ] Success feedback shows for 2 seconds
   - [ ] Button returns to original state

2. **Error Handling**
   - [ ] Test on browsers without Clipboard API support
   - [ ] Test when clipboard permissions are denied
   - [ ] Error message displays appropriately
   - [ ] Fallback to download works (if implemented)

3. **Language Support**
   - [ ] Copy button text in English mode
   - [ ] Copy button text in Japanese mode
   - [ ] Success/error messages in both languages
   - [ ] Tooltip text in both languages

4. **Integration**
   - [ ] Works with regular future boot
   - [ ] Works with interactive mode
   - [ ] Works with share-last command
   - [ ] Training mode watermark included when applicable

5. **Browser Testing**
   - [ ] Chrome/Edge (full support expected)
   - [ ] Firefox (full support expected)
   - [ ] Safari (may have limitations)
   - [ ] Mobile browsers (varies by OS/browser)

## Notes

- The Clipboard API requires HTTPS in production
- Some browsers may show a permission prompt on first use
- Safari has limited support for clipboard.write() with images
- Mobile Safari may not support this feature at all
- Consider keeping the image visible so users can long-press to save on mobile

## Migration Considerations

Since users may be accustomed to the download button:
1. The tooltip helps users understand they can still save the image
2. The 📋 emoji makes the function clear
3. Consider adding a first-time hint: "Tip: Image copied! You can paste it anywhere."