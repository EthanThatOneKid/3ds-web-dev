---
name: 3ds-web-dev
description: Build web experiences for the Nintendo 3DS browser. Use this skill whenever the user wants to create websites, apps, or interactive experiences that work on the Nintendo 3DS browser, including optimizing for its hardware limitations, outdated WebKit engine, input controls (D-Pad, face buttons, touch), or legacy web standards. Also use for 3DS homebrew web apps, retro mobile web development, or understanding browser constraints of older hardware.
compatibility: Created for Zo Computer
metadata:
  author: etok.zo.computer
---

# 3DS Web Development

The Nintendo 3DS browser is a unique platform with significant constraints. This skill helps you build web experiences that work within its limitations.

## Hardware Constraints

| Model | CPU | RAM |
|-------|-----|-----|
| Old 3DS | 268 MHz ARM11 | 128 MB |
| New 3DS | 804 MHz ARM11 | 256 MB |

Always design for the Old 3DS as baseline — it has ~10x less power than a modern smartphone.

## Browser Limitations

- **Engine**: Older WebKit (~iOS 5 era)
- **JavaScript**: No ES6+, no arrow functions, no Promises, no async/await
- **CSS**: Limited, no flexbox/grid, basic animations only
- **Storage**: LocalStorage is volatile (cleared on browser restart)
- **Media**: Limited codec support for audio/video, no WebGL

## Development Philosophy

### Server-Side Rendering
100% of business logic and HTML generation must happen server-side:
- Use Node.js, PHP, Python, or any server runtime
- No client-side frameworks
- Pre-render everything

### State Persistence
Since LocalStorage is unreliable:
- Use standard `<form>` submissions
- Server-side sessions/cookies for state
- Examples: to-do lists, user preferences, shopping carts

### Form Handling
```html
<form action="/submit" method="POST">
  <input type="text" name="item">
  <button type="submit">Add</button>
</form>
```

### AJAX
Use sparingly — only for small updates. Prefer full page submissions for everything else.

## Input Handling

The 3DS has unique input methods:

### D-Pad and Buttons
```javascript
document.addEventListener('keydown', function(e) {
    var display = document.getElementById('display');
    switch(e.keyCode) {
        case 13: display.innerText = "Button: A"; break;
        case 27: display.innerText = "Button: B (Esc)"; break;
        case 37: display.innerText = "D-Pad: Left"; break;
        case 38: display.innerText = "D-Pad: Up"; break;
        case 39: display.innerText = "D-Pad: Right"; break;
        case 40: display.innerText = "D-Pad: Down"; break;
    }
});
```

### Touch Screen
- Single touch only
- Avoid multi-touch gestures
- Large tap targets (minimum 44px)

## Best Practices

1. **Keep it simple** — Minimal JavaScript, maximum server rendering
2. **Optimize images** — Small file sizes, use appropriate formats
3. **Test on device** — Emulators aren't perfect
4. **Offline support** — Consider what works without internet
5. **Consider the screen** — 400x240 resolution, ~15 fps for animations

## Project Structure

```
my-3ds-site/
├── server.js          # Express/Node server
├── views/
│   ├── index.ejs      # Server-rendered template
│   └── todo.ejs
├── public/
│   ├── style.css
│   └── script.js      # Minimal client JS
└── package.json
```

## Example: Simple To-Do List

**Server (Express):**
```javascript
const express = require('express');
const app = express();
app.use(express.urlencoded({ extended: true }));
app.set('view engine', 'ejs');

let todos = [];

app.get('/', (req, res) => {
  res.render('index', { todos });
});

app.post('/add', (req, res) => {
  todos.push(req.body.item);
  res.redirect('/');
});
```

**Template (EJS):**
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>To-Do List</title>
</head>
<body>
  <h1>To-Do</h1>
  <ul>
    <% todos.forEach(t => { %>
      <li><%= t %></li>
    <% }); %>
  </ul>
  <form action="/add" method="POST">
    <input type="text" name="item" placeholder="New item">
    <button type="submit">Add</button>
  </form>
</body>
</html>
```

## Getting Started

When the user wants to build a 3DS web project:
1. Ask about their target experience (simple page, interactive app, game)
2. Recommend server-side stack based on their preference (Node, PHP, Python)
3. Help structure the project for server-rendered HTML
4. Guide on input handling patterns
5. Test and optimize for 3DS constraints
