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

## Caching

Save object in cache
```typescript
localStorage.setItem(storageKey, object);
```

Get object from cache
```typescript
const object = localStorage.getItem(storageKey);
```

## Interval

```typescript
private interval: number = undefined;

public componentDidMount(): void {
    if (!this.interval) {
        this.interval = setInterval(() => {
            ...
        }, 1000);
    }
}

public componentWillUnmount(): void {
    if (this.interval) {
        clearInterval(this.interval);
    }
}
```