# Kusi's Knowledge Base

## Adaptive Cards with SPFx

[Viva Connections & SPFx Community calls](https://www.youtube.com/playlist?list=PLR9nK3mnD-OXdcwfcHGsGr78nHWLRsv1x)

[Adaptive Cards Sample Gallery](https://aka.ms/spfx-aces)

[Viva Connection Toolkit for Visual Studio Code](https://aka.ms/viva/vscode)

### Versions used

![SPFx](https://img.shields.io/badge/SPFx-1.18.0-green.svg)

![NodeJS](https://img.shields.io/badge/NodeJS-16.20.0-green.svg)

### Prepare dev environment

Install Visual Studio Code

Install latest possible Node.js with npm

you can use [NVM](../SPFx/index.md) to use different NodeJS Versions.

Call in PowerShell

```powershell
npm install gulp --global
npm install yo --global
npm install @microsoft/generator-sharepoint --global
gulp trust-dev-cert
```

### Create a new project

Call in PowerShell

```powershell
yo
```
and choose **\@microsoft/sharepoint** as generator. Use **Adaptive Card Extension** and **Generic Card Template** and follow the questions.

Replace in **config/serve.json** the **[tenantDomain]** with your Tenant Url.

### Use Image Card View

Per default it use a **BaseComponentsCardView** if you want change to **BaseImageCardView** replace following code snippets.

#### cardView\CardViews.ts
```javascript
import {
  BaseImageCardView,
  IImageCardParameters,
  ICardButton,
  ...
  }
...

export class CardView extends BaseImageCardView<
  I[...]AdaptiveCardExtensionProps,
  I[...]AdaptiveCardExtensionState
> {
  public get cardButtons(): [ICardButton] | [ICardButton, ICardButton] | undefined {
    return [
      {
        title: strings.QuickViewButton,
        action: {
          type: 'QuickView',
          parameters: {
            view: QUICK_VIEW_REGISTRY_ID
          }
        }
      }
    ];
  }

  public get data(): IImageCardParameters {
    return {
      title: strings.Title,
      iconProperty: require('../assets/....png'),
      primaryText: strings.PrimaryText,
      imageUrl: require('../assets/....png')
    };
  }

  public get onCardSelection(): IQuickViewCardAction |
  IExternalLinkCardAction | undefined {
    
  ...
  }
}
```

### Get Data from a SharePoint List

To use PnP and data-fns install following:

```powershell
npm install @pnp/sp
npm instll date-fns
```

To get Data from a SharePoint List you can edit following:
the List in this Example have following Structure:

|Column|Type|
|--|--|
|Title| Singleline text|
|Image| Picture|
|Description| Singleline text|
|Articledate| Date only|
|Link| Multiline text|

#### IListItem.ts

```javascript
export interface IListItem
{
    Title: string;
    Image: string;
    Description: string;
    Articledate: string;
    Link: string;
}
```

#### [...]AdaptiveCardExtension.ts

```javascript
import { spfi, SPFx } from "@pnp/sp";
import "@pnp/sp/webs";
import "@pnp/sp/items";
import "@pnp/sp/lists";
import "@pnp/sp/items/get-all";
import { format, parseISO } from 'date-fns';
import { IListItem } from './IListItem';
...

export interface INewsAdaptiveCardExtensionState {
  toptitle: string;
  items?: IListItem[];
}

...

public onInit(): Promise<void> {
    ...
    return this._fetchData();
}

private async _fetchData(): Promise<void> {
    const sp = spfi("https://[UrlToSharePoint]").using(SPFx(this.context));

    const items = await sp.web.lists.getByTitle("[ListName]").items
    .orderBy('Articledate', false).top(5).getAll();

    this.setState({
      items: items.map(i => {
        return {
          Title: i.Title,
          Image: i.Image ? JSON.parse(i.Image).serverRelativeUrl : '',
          Description: i.Description,
          Articledate: i.Articledate ? format(parseISO(i.Articledate), 'dd.MM.yyyy') : '',
          Link: i.Link
        } as IListItem
      })
    });
}
```

#### quickView\QuickView.ts

```javascript
import { IListItem } from '../IListItem';

export interface IQuickViewData {
  items: IListItem[];
  ...
}

...

public get data(): IQuickViewData {
  return {
      ...
      items: this.state.items ? this.state.items : [],
  }
}
...
```

#### quickView\template\QuickvViewTemplate.json

To Design your individual Card you can use the [Adaptive Cards Designer](https://adaptivecards.io/designer/)

```json
{
// ...
  "body": [
    {
        "type": "TextBlock",
        // Property name of a root element
        "text": "${toptitle}"
    },
    // Start Loop
    {
      "type": "Container",
      // data Property name of Array
      "$data": "{items}",
      "separator": true,
      "items": [
        {
          "type": "ColumnSet",
          "columns": [
            {
              "type": "Column",
              "items": [
                {
                  "type": "TextBlock",
                  // Property name of a item element
                  "text": "${Title}"
                },{
                    "type": "TextBlock",
                    // Within a Container you can access the root properties like
                    "text": "${$root.toptitle}"
                }
              ]
            }
          ],
          // click event for this section
          "selectAction": {
              "type": "Action.OpenUrl",
              "url": "http://..."
          }
        }
      ]
    }
    // End Loop
  ]
// ...
}
```

### Debug widget

```powershell
npm gulp serve
```