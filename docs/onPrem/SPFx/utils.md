# Kusi's Knowledge Base

## Convert Base64 to File

```typescript
public convertBase64ToFile(base64string: string, fileName: string, type: string): File {
    const binaryImg: string = atob(base64string.substring(base64string.indexOf(",") + 1));
    const length: number = binaryImg.length;
    const ab: ArrayBuffer = new ArrayBuffer(length);
    const ua: Uint8Array = new Uint8Array(ab);
    for (let i: number = 0; i < length; i++) {
      ua[i] = binaryImg.charCodeAt(i);
    }
    const blob: Blob = new Blob([ab], {
      type: type,
    });
    return new File([blob], fileName, { type: type });
  }
```

## Upload file to folder

```typescript
public async uploadFileToFolder(
    fileObj: File,
    url: string,
    urlOrigin: string,
    instance: IPublicClientApplication,
    accounts: AccountInfo[]
  ): Promise<number> {
    return new Promise<number>((resolve, reject) => {
      this.getFileBuffer(fileObj).then(
        (f) => {
          if (f) {
            const request: RedirectRequest = {
              account: accounts[0],
              scopes: [urlOrigin + "/.default"],
            };
            instance.acquireTokenSilent(request).then((response) => {
              fetch(url, {
                method: "POST",
                headers: {
                  Authorization: `Bearer ${response.accessToken}`,
                  "odata-version": "3.0",
                  Accept: "application/json;odata=verbose",
                  "Content-Type": "application/json;odata=verbose",
                  "IF-MATCH": "*",
                  "X-HTTP-Method": "POST",
                },
                body: f,
              })
                .then((res) => res.json())
                .then((res) => {
                  if (res && res.d) {
                    resolve(res.d.ListItemAllFields ? res.d.ListItemAllFields.Id : 0);
                  } else if (res && res.error) {
                    reject(`${res.error.code}: ${res.error.message.value}`);
                  }
                })
                .catch((e) => {
                  reject(e.message);
                });
            });
          } else {
            reject("file buffer ist leer");
          }
        },
        (e) => {
          reject(e);
        }
      );
    });
  }
```

## Get file Buffer

```typescript
  private getFileBuffer(uploadFile: File): Promise<string | ArrayBuffer> {
    const reader: FileReader = new FileReader();
    return new Promise<string | ArrayBuffer>((resolve, reject) => {
      reader.onloadend = () => {
        const target: FileReader = event.target as FileReader;
        resolve(target.result);
      };
      reader.onerror = (e) => {
        reject(e);
      };
      reader.readAsArrayBuffer(uploadFile);
    });
  }
```

## Get User Properties

```typescript
    public static GetUserInfo(spHttpClient: SPHttpClient, siteurl: string,
        culture: string): Promise<string> {
        const storageKey: string = 'currentCultureKey';
        let currentCulture: string = undefined;
        return new Promise<string>((resolve, reject) => {
            if (localStorage.getItem(storageKey) != null) {
                currentCulture = localStorage.getItem(storageKey);
                resolve(currentCulture);
            } else {

                // Get from SP.UserProfiles.PeopleManager API.
                spHttpClient.post(`${siteurl}/_api/SP.UserProfiles.PeopleManager/getmyproperties`,
                    SPHttpClient.configurations.v1, undefined)
                    .then(res => res.json())
                    .then(res => {
                        currentCulture = res.UserProfileProperties.filter(i =>
                            i.Key === 'SPS-MUILanguages')[0].Value.split(',')[0];
                        localStorage.setItem(storageKey, currentCulture);
                        resolve(currentCulture);
                    });
            }
        });
    }

```

## Get Terms

```typescript
import { Session, ITermData } from '@pnp/sp-taxonomy';
const taxonomy = new Session(this.props.siteurl);
const stores = await taxonomy.termStores.get();
await stores[0].getTermSetById(termGUID).terms.get().then((resp2: ITermData[]) => {
  ...
});
```
