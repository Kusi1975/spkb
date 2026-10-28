# Kusi's SPFx Knowledgebase

## Introduction

In the past, if you wanted to build custom reusable WebParts, you had to write server code that had to be installed server-side directly on the server. Since SharePoint 2016 with Feature Pack 2, there is a possibility to package a Clientside WebPart that can be stored in the App Catalog Site Collection. If you have the appropriate rights, the package can be installed without having to connect directly to the server, and the package can also be replaced without provoking an app pool reset, meaning there is no down time. In this guide, I would like to offer you a "step-by-step" guide that will help you to create an SPFx package for SP2016, SP2019 or SharePoint Online.

## Versions

SPFx Version 1.1.0 is supplied with SharePoint Feature Pack 2 (corresponds to CU 09.17). SPFx 1.4.1 is included with SharePoint 2019/SE. SharePoint Online currently uses 1.23.2

||SP2019 / SPSE|SPOnline|
|---|---|---|
|SPFx|![version](https://img.shields.io/badge/version-1.4.1-green.svg)|![version](https://img.shields.io/badge/version-1.24.0-green.svg)|
|Node.js|8.x|22.x|
|NPM|5.x/6.x|10.x|
|TypeScript|2.4.2|5.8.0|
|React|15.6.2|17.0.1|
|Yeoman|3.1.1|7.x|
|Toolchain|Gulp 4.0.2|Heft 1.x|
|\@microsoft/generator-sharepoint|1.12.1|1.23.2|
