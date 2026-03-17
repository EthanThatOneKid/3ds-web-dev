# 3DS Web Dev

Introduction

Target Audience: Modern web developers.

Goal: Understanding the limitations and unique properties of the 3DS browser.

## Hardware Overview

- CPU speed
- Limited RAM (Old 3DS vs. New 3DS)

## Browser Limitations

### Rendering Engine

Older WebKit version.

### Feature Support

- Outdated standards
- No modern JavaScript (ES6+)
- Poor performance with heavy scripts

### Media Restrictions

- Limited audio and video codec support
- Specific constraints for 3D imagery

## Development Philosophy

### Dynamic via the Server

100% of business logic and HTML generation should happen on the server (Node.js, PHP, Python).

### Form Persistence

Since LocalStorage is volatile on 3DS, use standard `<form>` submissions and server-side sessions/cookies for state (e.g., a to-do list).

### AJAX vs. Forms

Use AJAX sparingly for small updates; use full page submissions for everything else.

### Input Handling

The 3DS has a limited keyboard. Consider these key codes for D-Pad and button input:

```javascript
document.addEventListener("keydown", function(e) {
    var display = document.getElementById("display");
    switch (e.keyCode) {
        case 13: display.innerText = "Button: A (Enter)"; break;
        case 8: display.innerText = "Button: B (Esc)"; break;
        case 37: display.innerText = "D-Pad: Left"; break;
        case 38: display.innerText = "D-Pad: Up"; break;
        case 39: display.innerText = "D-Pad: Right"; break;
        case 40: display.innerText = "D-Pad: Down"; break;
        default: display.innerText = "Key: " + e.keyCode;
    }
});
```

This structure should provide a complete foundation for building both simple and interactive applications for the 3DS browser.
