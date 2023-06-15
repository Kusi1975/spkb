# Kusi's Knowledge Base

## PowerPlatform Flow

### Send Mail on add item depend on a lookup value from a Dataverse table

- Add Action **Add a new row Dataverse**

  |Property   |Value            |
  |-----------|-----------------|
  |Environment|(Current)        |
  |Table name |&lt;TableName&gt;|
  |Scope      |Organization     |

- Add Action **Get a row by ID Dataverse**

  |Property  |Value              |
  |----------|-------------------|
  |Table name|&lt;LookupTable&gt;|
  |Row ID    |&lt;TableName&gt;  |

- Add Action **Compose**

  |Property|Value                  |
  |--------|-----------------------|
  |Input   |&lt;LookupFieldName&gt;|

- Add Action **Send an email (v2) Outlook**

  |Property|Value  |
  |--------|-------|
  |To      |Outputs|

