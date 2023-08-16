# Kusi's Knowledge Base

## ichicraft

### Prepare dev environment

Install Visual Studio Code

Install latest Node.js with npm

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

To use the MSGraphClient install following:

```powershell
npm install @microsoft/sp-core-library@1.15.2
npm install @microsoft/sp-webpart-base@1.15.2
```

example App.tsx

```javascript
import * as React from 'react';

import { WidgetContext } from '@ichicraft/widgets-widget-base';
import styles from '../styles/widget.module.scss';
import { UserConfiguration } from '../types';
import { AdminConfiguration } from '../types';
import { MSGraphClient } from '@microsoft/sp-http';
import { Spinner } from '@fluentui/react';

export interface AppProps {
    context: WidgetContext;
    userConfiguration: UserConfiguration;
    adminConfiguration: AdminConfiguration;
}

export interface Item {
    id: string;
    title: string;
    description: string;
    imageUrl: string;
}

const App: React.FunctionComponent<AppProps> = (props) => {
    const [items, setItems] = React.useState<Item[]>([]);
    const [isLoading, setIsLoading] = React.useState<boolean>(true);

    React.useEffect(() => {
        props.context.msGraphClientFactory.getClient().then((client: MSGraphClient) => {
            return client
                .api(`sites('root')/lists('News')/items?$expand=fields&$orderby=fields/Sort`)
                .version('v1.0')
                .get((err, res) => {
                    if (err) {
                        console.log(err);
                        setIsLoading(false);
                    } else {
                        setItems(
                            res.value.map((t) => {
                                return {
                                    id: t.fields.id,
                                    title: t.fields.Title,
                                    description: t.fields.Description,
                                    imageUrl: t.fields.ImageUrl,
                                };
                            })
                        );
                        setIsLoading(false);
                    }
                });
        });
    }, [props.context.msGraphClientFactory]);

    return (
        <div className={styles.root}>
            {isLoading ? (
                <Spinner label='Loading items...' />
            ) : (
                <div>
                    {items.map((i) => (
                        <div key={i.id}>
                            <h1>{i.title}</h1>
                            <div>{i.description}</div>
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

- Edit the scriptUrl in the manifest.config.js

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
