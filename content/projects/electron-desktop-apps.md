---
title: "Building Desktop Apps with Electron"
date: 2025-11-18
tags: ["Electron", "desktop", "cross-platform", "Node.js"]
categories: ["Projects"]
summary: "Creating cross-platform desktop applications using Electron with Next.js static export."
---

## What is Electron?

Electron is a framework for building **cross-platform desktop apps** using **web technologies** (HTML, CSS, JavaScript). Developed by GitHub, notable apps like VS Code, Discord, and Slack use Electron.

## Core Concepts

### Process Model

Electron apps have two types of processes:

```
+------------------------------------------+
|         Main Process                      |
|   - Only one                             |
|   - Full Node.js API access              |
|   - Creates and manages windows          |
|   - Handles system operations            |
+------------------------------------------+
         | Creates
         v
+------------------------------------------+
|       Renderer Process                    |
|   - One per window                       |
|   - Runs web pages (HTML/CSS/JS)         |
|   - No direct Node.js access by default  |
|   - Communicates via preload script      |
+------------------------------------------+
```

### Main Process Code

```javascript
// electron/main.js
const { app, BrowserWindow } = require('electron');
const path = require('path');

function createWindow() {
  const win = new BrowserWindow({
    width: 1280,
    height: 800,
    webPreferences: {
      nodeIntegration: false,        // Disabled for security
      contextIsolation: true,        // Enabled for security
      preload: path.join(__dirname, 'preload.js')
    }
  });

  // Load content
  win.loadURL('http://localhost:3000');  // Dev mode
  // win.loadFile('out/index.html');     // Production
}

app.whenReady().then(createWindow);

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit();
  }
});
```

### Preload Script

The preload script runs before renderer loads, safely exposing Node.js features:

```javascript
// electron/preload.js
const { contextBridge, ipcRenderer } = require('electron');

contextBridge.exposeInMainWorld('electronAPI', {
  sendMessage: (msg) => ipcRenderer.send('message', msg),
  onReply: (callback) => ipcRenderer.on('reply', callback)
});
```

## Special Implementation: Local HTTP Server

Since Next.js static export doesn't include API routes, we implement a local HTTP server in Electron's main process:

```javascript
// electron/main.js (simplified)
const http = require('http');
const fetch = require('node-fetch');

function createServer() {
  const server = http.createServer(async (req, res) => {
    // API requests
    if (req.url === '/api/chat' && req.method === 'POST') {
      let body = '';
      req.on('data', chunk => body += chunk);
      req.on('end', async () => {
        // Forward to AI API
        const response = await fetch('https://api.deepseek.com/v1/chat/completions', {
          method: 'POST',
          headers: {
            'Authorization': `Bearer ${API_KEY}`,
            'Content-Type': 'application/json'
          },
          body: body
        });
        const data = await response.json();
        res.end(JSON.stringify(data));
      });
      return;
    }
    
    // Static files...
  });
  
  server.listen(23456);
}
```

## Packaging Configuration

### electron-builder

```json
// package.json
{
  "build": {
    "appId": "com.example.myapp",
    "productName": "My App",
    "directories": {
      "output": "dist-electron"
    },
    "files": [
      "electron/**/*",
      "out/**/*",
      "public/**/*"
    ],
    "win": {
      "target": "nsis",
      "icon": "public/icon.png"
    },
    "mac": {
      "target": "dmg",
      "icon": "public/icon.png"
    }
  }
}
```

### Build Commands

```bash
# Development mode
npm run electron:dev

# Package for Windows
npm run electron:build:win

# Package for Mac
npm run electron:build:mac
```

## Output Files

```
dist-electron/
├── win-unpacked/          # Windows portable
│   ├── MyApp.exe          # Executable
│   ├── resources/         # Resources
│   └── ...
└── MyApp Setup.exe        # Windows installer
```

## Common Issues

### Package fails?

Common causes:
1. **Network issues**: Electron binary download failed
   - Solution: Use mirror `ELECTRON_MIRROR=https://npmmirror.com/mirrors/electron/`
2. **Permission issues**: Creating symlinks requires admin on Windows
   - Solution: Run as admin, or disable code signing `"sign": null`

### White screen on launch?

Possible causes:
1. Static file paths incorrect
2. Local server not started
3. Check dev tools (F12) console for errors

## Learning Resources

- [Electron Official Docs](https://www.electronjs.org/docs)
- [electron-builder Docs](https://www.electron.build/)

---

**Last Updated**: 2025-11-18
