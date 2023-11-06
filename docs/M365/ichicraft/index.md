# Kusi's Knowledge Base

## ichicraft

### Versions used

![NodeJS](https://img.shields.io/badge/NodeJS-16.20.0-green.svg)

### Prepare dev environment

Install Visual Studio Code

Install latest possible Node.js with npm (at now 16.20.0)

you can use [NVM](../SPFx/index.md) to use different NodeJS Versions.

Call in PowerShell

```powershell
npm install gulp --global
npm install yo --global
npm install @microsoft/generator-sharepoint --global
npm install @ichicraft/generator-widgets --global
gulp trust-dev-cert
```

### Create a new project

Call in PowerShell

```powershell
yo
```
and choose **@ichicraft/generator-widgets** as generator and follow the questions.

To use the AADHttpClientFactory install following:

```powershell
npm install @pnp/sp
```

example App.tsx

```javascript
import * as React from 'react';

import { WidgetContext } from '@ichicraft/widgets-widget-base';
import styles from '../styles/widget.module.scss';
import { Spinner } from '@fluentui/react';
import { sp } from '@pnp/sp';
import '@pnp/sp/webs';
import '@pnp/sp/lists';
import '@pnp/sp/items';

export interface AppProps {
    context: WidgetContext;
}

export interface Item {
    id: string;
    title: string;
    description: string;
}

const App: React.FunctionComponent<AppProps> = (props) => {
    const [items, setItems] = React.useState<Item[]>([]);
    const [isLoading, setIsLoading] = React.useState<boolean>(true);

    React.useEffect(() => {
        //current Web
        const sp = spfi().using(SPFx(props.context.aadHttpClientFactory));
        //another Web
        const sp = spfi(webUrl).using(SPFx(props.context.aadHttpClientFactory));
        sp.web.lists
            .getByTitle('News')
            .items.select('Title', 'ID', 'Description')
            .get()
            .then((items) => {
                setItems(
                    items.map((item) => {
                        return {
                            id: item.ID,
                            title: item.Title,
                            description: item.Description,
                        };
                    })
                );
                setIsLoading(false);
            })
            .catch((err) => {
                alert(err);
                setIsLoading(false);
            });
    }, [props.context.aadHttpClientFactory]);

    return (
        <div className={styles.root}>
            {isLoading ? (
                <Spinner label='Am laden...' />
            ) : (
                <div>
                    {items.map((i) => (
                        <div key={i.id}>
                            <div>
                                <div>{i.title}</div>
                                <div>
                                    <div dangerouslySetInnerHTML={{ __html: i.description }} />
                                </div>
                            </div>
                        </div>
                    ))}
                </div>
            )}
        </div>
    );
};

export default App;
```

### Debug widget

```powershell
npm run start
```

### Deploy widget

- Edit the scriptUrl in the **manifest.config.js** with the full url like **https://[siteUrl]/[DocLib]/[Folder]/bundle.js**

- make packages with:

    ```powershell
    npm run build
    ```

- Create a folder with the Widget Name in the document library from the root site collection

- Upload **budle.js** from dist folder into this folder

- Register Widget 
  - on Dashboard click in the right top corner on **...**
  - choose **General Settings**
  - navigate to **Widgets** 
  - click on **Register your own widget**
  - click on **Read manifest.json**
  - Upload **manifest.json** from dist folder
  - click on **Save**
