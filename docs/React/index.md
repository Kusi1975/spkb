# Kusi's Knowledge Base

## React

### Promise

Wait on Promise from Sub Async functions in a loop.

```javascript
    public static async getItemNamesAsync(itemids: number[]
        siteUrl: string, spHttpClient: SPHttpClient): Promise<string[]> {
        return new Promise<string[]>(resolve => {
            const itemsDetail: string[] = [];
            const allPromises: Promise<string>[] = [];
            this.getItemsAsync(itemids).then((items) => {
                for (const item of items) {
                    allPromises.push(this.getDetailInfoAsync(item.id, siteUrl, spHttpClient));
                }
                Promise.all(allPromises).then((all: string[]) => {
                    all.forEach(i => {
                        itemsDetail.push(i);
                    });
                    resolve(itemsDetail);
                });
            });
        });
    }
```

### Get Url Parameter

```javascript
private GetUrlParameters(): { [id: string]: string } {
    const params: { [id: string]: string } = {};
    document.location.search.substring(1).split('&').forEach(t => {
      if (t.indexOf('=') > 0) {
        params[t.substring(0, t.indexOf('=')).toLowerCase()] = decodeURIComponent(t.substring(t.indexOf('=') + 1));
      }
    });
    return params;
}
```