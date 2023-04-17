# Kusi's Knowledge Base

## PowerPlatform

### Power Fx Examples

#### Update item OnSelect event

```cs
Patch(<DataSource>, <GalleryName>.Selected,{<Field1>:<Value1>,<Field2>:<Value2>});Refresh(<DataSource>)
```

#### Display labels from Multi TermField

```cs
Concat(ThisItem.<TermField>, Label, ";")
```
