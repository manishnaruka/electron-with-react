## Electron with React

### steps

1. run these commands on terminal :

   1. npx create-react-app -your app name here-
   2. npm install --save-dev electron electron-builder wait-on concurrently
   3. npm install --save electron-is-dev

2. create electron.js file inside ./public/ folder and paste the code given bellow

```
const electron = require('electron');
const app = electron.app;
const BrowserWindow = electron.BrowserWindow;

const path = require('path');
const url = require('url');
const isDev = require('electron-is-dev');

let mainWindow;

function createWindow() {
  mainWindow = new BrowserWindow({width: 900, height: 680});
  mainWindow.loadURL(isDev ? 'http://localhost:3000' : `file://${path.join(__dirname, '../build/index.html')}`);
  mainWindow.on('closed', () => mainWindow = null);
}

app.on('ready', createWindow);

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit();
  }
});

app.on('activate', () => {
  if (mainWindow === null) {
    createWindow();
  }
});

```

3. add "main": "public/electron.js" entry to package.json .
4. add "electron-dev": "concurrently \"BROWSER=none npm start\" \"wait-on http://localhost:3000 && electron .\"" script to package.json
5. add "build": {
   "appId": "com.example.electron-cra",
   "files": [
   "build/**/*",
   "node_modules/**/*"
   ],
   "directories":{
   "buildResources": "assets"
   }
   } to package.json
6. add following scripts to pachage.json

   1. "electron-pack": "build --em.main=build/electron.js"
   2. "preelectron-pack": "npm run-script build",
   3. "package-mac": "npm build && electron-builder build --mac",
   4. "package-linux": "npm build && electron-builder build --linux",
   5. "package-win": "npm build && electron-builder build --win --x64"

7. add "homepage": "./" entry to package.json

Now for development you can simply run "npm run electron-dev"
To package it first run "npm run electron-pack" then "npm run package-mac/package-win/package-linux" (accourding to targeting OS)
