# Kusi's Knowledge Base

## M365 SPFx

[M365 Community](https://aka.ms/community/home)

[SPFx Community Call](https://aka.ms/spfx-call)

[SPFx WebParts Sample Gallery](https://aka.ms/spfx-webparts)

[SPFx Extensions Sample Gallery](https://aka.ms/spfx-extensions)

[SPFx for Teams Sample Gallery](https://aka.ms/teams-samples)

[Teams Extensions Sample Gallery](https://aka.ms/teams-extensions)

### fnm

In the M365 World the NodeJS version change ofen than in onPrem. To use more than one NodeJS Version you can use fnm.

In the past I used NVM. Now NVM is only being developed further for Linux.

Before you Install uninstall all local installed NodeJS Versions.

Download and Install the Setup from [Github](https://github.com/Schniz/fnm/releases).

For fnm it is not strictly necessary to run PowerShell as an administrator.

```powershell
# Return current used NodeJS
fnm current

# Return all installed NodeJS
fnm list

# Return all available NodeJS
fnm list-remote

# Switch NodeJS
fnm use xx.xx.xx
```
fnm switch automatically the NodeJS Version if in the folder a file **.node-version** as a text file with only the NodeJS version.

Example .node-version:

```
22.22.1
```

### PNPM

NPM download each file from each packages into the node_modules folder from each project, this need much disk space. To save space you can use PNPM.
PNPM have a central folder and store each package in this folder and create only a hard link per file.

If you use fnm or NVM use for global packages NPM, then PNPM store for each NodeJS Version in the same place and override global packages.

Installation from PNPM:

```powershell
npm install pnpm@12.11.2 --global
```

Use in project folder:

```powershell
pnpm install
```

### Show all Theme colors

```javascript
    var palette = window.__themeState__.theme;
    var containerElement = document.createElement("div");
    containerElement.style.padding = "1em";
    containerElement.style.fontFamily = "sans-serif";
    containerElement.style.columnCount = "3";
    document.body.appendChild(containerElement);
    var arr = Object.keys(palette).map(k => {
      var colorElement = document.createElement("div");
      colorElement.style.marginTop = "1ex";
      var nameElement = document.createElement("span");
      nameElement.style.display = "inline-block";
      nameElement.style.minWidth = "150px";
      nameElement.innerHTML = k
      colorElement.appendChild(nameElement);
      var squareElement = document.createElement("span");
      squareElement.style.display = "inline-block";
      squareElement.style.border = "solid 1px #888";
      squareElement.style.width = "12px";
      squareElement.style.height = "12px";
      squareElement.style.margin = "0 2px 0 1ex";
      squareElement.style.backgroundColor = window.__themeState__.theme[k];
      colorElement.appendChild(squareElement);
      var hexElement = document.createElement("span");
      hexElement.innerHTML = window.__themeState__.theme[k];
      colorElement.appendChild(hexElement);
      containerElement.appendChild(colorElement);
    });
```

### Use Theme color in inline style

In WebPart definition:

```javascript
import { IReadonlyTheme } from '@microsoft/sp-component-base';

export default class <WebPartName>WebPart {
  private _themeColors: IReadonlyTheme = undefined;
  ...
}

protected onThemeChanged(currentTheme: IReadonlyTheme | undefined): void {
    if (!currentTheme) {
      return;
    }

    this._themeColors = currentTheme;
  }
```

In WebPart props definition:

```javascript
import { IReadonlyTheme } from '@microsoft/sp-component-base';

export interface I<WebPart>Props {
  themeColors: IReadonlyTheme;
  ...
}
```

In WebPart render function:

```javascript
 {this.props.themeColors ? (Object.keys(this.props.themeColors.semanticColors) as
          (keyof typeof this.props.themeColors.semanticColors)[]).map(k =>
            <div key={k}><div style={{
              display: 'inline-block',
              backgroundColor: `${this.props.themeColors.semanticColors[k]}`,
              width: '30px', marginRight: '10px'
            }}>&nbsp;</div>
              <div style={{ display: 'inline-block' }}>{this.props.themeColors.semanticColors[k]} - {k}</div>
            </div>) : ''}
```

### Use Theme color in scss

```css
background-color: "[theme: themeLighterAlt, default: #0078d7]";
```

### Migrate from gulp to heft

#### Use update command from Microsoft

You can update with the update tool from Microsoft but in my case I was not so happy.

```powershell
fnm use 22.22.1
pnpm install -g @pnp/cli-microsoft365
m365 spfx project upgrade --toVersion 1.22.2
```

#### Use manual update with update the files

The best is to create a new empty project with SPFx 1.22.2 or later and compare it, following files are changed:

Delete files

- gulpfile.js
- package-lock.json

Copy from new empty project (override if exists):

- Config\rig.json kopieren
- Config\typescript.json kopieren
- Config\sass.json kopieren
- .eslintrc.js
- tsconfig.json
- .gitignore

Compare with the empty project and edit it:

- .node-version

- package.json
  - delete main
  - edit scripts, dependencies, devDependencies, overrides
  - new resolutions

- .yo-rc.json
  - useGulp
  - nodeVersion
  - sdksVersions
  - edit version

- readme.md 
  - edit version
  - replace gulp with npm (heft use npm)
  
- config/package-solution.json
  - mpnId
	
This may change, but is currently identical:

- config/serve.json
- config/config.json

```powershell	
rd .\node_modules\ -erroraction silentlycontinue
pnpm install
```
