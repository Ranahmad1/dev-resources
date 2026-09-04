# VS Code Shortcuts & Tips

## Essential Shortcuts (Windows/Linux)

### Command Palette & Navigation
```
Ctrl + Shift + P     Command Palette (most important!)
Ctrl + P             Quick open file
Ctrl + G             Go to line number
Ctrl + Shift + O     Go to symbol in file
Ctrl + T             Go to symbol in workspace
Ctrl + Shift + E     Explorer panel
Ctrl + Shift + G     Source control (Git)
Ctrl + Shift + X     Extensions
Ctrl + Shift + F     Search across all files
Ctrl + Shift + H     Search & replace across files
Ctrl + B             Toggle sidebar
Ctrl + J             Toggle terminal panel
Ctrl + `             Toggle integrated terminal
Ctrl + K, Z          Zen mode (fullscreen focus)
Ctrl + K, Ctrl + S   View all keybindings
```

### Editing
```
Ctrl + D             Select next occurrence of word
Ctrl + Shift + L     Select ALL occurrences
Ctrl + K, Ctrl + D   Skip + select next occurrence
Alt + Click          Add cursor at position
Ctrl + Alt + ↑/↓     Add cursor above/below
Alt + Shift + I      Add cursor at end of each selected line
Alt + ↑/↓            Move line up/down
Shift + Alt + ↑/↓    Duplicate line up/down
Ctrl + Shift + K     Delete line
Ctrl + Enter         Insert line below
Ctrl + Shift + Enter Insert line above
Ctrl + /             Toggle line comment
Shift + Alt + A      Toggle block comment
Ctrl + Shift + [     Fold code block
Ctrl + Shift + ]     Unfold code block
Ctrl + K, Ctrl + 0   Fold all
Ctrl + K, Ctrl + J   Unfold all
Shift + Alt + F      Format document
Ctrl + K, Ctrl + F   Format selection
Ctrl + Space         Trigger IntelliSense
Ctrl + Shift + Space Trigger parameter hints
F2                   Rename symbol (all occurrences)
F8                   Go to next problem
Ctrl + .             Quick fix / refactor
```

### Selection
```
Ctrl + A             Select all
Shift + Click        Select range
Ctrl + Shift + →     Select word by word
Alt + Shift + →      Expand selection
Alt + Shift + ←      Shrink selection
Ctrl + L             Select current line
```

### Navigation
```
Ctrl + Click         Go to definition
F12                  Go to definition
Alt + F12            Peek definition (inline)
Shift + F12          Find all references
Ctrl + -             Go back
Ctrl + Shift + -     Go forward
Ctrl + Tab           Switch open tabs
Ctrl + 1 / 2 / 3     Focus editor group
Ctrl + \             Split editor
Ctrl + W             Close tab
Ctrl + Shift + T     Reopen closed tab
Ctrl + K, Ctrl + W   Close all tabs
Ctrl + K, P          Copy path of file
Ctrl + K, R          Reveal in Explorer
```

### File Operations
```
Ctrl + N             New file
Ctrl + O             Open file
Ctrl + S             Save
Ctrl + Shift + S     Save as
Ctrl + K, S          Save all
Ctrl + F4            Close file
```

### Find & Replace
```
Ctrl + F             Find in file
Ctrl + H             Replace in file
F3 / Shift + F3      Next / previous match
Alt + Enter          Select all matches
Ctrl + Shift + F     Find in all files
Ctrl + Shift + H     Replace in all files

# In search box:
.*                   Enable regex
Ab                   Match case
\b                   Match whole word
```

### Terminal
```
Ctrl + `             Toggle terminal
Ctrl + Shift + `     New terminal
Ctrl + Shift + 5     Split terminal
Ctrl + K             Clear terminal
Ctrl + C             Kill running process
```

### macOS Differences
```
Cmd instead of Ctrl
Option instead of Alt
Cmd + Shift + P      Command Palette
Cmd + P              Quick open
```

---

## Emmet — Fast HTML Typing

```html
!                    Full HTML5 boilerplate
div.container        <div class="container"></div>
.card.active         <div class="card active"></div>
#header              <div id="header"></div>
p.text>span          <p class="text"><span></span></p>
nav>ul>li*5>a        nav with ul, 5 li, each with a
h1+p+button          h1, p, button as siblings
(header>nav)+(main>section*3)+footer
div{Hello World}     <div>Hello World</div>
img[src=img.jpg alt=Photo]
input[type=email placeholder=Email required]
ul>li.item$*5        li.item1 through li.item5
a{Click me}          <a href="">Click me</a>
table>tr*3>td*4      3 rows, 4 cols each
```

---

## settings.json — Recommended Config

```json
{
  "editor.fontSize": 14,
  "editor.fontFamily": "'Fira Code', 'Cascadia Code', Consolas, monospace",
  "editor.fontLigatures": true,
  "editor.tabSize": 2,
  "editor.formatOnSave": true,
  "editor.formatOnPaste": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.wordWrap": "on",
  "editor.cursorStyle": "line",
  "editor.cursorBlinking": "smooth",
  "editor.minimap.enabled": false,
  "editor.bracketPairColorization.enabled": true,
  "editor.guides.bracketPairs": true,
  "editor.inlineSuggest.enabled": true,
  "editor.suggestSelection": "first",
  "editor.linkedEditing": true,
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 1000,
  "files.trimTrailingWhitespace": true,
  "files.insertFinalNewline": true,
  "terminal.integrated.fontSize": 13,
  "terminal.integrated.cursorStyle": "line",
  "workbench.iconTheme": "material-icon-theme",
  "workbench.colorTheme": "One Dark Pro",
  "workbench.startupEditor": "none",
  "git.confirmSync": false,
  "git.autofetch": true,
  "emmet.triggerExpansionOnTab": true,
  "prettier.singleQuote": true,
  "prettier.semi": true,
  "prettier.trailingComma": "es5"
}
```

---

## Must-Have Extensions

### Essentials
| Extension | Purpose |
|-----------|--------|
| **Prettier** | Code formatter (JS, CSS, HTML, JSON) |
| **ESLint** | JavaScript linting |
| **GitLens** | Git blame, history, branch visualization |
| **Live Server** | Auto-reload on save for HTML/CSS |
| **Auto Rename Tag** | Rename HTML open/close tag pairs |
| **Path IntelliSense** | Autocomplete file paths |

### Productivity
| Extension | Purpose |
|-----------|--------|
| **Thunder Client** | REST API testing (like Postman, in VS Code) |
| **REST Client** | Send HTTP requests from .http files |
| **Error Lens** | Inline error messages |
| **Todo Tree** | Highlight and list TODO comments |
| **Code Spell Checker** | Catch typos in code + comments |
| **Turbo Console Log** | Auto-generate console.log statements |

### Appearance
| Extension | Purpose |
|-----------|--------|
| **One Dark Pro** | Best dark theme |
| **Catppuccin** | Pastel dark theme |
| **Material Icon Theme** | Better file icons |

### Language Support
| Extension | Purpose |
|-----------|--------|
| **PHP Intelephense** | PHP IntelliSense, go to definition |
| **Laravel Blade Snippets** | Blade syntax highlighting |
| **Tailwind CSS IntelliSense** | Tailwind class autocomplete |
| **Volar** | Vue.js support |
| **Python** (Microsoft) | Python IntelliSense & debugging |
| **C/C++** (Microsoft) | C/C++ support |
