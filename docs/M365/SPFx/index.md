# Kusi's Knowledge Base

## M365 SPFx

[M365 Community](https://aka.ms/community/home)

[SPFx Community Call](https://aka.ms/spfx-call)

[SPFx WebParts Sample Gallery](https://aka.ms/spfx-webparts)

[SPFx Extensions Sample Gallery](https://aka.ms/spfx-extensions)

[SPFx for Teams Sample Gallery](https://aka.ms/teams-samples)

[Teams Extensions Sample Gallery](https://aka.ms/teams-extensions)

### NVM

In the M365 World the NodeJS version change ofen than in onPrem. To use more than one NodeJS Version you can use NVM.

Before you Install uninstall all local installed NodeJS Versions.

Download and Install the Setup from [Github](https://github.com/coreybutler/nvm-windows/releases).

To use NVM start PowerShell as Administrator.

```powershell
# Return current used NodeJS
nvm current

# Return all installed NodeJS
nvm list

# Return all available NodeJS
nvm list available

# Switch NodeJS
nvm use xx.xx.xx
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