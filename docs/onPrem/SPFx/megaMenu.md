# Kusi's Knowledge Base

## MegaMenu

### IMegaMenu.ts

```typescript
import { SPHttpClient } from '@microsoft/sp-http';

export interface IMegaMenuProps {
  siteurl: string;
  spHttpClient: SPHttpClient;
  culture: string;
}
export interface IMegaMenuUrl {
  Url: string;
  Description: string;
}
export interface IMegaMenuItem {
  ID: number;
  URL: IMegaMenuUrl;
  IsLink: boolean;
  Data: IMegaMenuDetails;
}
export interface IMegaMenuLink {
  Title: string;
  UIFabricIconName: string;
  Url: string;
  Target: string;
}
export interface IMegaMenuGroup {
  Title: string;
  TitleLink: string;
  TitleTarget: string;
  UIFabricIconName: string;
  ImageUrl: string;
  ImageLink: string;
  ImageTarget: string;
  Links: Array<IMegaMenuLink>;
}
export interface IMegaMenuDetails {
  Groups: Array<IMegaMenuGroup>;
  Columns: number;
}
export interface IMegaMenuState {
  error: undefined;
  isLoaded: boolean;
  items: Array<IMegaMenuItem>;
}
```

### MegaMenu.Module.scss

```scss
:global(.ms-compositeHeader-horizontalNav) {
  display: none;
}
.megamenu {
  .error {
    color: red;
  }
  .loadingWrapper {
    padding-top: 80px;
    padding-bottom: 80px;
  }
  .megamenutop {
    line-height: 64px;
    font-size: 24px;
    padding: 0 20px;
    cursor: pointer;
  }
  .megamenutopcontainer:hover {
    .megamenutop {
      color: #d50025;
    }
    .megamenuflyout {
      opacity: 1;
      transition: 0.8s;
      transform: translate(0, 10px);
      z-index: 100;
      border: 1px #dddddd solid;
      height: unset;
    }
  }

  .megamenutopcontainer {
    display: inline-block;
    color: #666666;
    .megamenuflyout {
      opacity: 0;
      transition: 0.5s;
      z-index: -1;
      position: absolute;
      left: 20px;
      right: 20px;
      top: 54px;
      color: #000000;
      background-color: rgba(255, 255, 255, 0.95);
      padding: 0 20px;
      border: 1px #ffffff solid;
      height: 0;
      overflow: hidden;
    }
  }

  .megamenugroup {
    float: left;
  }
  .megamenugroupblock {
    padding: 20px;
  }
  .megamenugroup8 {
    width: 12.5%;
  }
  .megamenugroup7 {
    width: 14.2857%;
  }
  .megamenugroup6 {
    width: 16.6666%;
  }
  .megamenugroup5 {
    width: 20%;
  }
  .megamenugroup4 {
    width: 25%;
  }
  .megamenugroup3 {
    width: 33.3333%;
  }
  .megamenugroup2 {
    width: 50%;
  }
  .megamenugroup1 {
    width: 100%;
  }
  .megamenugroups {
    position: relative;
  }

  @media only screen and (max-width: 1100px) {
    .megamenuimage {
      display: none;
    }
  }
  @media only screen and (max-width: 250px) {
    .megamenugroup2,
    .megamenugroup3,
    .megamenugroup4,
    .megamenugroup5,
    .megamenugroup6,
    .megamenugroup7,
    .megamenugroup8 {
      width: 100%;
      clear: left;
    }
  }
  @media only screen and (max-width: 500px) and (min-width: 251px) {
    .megamenugroup3,
    .megamenugroup4,
    .megamenugroup5,
    .megamenugroup6,
    .megamenugroup7,
    .megamenugroup8 {
      width: 50%;
    }
    .megamenugroup3:nth-child(3),
    .megamenugroup4:nth-child(3),
    .megamenugroup5:nth-child(3),
    .megamenugroup5:nth-child(5),
    .megamenugroup6:nth-child(3),
    .megamenugroup6:nth-child(5),
    .megamenugroup7:nth-child(3),
    .megamenugroup7:nth-child(5),
    .megamenugroup7:nth-child(7),
    .megamenugroup8:nth-child(3),
    .megamenugroup8:nth-child(5),
    .megamenugroup8:nth-child(7) {
      clear: left;
    }
  }
  @media only screen and (max-width: 750px) and (min-width: 501px) {
    .megamenugroup4,
    .megamenugroup5,
    .megamenugroup6,
    .megamenugroup7,
    .megamenugroup8 {
      width: 33.3333%;
    }
    .megamenugroup4:nth-child(4),
    .megamenugroup5:nth-child(4),
    .megamenugroup6:nth-child(4),
    .megamenugroup7:nth-child(4),
    .megamenugroup7:nth-child(7),
    .megamenugroup8:nth-child(4),
    .megamenugroup8:nth-child(7) {
      clear: left;
    }
  }
  @media only screen and (max-width: 1000px) and (min-width: 751px) {
    .megamenugroup5,
    .megamenugroup6,
    .megamenugroup7,
    .megamenugroup8 {
      width: 25%;
    }
    .megamenugroup5:nth-child(5),
    .megamenugroup6:nth-child(5),
    .megamenugroup7:nth-child(5),
    .megamenugroup8:nth-child(5) {
      clear: left;
    }
  }
  @media only screen and (max-width: 1250px) and (min-width: 1001px) {
    .megamenugroup6,
    .megamenugroup7,
    .megamenugroup8 {
      width: 20%;
    }
    .megamenugroup6:nth-child(6),
    .megamenugroup7:nth-child(6),
    .megamenugroup8:nth-child(6) {
      clear: left;
    }
  }
  @media only screen and (max-width: 1500px) and (min-width: 1251px) {
    .megamenugroup7,
    .megamenugroup8 {
      width: 16.6666%;
    }
    .megamenugroup7:nth-child(7),
    .megamenugroup8:nth-child(7) {
      clear: left;
    }
  }
  @media only screen and (max-width: 1750px) and (min-width: 1501px) {
    .megamenugroup8 {
      width: 14.2857%;
    }
    .megamenugroup8:nth-child(8) {
      clear: left;
    }
  }

  .megamenuTopLink {
    line-height: 64px;
    font-size: 24px;
    padding: 0 20px;
    display: inline-block;
    a {
      text-decoration: none;
      color: #333333;
    }
    a:visited {
      color: #333333;
    }
    a:hover {
      color: #d50025;
    }
  }

  .megamenutitle {
    font-size: 1.2em;
    border-bottom: 1px solid #d2d2d2;
    margin-bottom: 10px;
    padding-bottom: 5px;
    color: #222222;
    a {
      text-decoration: none;
      color: #333333;
    }
    a:visited {
      color: #333333;
    }
    a:hover {
      color: #d50025;
    }
  }

  .megamenulink :global(.ms-Icon),
  .megamenutitle :global(.ms-Icon) {
    padding-right: 5px;
  }
  @media only screen and (max-width: 750px) {
    .megamenulink {
      padding: 0;
    }
  }

  .megamenulink {
    padding: 5px 0;
    a:hover {
      color: #d50025;
    }
    a,
    a:visited {
      color: #333333;
      text-decoration: none;
    }
  }
  .megamenuimage {
    padding: 5px 0;
    img {
      width: 200px;
    }
  }
}
```

### MegaMenu.tsx

```typescript
import * as React from 'react';
import { Utils } from '../../../../common/Utils';
import { IMegaMenuItem, IMegaMenuProps, IMegaMenuState } from './IMegaMenu';
import styles from './MegaMenu.module.scss';
import { SPHttpClient } from '@microsoft/sp-http';

export default class MegaMenu extends React.Component<IMegaMenuProps, {}> {
  public constructor(props: IMegaMenuProps) {
    super(props);
    this.state = {
      error: undefined,
      isLoaded: false,
      items: Array<IMegaMenuItem>()
    };
  }

  public componentDidMount(): void {
    Utils.GetStoredUserInfo(this.props.spHttpClient, this.props.siteurl, this.props.culture)
      .then(c => {
        this.getMegaMenuConfig(c); });
  }

  public getMegaMenuConfig(culture: string): void {
    this.props.spHttpClient.post(`${this.props.siteurl}/_api/web/lists/getbytitle('Megamenu')` +
      `/GetItems?$orderby=Sorting asc`,
      SPHttpClient.configurations.v1, {
      headers: {
        'odata-version': '3.0',
        'Accept': 'application/json;odata=verbose',
        'Content-Type': 'application/json;odata=verbose'
      },

      body: JSON.stringify({
        'query': {
          '__metadata': { 'type': 'SP.CamlQuery' },
          'ViewXml': `<View><Query><Where><Eq><FieldRef Name='Sprache' />` +
            `<Value Type='TaxonomyFieldType'>${culture}</Value></Eq>` +
            `</Where></Query></View>`
        }
      })
    })
      .then(res => res.json())
      .then(data => {
        this.setState({
          isLoaded: true,
          items: data.d.results
        });
      }).catch(error => {
        this.setState({
          isLoaded: true,
          items: [],
          error: error
        });
      }
      );
  }
  public render(): JSX.Element {
    const { error, isLoaded, items } = this.state as IMegaMenuState;
    let retValue: JSX.Element = <div>Keine Daten vorhanden.</div>;

    if (error) {
      retValue = <div className={styles.error}>Fehler: {error}</div>;
    } else if (!isLoaded) {
      retValue = <div>Am Laden...</div>;
    } else if (items) {
      retValue = <div> {
        items
          .filter(i => i.URL)
          .map(i => i.IsLink ? <div className={styles.megamenuTopLink}><a href={i.URL.Url}>{i.URL.Description}</a></div> :
            <div role='button' className={styles.megamenutopcontainer}
              onMouseOver={(e) => this.loadMegaMenuFlyout(i.ID, i.URL.Url)}>
              <div className={styles.megamenutop}>{i.URL.Description}</div>
              <div className={styles.megamenuflyout}>
                <div className={styles.megamenugroups}>
                  {i.Data && i.Data.Groups ? (
                    <div className={styles.megamenugroups}>
                      {i.Data.Groups.map(g =>
                        <div
                          className={
                            styles.megamenugroup + ' ' +
                            styles.megamenugroup.replace('megamenugroup', 'megamenugroup' + i.Data.Columns)}
                        >
                          <div className={styles.megamenugroupblock}>
                            <div className={styles.megamenutitle}>{g.TitleLink
                              ? <a href={g.TitleLink} target={g.TitleTarget}>{g.UIFabricIconName
                                ? <i className={'ms-Icon ms-Icon--' + g.UIFabricIconName}></i>
                                : ''}{g.Title}</a>
                              : <div>{g.UIFabricIconName
                                ? <i className={'ms-Icon ms-Icon--' + g.UIFabricIconName}></i>
                                : ''}{g.Title}</div>
                            }</div>
                            {g.ImageUrl
                              ? (g.ImageLink
                                ? <div className={styles.megamenuimage}>
                                  <a href={g.ImageLink} target={g.ImageTarget}>
                                    <img src={g.ImageUrl} alt={g.Title} />
                                  </a>
                                </div>
                                : <div className={styles.megamenuimage}>
                                  <img src={g.ImageUrl} alt={g.Title} />
                                </div>)
                              : ('')}
                            {g.Links.map(l =>
                              <div className={styles.megamenulink}>
                                <a href={l.Url} target={l.Target}>{
                                  l.UIFabricIconName
                                    ? <i className={'ms-Icon ms-Icon--' + l.UIFabricIconName}></i>
                                    : ''
                                }{l.Title}</a>
                              </div>
                            )}
                          </div>
                        </div>)
                      }
                    </div>)
                    : ('')}
                </div>
              </div>
            </div>
          )
      }</div>;
    }
    return <div className={styles.megamenu}>{retValue}</div>;
  }

  private loadMegaMenuFlyout(parentId: number, url: string): void {
    const { items } = this.state as IMegaMenuState;

    const item: IMegaMenuItem = items.filter((i) => i.ID === parentId)[0];
    if (!item.Data) {
      fetch(url)
        .then(res => res.json())
        .then(data => {
          item.Data = data;
          this.setState({
            isLoaded: true,
            items: items
          });
        });
    }
  }
}
```

### PnP Template for MegaMenu SharePoint List 

```xml
<?xml version="1.0"?>
<pnp:Provisioning xmlns:pnp="http://schemas.dev.office.com/PnP/2020/02/ProvisioningSchema">
  <pnp:Preferences Generator="OfficeDevPnP.Core, Version=3.28.2012.0, Culture=neutral, PublicKeyToken=5e633289e95c321a" />
  <pnp:Templates ID="CONTAINER-TEMPLATE-93B77BA906D845ADBB840DD999ED3F6A">
    <pnp:ProvisioningTemplate ID="TEMPLATE-93B77BA906D845ADBB840DD999ED3F6A" Version="1" BaseSiteTemplate="STS#3" Scope="RootSite">
      <pnp:Lists>
        <pnp:ListInstance Title="MegaMenu" Description="" DocumentTemplate="" TemplateType="103" Url="Lists/MegaMenu" MinorVersionLimit="0" MaxVersionLimit="0" DraftVersionVisibility="0" TemplateFeatureID="00bfea71-2062-426c-90bf-714c59600103" EnableAttachments="false" DefaultDisplayFormUrl="{site}/Lists/MegaMenu/DispForm.aspx" DefaultEditFormUrl="{site}/Lists/MegaMenu/EditForm.aspx" DefaultNewFormUrl="{site}/Lists/MegaMenu/NewForm.aspx" ImageUrl="{site}/_layouts/15/images/itlink.png?rev=43" IrmExpire="false" IrmReject="false" IsApplicationList="false" ValidationFormula="" ValidationMessage="">
          <pnp:ContentTypeBindings>
            <pnp:ContentTypeBinding ContentTypeID="0x0105" Default="true" />
            <pnp:ContentTypeBinding ContentTypeID="0x0120" />
          </pnp:ContentTypeBindings>
          <pnp:Views>
            <View Name="{23F139A2-3FC2-475A-9CCF-A70A3C3041D5}" DefaultView="TRUE" MobileView="TRUE" MobileDefaultView="TRUE" Type="HTML" DisplayName="Alle Hyperlinks" Url="{site}/Lists/MegaMenu/AllItems.aspx" Level="1" BaseViewID="1" ContentTypeID="0x" ImageUrl="/_layouts/15/images/links.png?rev=43">
              <Query>
                <OrderBy>
                  <FieldRef Name="Order" Ascending="TRUE" />
                </OrderBy>
              </Query>
              <ViewFields>
                <FieldRef Name="DocIcon" />
                <FieldRef Name="Edit" />
                <FieldRef Name="URLwMenu" />
                <FieldRef Name="Comments" />
                <FieldRef Name="Sorting" />
                <FieldRef Name="Sprache" />
                <FieldRef Name="IsLink" />
              </ViewFields>
              <RowLimit Paged="TRUE">30</RowLimit>
              <JSLink>clienttemplates.js</JSLink>
            </View>
          </pnp:Views>
          <pnp:Fields>
            <Field Type="Note" DisplayName="Sprache_0" StaticName="f945129fdc4041bda8256aa59c49b2f1" Name="f945129fdc4041bda8256aa59c49b2f1" ID="{18fccc4e-c290-44c6-ac3b-67a3ea6f2c94}" ShowInViewForms="FALSE" Required="FALSE" Hidden="TRUE" CanToggleHidden="TRUE" ColName="ntext4" RowOrdinal="0" Version="8" />
            <Field ID="{7a7f3660-d408-4734-9aa0-2d86e33d79ab}" ReadOnly="TRUE" Filterable="FALSE" Type="Computed" Name="URLwMenu2" DisplayName="URL" DisplayNameSrcField="URL" ClassInfo="Menu" AuthoringInfo="(URL mit Bearbeitungsmenü) (old)" SourceID="http://schemas.microsoft.com/sharepoint/v3" StaticName="URLwMenu2" Version="2">
              <FieldRefs>
                <FieldRef Name="URL" />
                <FieldRef Name="FileLeafRef" />
                <FieldRef Name="FileRef" />
                <FieldRef Name="FSObjType" />
                <FieldRef Name="_EditMenuTableStart" />
                <FieldRef Name="_EditMenuTableEnd" />
              </FieldRefs>
              <DisplayPattern>
                <FieldSwitch>
                  <Expr>
                    <GetVar Name="FreeForm" />
                  </Expr>
                  <Case Value="TRUE">
                    <IfEqual>
                      <Expr1>
                        <LookupColumn Name="FSObjType" />
                      </Expr1>
                      <Expr2>1</Expr2>
                      <Then>
                        <Field Name="FileLeafRef" />
                      </Then>
                      <Else>
                        <Field Name="URL" />
                      </Else>
                    </IfEqual>
                  </Case>
                  <Default>
                    <Field Name="_EditMenuTableStart" />
                    <IfEqual>
                      <Expr1>
                        <LookupColumn Name="FSObjType" />
                      </Expr1>
                      <Expr2>1</Expr2>
                      <Then>
                        <Switch>
                          <Expr>
                            <GetVar Name="RecursiveView" />
                          </Expr>
                          <Case Value="1">
                            <LookupColumn Name="FileLeafRef" HTMLEncode="TRUE" />
                          </Case>
                          <Default>
                            <HTML>&lt;a onfocus="OnLink(this)" href="javascript:SubmitFormPost()" onclick='javascript:ClearSearchTerm("</HTML>
                            <GetVar Name="View" />
                            <HTML>");ClearSearchTerm("");javascript:SubmitFormPost("</HTML>
                            <SetVar Name="RootFolder">
                              <HTML>/</HTML>
                              <LookupColumn Name="FileRef" />
                            </SetVar>
                            <ScriptQuote NotAddingQuote="TRUE">
                              <FilterLink Default="" Paged="FALSE" />
                            </ScriptQuote>
                            <HTML>");javascript:return false;'&gt;</HTML>
                            <LookupColumn Name="FileLeafRef" HTMLEncode="TRUE" />
                            <HTML>&lt;/a&gt;</HTML>
                          </Default>
                        </Switch>
                      </Then>
                      <Else>
                        <Switch>
                          <Expr>
                            <Column Name="URL" />
                          </Expr>
                          <Case Value="">
                            <Column2 Name="URL" HTMLEncode="TRUE" />
                          </Case>
                          <Default>
                            <FieldSwitch>
                              <Expr>
                                <FieldProperty Name="URL" Select="Format" />
                              </Expr>
                              <Case Value="Image">
                                <HTML>&lt;img onfocus="OnLink(this)" src="</HTML>
                                <Column Name="URL" HTMLEncode="TRUE" />
                                <HTML>" alt="</HTML>
                                <Column2 Name="URL" HTMLEncode="TRUE" />
                                <HTML>" /&gt;</HTML>
                              </Case>
                              <Default>
                                <HTML>&lt;a onfocus="OnLink(this)" href="</HTML>
                                <Column Name="URL" HTMLEncode="TRUE" />
                                <HTML>"&gt;</HTML>
                                <Switch>
                                  <Expr>
                                    <Column2 Name="URL" />
                                  </Expr>
                                  <Case Value="">
                                    <Column Name="URL" HTMLEncode="TRUE" />
                                  </Case>
                                  <Default>
                                    <Column2 Name="URL" HTMLEncode="TRUE" />
                                  </Default>
                                </Switch>
                                <HTML>&lt;/a&gt;</HTML>
                              </Default>
                            </FieldSwitch>
                          </Default>
                        </Switch>
                      </Else>
                    </IfEqual>
                    <Field Name="_EditMenuTableEnd" />
                  </Default>
                </FieldSwitch>
              </DisplayPattern>
            </Field>
            <Field Type="Number" DisplayName="Sorting" Required="FALSE" EnforceUniqueValues="FALSE" Indexed="FALSE" Decimals="0" ID="{2f8251cf-3218-4c5a-aa8c-01d65f2401d2}" SourceID="{{listid:MegaMenu}}" StaticName="Sorting" Name="Sorting" ColName="float1" RowOrdinal="0" />
            <Field Type="TaxonomyFieldType" DisplayName="Sprache" List="{listid:TaxonomyHiddenList}" WebId="{siteid}" ShowField="Term1031" Required="FALSE" EnforceUniqueValues="FALSE" ID="{f945129f-dc40-41bd-a825-6aa59c49b2f1}" SourceID="{{listid:MegaMenu}}" StaticName="Sprache" Name="Sprache0" ColName="int3" RowOrdinal="0" Version="11">
              <Default />
              <Customization>
                <ArrayOfProperty>
                  <Property>
                    <Name>SspId</Name>
                    <Value xmlns:q1="http://www.w3.org/2001/XMLSchema" p4:type="q1:string" xmlns:p4="http://www.w3.org/2001/XMLSchema-instance">{sitecollectiontermstoreid}</Value>
                  </Property>
                  <Property>
                    <Name>GroupId</Name>
                  </Property>
                  <Property>
                    <Name>TermSetId</Name>
                    <Value xmlns:q2="http://www.w3.org/2001/XMLSchema" p4:type="q2:string" xmlns:p4="http://www.w3.org/2001/XMLSchema-instance">{termsetid:AVAnet:Sprache}</Value>
                  </Property>
                  <Property>
                    <Name>AnchorId</Name>
                    <Value xmlns:q3="http://www.w3.org/2001/XMLSchema" p4:type="q3:string" xmlns:p4="http://www.w3.org/2001/XMLSchema-instance">00000000-0000-0000-0000-000000000000</Value>
                  </Property>
                  <Property>
                    <Name>UserCreated</Name>
                    <Value xmlns:q4="http://www.w3.org/2001/XMLSchema" p4:type="q4:boolean" xmlns:p4="http://www.w3.org/2001/XMLSchema-instance">false</Value>
                  </Property>
                  <Property>
                    <Name>Open</Name>
                    <Value xmlns:q5="http://www.w3.org/2001/XMLSchema" p4:type="q5:boolean" xmlns:p4="http://www.w3.org/2001/XMLSchema-instance">false</Value>
                  </Property>
                  <Property>
                    <Name>TextField</Name>
                    <Value xmlns:q6="http://www.w3.org/2001/XMLSchema" p4:type="q6:string" xmlns:p4="http://www.w3.org/2001/XMLSchema-instance">{18fccc4e-c290-44c6-ac3b-67a3ea6f2c94}</Value>
                  </Property>
                  <Property>
                    <Name>IsPathRendered</Name>
                    <Value xmlns:q7="http://www.w3.org/2001/XMLSchema" p4:type="q7:boolean" xmlns:p4="http://www.w3.org/2001/XMLSchema-instance">false</Value>
                  </Property>
                  <Property>
                    <Name>IsKeyword</Name>
                    <Value xmlns:q8="http://www.w3.org/2001/XMLSchema" p4:type="q8:boolean" xmlns:p4="http://www.w3.org/2001/XMLSchema-instance">false</Value>
                  </Property>
                  <Property>
                    <Name>TargetTemplate</Name>
                  </Property>
                  <Property>
                    <Name>CreateValuesInEditForm</Name>
                    <Value xmlns:q9="http://www.w3.org/2001/XMLSchema" p4:type="q9:boolean" xmlns:p4="http://www.w3.org/2001/XMLSchema-instance">false</Value>
                  </Property>
                  <Property>
                    <Name>FilterAssemblyStrongName</Name>
                    <Value xmlns:q10="http://www.w3.org/2001/XMLSchema" p4:type="q10:string" xmlns:p4="http://www.w3.org/2001/XMLSchema-instance">Microsoft.SharePoint.Taxonomy, Version=16.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c</Value>
                  </Property>
                  <Property>
                    <Name>FilterClassName</Name>
                    <Value xmlns:q11="http://www.w3.org/2001/XMLSchema" p4:type="q11:string" xmlns:p4="http://www.w3.org/2001/XMLSchema-instance">Microsoft.SharePoint.Taxonomy.TaxonomyField</Value>
                  </Property>
                  <Property>
                    <Name>FilterMethodName</Name>
                    <Value xmlns:q12="http://www.w3.org/2001/XMLSchema" p4:type="q12:string" xmlns:p4="http://www.w3.org/2001/XMLSchema-instance">GetFilteringHtml</Value>
                  </Property>
                  <Property>
                    <Name>FilterJavascriptProperty</Name>
                    <Value xmlns:q13="http://www.w3.org/2001/XMLSchema" p4:type="q13:string" xmlns:p4="http://www.w3.org/2001/XMLSchema-instance">FilteringJavascript</Value>
                  </Property>
                </ArrayOfProperty>
              </Customization>
            </Field>
            <Field DisplayName="IsLink" Title="IsLink" Type="Boolean" ID="{4dff5d3e-cc72-4282-b9bf-e24ce36abb96}" SourceID="{{listid:MegaMenu}}" StaticName="IsLink" Name="IsLink" ColName="bit1" RowOrdinal="0">
              <Default>0</Default>
            </Field>
          </pnp:Fields>
          <pnp:FieldRefs>
            <pnp:FieldRef ID="41614f06-ec6f-4810-aa2d-7eb573a88234" Name="h0634a83c6cd443e9c43201af4a3b74e" Hidden="true" DisplayName="Sprache_0" />
            <pnp:FieldRef ID="c29e077d-f466-4d8e-8bbe-72b66c5f205c" Name="URL" Required="true" DisplayName="URL" />
            <pnp:FieldRef ID="9da97a8a-1da5-4a77-98d3-4bc10456e700" Name="Comments" DisplayName="Notizen" />
            <pnp:FieldRef ID="10634a83-c6cd-443e-9c43-201af4a3b74e" Name="Sprache" DisplayName="Sprache" />
            <pnp:FieldRef ID="2a9ab6d3-268a-4c1c-9897-e5f018f87e64" Name="URLwMenu" DisplayName="URL" />
            <pnp:FieldRef ID="aeaf07ee-d2fb-448b-a7a3-cf7e062d6c2a" Name="URLNoMenu" DisplayName="URL" />
          </pnp:FieldRefs>
        </pnp:ListInstance>
      </pnp:Lists>
    </pnp:ProvisioningTemplate>
  </pnp:Templates>
</pnp:Provisioning>
```
