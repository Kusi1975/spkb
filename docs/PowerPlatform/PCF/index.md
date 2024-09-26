# Kusi's Knowledge Base

## PowerApps Component Framework

### Learning

[PCF Learning Roeadmap](https://dianabirkelbach.wordpress.com/2024/04/03/pcf-learning-roadmap/)
[PCF Implementing Controls](https://learn.microsoft.com/en-us/power-apps/developer/component-framework/implementing-controls-using-typescript?tabs=before)

### Prerequisits

Install [Visual Studio Code](https://code.visualstudio.com/download)

Download and Install the Setup from [Github](https://github.com/coreybutler/nvm-windows/releases).

Install the latest NodeJS
```powershell
nvm list available
nvm install 20.17.0
nvm use 20.17.0
```

Install [PowerShell 7.x](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows)

Start Visual Studio Code and install Extension: **Power Platform Tools**

### Create Project

Available Templates: **field** oder **dataset**

Open Terminal in Visual Studio Code

Long Form:
```powershell
pac pcf init --namespace SampleNamespace --name LinearInputControl --template field --run-npm-install --framework react
```

Short Form:
```powershell
pac pcf init -ns SampleNamespace -n CanvasGrid -t dataset -npm -fw react
```

Add usefull packages

```powershell
npm install office-ui-fabric-react
npm install --save react-copy-to-clipboard @types/react-copy-to-clipboard
```

### Debug

```powershell
npm start watch
```

### Build

```powershell
npm run build -- --buildMode production
```
