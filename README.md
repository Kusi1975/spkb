# Kusi's Knowledgebase

[Kusi's Knowledgebase](https://kusi1975.github.io/spkb/)

## MkDocs

### Installation

[Install Python](https://www.python.org/downloads)

[Install MkDocs](https://www.mkdocs.org/getting-started)

[Install Material for MkDocs](https://squidfunk.github.io/mkdocs-material/getting-started)

```cmd
py -m pip install --upgrade pip
SET PATH=%PATH%;C:\Users\<User>\AppData\Local\Programs\Python\Python311\Scripts
pip install mkdocs
pip install mkdocs-material
```

### Edit

The Page is available on <http://127.0.0.1:8000/>

```cmd
mkdocs serve
```

### Publish

The Page is available on:

<pre>
https://<b>&lt;gitname&gt;</b>.github.io<b>/&lt;repo&gt;</b>
</pre>

```cmd
mkdocs gh-deploy
```
