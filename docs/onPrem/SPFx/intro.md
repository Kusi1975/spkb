# Kusi's SPFx Knowledgebase

## Introduction

In the past, if you wanted to build custom reusable WebParts, you had to write server code that had to be installed server-side directly on the server. Since SharePoint 2016 with Feature Pack 2, there is a possibility to package a Clientside WebPart that can be stored in the App Catalog Site Collection. If you have the appropriate rights, the package can be installed without having to connect directly to the server, and the package can also be replaced without provoking an app pool reset, meaning there is no down time. In this guide, I would like to offer you a "step-by-step" guide that will help you to create an SPFx package for SP2016, SP2019 or SharePoint Online.

## Versions

SPFx Version 1.1.0 is supplied with SharePoint Feature Pack 2 (corresponds to CU 09.17). SPFx 1.4.1 is included with SharePoint 2019/SE. SharePoint Online currently uses 1.16.1

||SP2016|SP2019/SE|SPOnline|
|---|---|---|---|
|SPFx|1.1.0|1.4.1|1.16.1|
|Node.js|6.x/8.x|6.x/8.x|16.13+|
|NPM|v3/v4|v3/v4|v5/v6/v7/v8|
|TypeScript|2.2.2|2.4.2|3.3|
|React|15.4.2|15.6.2|17.0.1|
|Yeoman|3.x|3.x|4.x|
|Gulp|4.x|4.x|4.x|
|@microsoft/generator-sharepoint|1.16.1|1.16.1|1.16.1|

### Navigation

[Introduction](intro.md)

[Development environment](devenv.md)

[Create a new project](createProject.md)

[Project structure](projectStructure.md)

[Migrate SPFx 2016 to 2019](migrate16to19.md)
