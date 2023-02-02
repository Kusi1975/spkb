# Kusi's SPFx Knowledgebase

## Introduction

> [Introduction](intro.md)

## Development environment

The development itself has no longer to be developed directly on the server as it used to be, but can also be done on a client.

## Node.js

Node.js must be installed on the development environment. The setup for the respective version can be downloaded from [https://nodejs.org/dist](https://nodejs.org/dist).

## NuGet Manager

The NuGet Manager can be installed using Powershell:

```powershell
Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
```

## Yeoman, Gulp und Microsoft SharePoint Generator

Then the missing NPM packages can be installed using PowerShell:

```powershell
npm install -g yo@3.1.1 gulp@4.0.2 @microsoft/generator-sharepoint@1.12.1
```

## Visual Studio Code

You need Visual Studio Code for the development:

[https://code.visualstudio.com/download](https://code.visualstudio.com/download)

## Create a new project

> [Create a new project](createProject.md)

## Project structure

> [Project structure](projectStructure.md)

## Migrate SPFx 2016 to 2019

> [Migrate SPFx 2016 to 2019](migrate16to19.md)
