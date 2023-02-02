# Kusi's Knowledgebase

## OnPrem SPFx

### SPFx Introduction Manual

[Introduction](intro.md)

[Development environment](devenv.md)

[Create a new project](createProject.md)

[Project structure](projectStructure.md)

[Migrate SPFx 2016 to 2019](migrate16to19.md)

### gulp serve with localizaion

```powershell
gulp serve --locale de-de
```

### gulp dist for SP2016/19

gulp dist replace the steps:

```powershell
gulp clean
gulp build
gulp bundle --ship
gulp package-solution --ship
```

Install gulp-sequence

```powershell
npm install gulp-sequence --save-dev
```

edit file gulpfile.js

```powershell
'use strict';

if (process.argv.indexOf('dist') !== -1){
    process.argv.push('--ship');
}

const gulp = require('gulp');
const build = require('@microsoft/sp-build-web');
const gulpSequence = require('gulp-sequence');

build.addSuppression(`Warning - [sass] The local CSS class 'ms-Grid' is not camelCase and will not be type-safe.`);

gulp.task('dist', gulpSequence('clean', 'bundle', 'package-solution'));

build.initialize(gulp);
```

### UI Fabric Icons

[https://uifabricicons.azurewebsites.net](https://uifabricicons.azurewebsites.net)
