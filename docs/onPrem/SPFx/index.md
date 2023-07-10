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

### Get ListItem by ID

```typescript
this.props.spHttpClient.get(
    encodeURI(`${this.props.siteurl}/_api/web/lists/getbytitle('${listName}')/items(${itemId})?$select=Id,Title`),
    SPHttpClient.configurations.v1)
    .then(res => res.json())
    .then(res => {
        ...
    });
```

### Get ListItem by filter

```typescript
this.props.spHttpClient.get(
    encodeURI(`${this.props.siteurl}/_api/web/lists/getbytitle('${listName}')/items(${itemId})?$filter=Title eq '...'&$select=Id,Title`),
    SPHttpClient.configurations.v1)
    .then(res => res.json())
    .then(res => {
        ...
    });
```

### CAML Query

```typescript
this._webPartContext.spHttpClient.post(
`${this.props.siteUrl}/_api/web/lists/GetByTitle('${this.listName}')/GetItems`,
SPHttpClient.configurations.v1,
{
    headers: {
        'odata-version': '3.0',
        'content-type': 'application/json;odata=verbose',
        'accept': 'application/json;odata=verbose'
    },
    body: JSON.stringify(
    {
        'query': {
            '__metadata': { 'type': 'SP.CamlQuery' },
            'ViewXml': `<View><Query><Where>${viewXml}</Where><OrderBy><FieldRef Name='Title' Ascending='False' /></OrderBy></Query></View>`
        }
    }),
    method: 'POST'
})
.then(res => res.json())
.then(res => {
    ...
});;
```

### Create ListItem

```typescript
this.props.spHttpClient.post(
    `${this.props.siteurl}/_api/web/lists/GetByTitle('${listName}')/items`,
    SPHttpClient.configurations.v1, {
    headers: {
        'odata-version': '3.0',
        'Accept': 'application/json;odata=verbose',
        'Content-Type': 'application/json;odata=verbose'
    },
    body: JSON.stringify(
    {
        '__metadata': {
            'type': `SP.Data.${listName}ListItem`
        },
        'Title': ...
    }
    ),
    method: 'POST'
    }).then(r => {
        ...
    });
```

### Update ListItem

```typescript
this.props.spHttpClient.post(
    `${this.props.siteurl}/_api/web/lists/GetByTitle('${listName}')/items(${itemId})`,
    SPHttpClient.configurations.v1, {
        headers: {
            'odata-version': '3.0',
            'Accept': 'application/json;odata=verbose',
            'Content-Type': 'application/json;odata=verbose',
            'IF-MATCH': '*',
            'X-HTTP-Method': 'MERGE'
        },
        body: JSON.stringify(
        {
            '__metadata': {
                'type': `SP.Data.${listName}ListItem`
            },
            'Title': ...
        }
        ),
        method: 'POST'
        }).then(res => {
            ...
    });
```

### Delete ListItem

```typescript
this.props.spHttpClient.post(
    `${this.props.siteurl}/_api/web/lists/GetByTitle('${listName}')/items(${itemId})`,
    SPHttpClient.configurations.v1, {
    headers: {
        'odata-version': '3.0',
        'Accept': 'application/json;odata=nometadata',
        'Content-Type': 'application/json;odata=verbose',
        'IF-MATCH': '*',
        'X-HTTP-Method': 'DELETE'
    },
    method: 'POST'
    }).then(r => {
    ...
    });
```

### Send Mail

```typescript
this._webPartContext.spHttpClient.get(
    this._webAbsoluteUrl + '/_api/web/currentuser',
    SPHttpClient.configurations.v1
)
.then(res => res.json())
.then(currentuser => {
    this._webPartContext.spHttpClient.post(
    this._webAbsoluteUrl + "/_api/SP.Utilities.Utility.SendEmail",
    SPHttpClient.configurations.v1,
    {
        headers: {
            'content-type': 'application/json;odata=verbose',
            'accept': 'application/json;odata=verbose'
        },
        body: JSON.stringify({
        'properties': {
            'To': [eMail],
            'CC': currentuser.Email,
            'Subject': '...',
            'Body': '<div>' +
            ...
            '</div>'
        }
        }),
        method: 'POST'
    }).then(resp => { resolve(true); });
})
.catch(err => {
    console.log(err);
    resolve(false);
});
```

### RxJsEventEmitter

To communicate between 2 WebParts you can use the RxJSEventEmitter

```Powershell
npm i rx-lite
npm i @types/rx-lite
```

#### Receiver WebPart 

```Typescript
import { RxJsEventEmitter } from './RxJsEventEmitter';

export default class ... {
    private readonly eventEmitter: RxJsEventEmitter = RxJsEventEmitter.getInstance();

    ...

    private onClick(...): void {
    ...
    this.eventEmitter.emit('timShareData', data);
    } 
}
```

#### Sender WebPart 

```Typescript
import { RxJsEventEmitter } from './RxJsEventEmitter';

export default class ... {
    private readonly eventEmitter: RxJsEventEmitter = RxJsEventEmitter.getInstance();
    public constructor(...) {
        this.eventEmitter.on('timShareData', this.receiveData.bind(this));
        ...
    }

    ...

    protected receiveData(data: IEventData): void {
        ...
    }
}
```

#### RxJsEventEmitter.ts

```Typescript
import { Subject } from 'rx-lite';

export class RxJsEventEmitter {
  public subjects: Object;
  // tslint:disable-next-line: no-any
  public readonly hasOwnProp: any = {}.hasOwnProperty;

  // tslint:disable:no-string-literal
  public static getInstance(): RxJsEventEmitter {
    if (!window['RxJsEventEmitter']) {
      window['RxJsEventEmitter'] = new RxJsEventEmitter();
    }
    return window['RxJsEventEmitter'];
  }

  public emit(name: string, data: Object): void {
    const fnName: string = this._createName(name);

    if (!this.subjects[fnName]) {
      this.subjects[fnName] = new Subject();
    }
    this.subjects[fnName].onNext(data);
  }

  public on(name: string, handler: any): void {
    const fnName: string = this._createName(name);

    if (!this.subjects[fnName]) {
      this.subjects[fnName] = new Subject();
    }
    this.subjects[fnName].subscribe(handler);
  }

  public off(name: string): void {
    const fnName: string = this._createName(name);

    if (this.subjects[fnName]) {
      this.subjects[fnName].dispose();
      delete this.subjects[fnName];
    }
  }

  public dispose(): void {
    const subjects: Object = this.subjects;

    for (const prop in subjects) {
      if (this.hasOwnProp.call(subjects, prop)) {
        subjects[prop].dispose();
      }
    }

    this.subjects = {};
  }

  private constructor() {
    this.subjects = {};
  }
  private _createName(name: string): string {
    return `$${name}`;
  }
}
```

### UI Fabric Icons

[https://uifabricicons.azurewebsites.net](https://uifabricicons.azurewebsites.net)



