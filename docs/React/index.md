# Kusi's Knowledge Base

## React

### Promise

Wait on Promise from Sub Async functions in a loop.

```javascript
    public static async getItemNamesAsync(itemids: number[]
        siteUrl: string, spHttpClient: SPHttpClient): Promise<string[]> {
        return new Promise<string[]>(resolve => {
            const itemsDetail: string[] = [];
            const all: Promise<string>[] = [];
            this.getItemsAsync(itemids).then((items) => {
                for (const item of items) {
                    all.push(this.getDetailInfoAsync(item.id, siteUrl, spHttpClient));
                }
                Promise.all(all).then((v: string[]) => {
                    v.forEach(u => {
                        itemsDetail.push(u);
                    });
                    resolve(eMails);
                });
            });
        });
    }
```
