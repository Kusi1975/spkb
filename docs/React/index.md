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
