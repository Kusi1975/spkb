# Kusi's Knowledge Base

## PowerPlatform

### Learning

[App in a day](https://aka.ms/appinaday)

### Power Fx Examples

#### Show another Canvas

On Button the OnSelect property

```cs
Navigate(<CanvasName>)
```

#### Update item with specific values

On Button the OnSelect property

```cs
Patch(<DataSource>, <GalleryName>.Selected,{<Field1>:<Value1>,<Field2>:<Value2>});Refresh(<DataSource>)
```

#### Save Form

On Button the OnSelect property

```cs
SubmitForm(<FormName>)
```

#### Remove selected item

On Button the OnSelect property

```cs
Remove(<DataSource>, <GalleryName>.Selected);Refresh(<DataSource>)
```

#### Display labels from Multi TermField

On Label the Text property

```cs
Concat(ThisItem.<TermField>, Label, ";")
```

#### Highlight selected Entry in Gallery in TemplateFill property

On Gallery the OnSelect property


```cs
If(ThisItem.IsSelected, RGBA(128, 128, 128, 0.2),  RGBA(0, 0, 0, 0))
```

#### New Form

On Canvas the OnVisible property

```cs
NewForm(<EditFormName>)
```

On EditForm the DefaultMode property

```cs
FormMode.New
```
