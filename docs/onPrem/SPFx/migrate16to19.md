# Kusi's SPFx Knowledgebase

## Introduction

> [Introduction](intro.md)

## Development environment

> [Development environment](devenv.md)

## Create a new project

> [Create a new project](createProject.md)

## Project structure

> [Project structure](projectStructure.md)

## Migrate SPFx 2016 to 2019

To migrate an SPFx package from 2016 to 2019, adjustments to certain files are necessary, after which only the package should be available instead of the old packages with the necessary JS files.

### File Customizations

Make the red text changes to the 2016 solution and rebuild.

#### config/config.json

<pre><code>{
  <span style='color:red'>"$schema": "https://developer.microsoft.com/json-schemas/spfx-build/config.2.0.schema.json",
  "version": "2.0",
  "bundles": {
    "hello-world-web-part": {
      <s>"entries"</s>"components": </span>[
        {
          "entry<span style='color:red'>point</span>": "./lib/webparts/helloWorld/HelloWorldWebPart.js",
          "manifest": "./src/webparts/helloWorld/HelloWorldWebPart.manifest.json"<span style='color:red'><s>,
          "outputPath": "./dist/hello-world-web-part.js"</s></span>
        }
      ]
    <span style='color:red'>}
  },</span>
  "externals": {},
  "localizedResources": {
    "HelloWorldWebPartStrings": "<span style='color:red'>lib/</span>webparts/helloWorld/loc/{locale}.js"
...</code></pre>

#### config/package-solution.json

<pre><code>...
"version": "1.0.0.0",
<span style='color:red'>"includeClientSideAssets": true</span>
...</code></pre>

#### config/write-manifest.json

<pre><code>...
"cdnBasePath": <span style='color:red'>"&lt;!-- PATH TO CDN --&gt;"</span>
...</code></pre>

#### .yo-rc.json

<pre><code>...
"environment": "onprem<span style='color:red'>19</span>"
...</code></pre>

#### package.json

<pre><code>...
"dependencies": {
    "react": "<span style='color:red'>15.6.2</span>",
    "react-dom": "<span style='color:red'>15.6.2</span>",
    "@types/react": "<span style='color:red'>15.6.6</span>",
    "@types/react-dom": "<span style='color:red'>15.5.6</span>",
    <span style='color:red'><s>"@types/react-addons-shallow-compare": "0.14.17",</s></span>
    <span style='color:red'><s>"@types/react-addons-update": "0.14.14",</s></span>
    <span style='color:red'><s>"@types/react-addons-test-utils": "0.14.15",</s></span>
    "@microsoft/sp-core-library": "<span style='color:red'>~1.4.0</span>",
    "@microsoft/sp-webpart-base": "<span style='color:red'>~1.4.0</span>",
    "@microsoft/sp-lodash-subset": "<span style='color:red'>~1.4.0</span>",
    "@microsoft/sp-office-ui-fabric-core": "<span style='color:red'>~1.4.0</span>",
    ...
  },
  "resolutions": {
    "@types/react": "<span style='color:red'>15.6.6</span>"
  },
  "devDependencies": {
    "@microsoft/sp-build-web": "<span style='color:red'>~1.4.1</span>",
    "@microsoft/sp-module-interfaces": "<span style='color:red'>~1.4.1</span>",
    "@microsoft/sp-webpart-workbench": "<span style='color:red'>~1.4.1</span>",
...</code></pre>

#### tsconfig.json

<pre><code>...
    "module": <span style='color:red'><s>"commonjs"</s>"esnext",
    "moduleResolution": "node",</span>
...
    "typeRoots": [
      "./node_modules/@types"<span style='color:red'>,
      "./node_modules/@microsoft"</span>
    ],
...</code></pre>

#### gulpfile.js

<pre><code>...
const gulpSequence = require('gulp-sequence');
<span style='color:red'>const del = require('del');

gulp.task('cleanup', function(){
     return del(['dist/**','lib/**','release/**','sharepoint/**','temp/**'], {force:true});
});</span>
...
gulp.task('dist', gulpSequence('clean', <span style='color:red'>'cleanup',</span> 'bundle', 'package-solution'));
...</code></pre>

#### README.md

<pre><code>## sp-20<span style='color:red'>19</span>
...</code></pre>
