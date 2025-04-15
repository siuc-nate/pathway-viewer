# Pathway Viewer
A simple viewer for CTDL Pathways.

Basic Usage
---
1. Add the **pathwayviewer.js** file to your page. The script will create a global `PathwayViewer` object.
2. Create a `div` tag to act as the container for the Pathway you want to load.
3. Call `PathwayViewer.loadPathway` and pass the `div` from step 2 and the JSON-LD URL (e.g. the Credential Registry URI) for the pathway.

Basic Example
---
```html
<head>
<script type="text/javascript" src="../src/pathwayviewer.js" onload="setupPathway()" defer async></script>
<script type="text/javascript">
  function setupPathway(){
    PathwayViewer.loadPathway(
      document.querySelector("#somePathwayBox"), 
      "https://credentialengineregistry.org/resources/ce-9dec2bcb-40a4-42e2-8fa3-afc04ddc04aa"
    );
  }
</script>
</head>
<body>
  <div id="somePathwayBox"></div>
</body>
```

Multiple Pathways
---
The Pathway Viewer supports loading and displaying multiple pathways on one page by creating a separate `div` tag for each pathway and passing each one to a separate call to `PathwayViewer.loadPathway`. For example:

```html
<head>
<script type="text/javascript" src="../src/pathwayviewer.js" onload="setupPathway()" defer async></script>
<script type="text/javascript">
  function setupPathway(){
    PathwayViewer.loadPathway(
      document.querySelector("#somePathwayBox1"), 
      "https://credentialengineregistry.org/resources/ce-9dec2bcb-40a4-42e2-8fa3-afc04ddc04aa"
    );
    PathwayViewer.loadPathway(
      document.querySelector("#somePathwayBox2"), 
      "https://credentialengineregistry.org/resources/ce-0f0af1dd-35c7-43e2-9363-6bc079508747"
    );
    PathwayViewer.loadPathway(
      document.querySelector("#somePathwayBox2"), 
      "https://credentialengineregistry.org/resources/ce-164b0c4e-f96e-4597-a5d6-2fcd53635c46"
    );
  }
</script>
</head>
<body>
  <div id="somePathwayBox1"></div>
  <div id="somePathwayBox2"></div>
  <div id="somePathwayBox3"></div>
</body>
```

Configuration
---
A third parameter can be passed to `PathwayViewer.loadPathway` consisting of several options for how pathways get rendered. The structure and defaults are shown below.
```json
{
  "UI": {
    "PathwayHeaderTag": "h2",
    "DefaultComponentProgressionLevelHeaderLabel": { "en-us": "Components" },
    "DefaultDestinationProgressionLevelHeaderLevel": { "en-us": "Destination" },
    "ComponentURILinks": [ 
      { 
        "Label": { "en-us": "View Component in Credential Registry" }, 
        "URIPattern": "{uri}" 
      },
      {
        "Label": { "en-us": "View Component in Credential Finder" },
        "URIPattern": "https://credentialfinder.org/resources/{ctid}"
      }
    ],
    "ComponentProxyForLinks": [
      {
        "Label": { "en-us": "View Resource in Credential Registry" },
        "URIPattern": "{uri}"
      },
      {
        "Label": { "en-us": "View Resource in Credential Finder" },
        "URIPattern": "https://credentialfinder.org/resources/{ctid}"
      }
    ]
  },
  "Language": {
    "MaxCodes": 100,
    "AllowAll": true,
    "PreferredCodes": [
      "en-US",
      "en-us",
      "en"
    ],
    "ValueJoiner": ", "
  }
}
```
- `UI.PathwayHeaderTag`: Controls which HTML tag is used to render the header that displays above the pathway box. Use CSS to hide the header if no header is desired.
- `UI.DefaultComponentProgressionLevelHeaderLabel`: For pathways with one or more Components which do not reference a Progression Level, a default level will be inserted with the label chosen here.
- `UI.DefaultDestinationProgressionLevelHeaderLevel`: For pathways where the destination Component does not reference a Progression Level, a default destination level will be inserted with the label chosen here.
- `UI.ComponentURILinks`: Each Component in the Pathway will display links to itself based on its `@id` with the pattern matching and label used here. Pass an empty array instead to disable these.
- `UI.ComponentProxyForLinks`: Each Component in the Pathway will display links to the Resource(s) for which it is a proxy (if any) with the pattern matching and label used here. Pass an empty array instead to disable these.
- `Language.MaxCodes`: If more than one value is present in any given language map, this will control the maximum number of values (i.e. languages) rendered for that language map.
- `Language.AllowAll`: If `true`, all language values present in all language maps will be rendered. If `false`, only languages present in `Language.PreferredCodes` will be rendered.
- `Language.PreferredCodes`: Array to look for language values in any given language map, in order of preferred language code.
- `Language.ValueJoiner`: String to use when joining language maps where any given language key has more than one value, such as `{ "en": [ "value one", "value two", "value three" ]}`.

To change the configuration, pass an object with the values you want to override as part of the call to `PathwayViewer.loadPathway`. For example:
```html
<head>
<script type="text/javascript" src="../src/pathwayviewer.js" onload="setupPathway()" defer async></script>
<script type="text/javascript">
  function setupPathway(){
    PathwayViewer.loadPathway(
      document.querySelector("#somePathwayBox"), 
      "https://credentialengineregistry.org/resources/ce-9dec2bcb-40a4-42e2-8fa3-afc04ddc04aa",
      {
        UI: {
          PathwayHeaderTag: "h1",
          DefaultComponentProgressionLevelHeaderLabel: { "en-us": "All Components" },
          ComponentURILinks: []
        },
        Language: {
          PreferredCodes: [ "en-gb" ]
        }
      }
    );
  }
</script>
</head>
<body>
  <div id="somePathwayBox"></div>
</body>
```

The configuration will need to be passed for each Pathway on the page where you want to override the default values. This allows having different configurations for each Pathway, if desired. For example:
```html
<head>
  <title>Pathway Viewer Demo</title>
  <script type="text/javascript" src="../src/pathwayviewer.js" onload="setupPathway()" defer async></script>
  <script type="text/javascript">
    function setupPathway(){
      PathwayViewer.loadPathway(
        document.querySelector("#somePathwayBox1"), 
        "https://credentialengineregistry.org/resources/ce-9dec2bcb-40a4-42e2-8fa3-afc04ddc04aa"
      );
      PathwayViewer.loadPathway(
        document.querySelector("#somePathwayBox2"), 
        "https://credentialengineregistry.org/resources/ce-0209da38-3aa8-4e9d-8b51-93e16fc9cb9a",
        {
          UI: {
            PathwayHeaderTag: "h3",
            DefaultComponentProgressionLevelHeaderLabel: { "en-us": "Components Without Level" }
          },
          Language: {
            PreferredCodes: [ "en-gb", "en" ]
          }
        }
      );
      PathwayViewer.loadPathway(
        document.querySelector("#somePathwayBox3"), 
        "https://credentialengineregistry.org/resources/ce-c0ff089f-7f18-4248-9054-45a6e5d2c8b4",
        {
          UI: {
            PathwayHeaderTag: "h1",
            DefaultComponentProgressionLevelHeaderLabel: { "en-us": "All Components" },
            ComponentURILinks: []
          },
          Language: {
            PreferredCodes: [ "en-gb" ]
          }
        }
      );
    }
  </script>
</head>
<body>
  <div id="somePathwayBox1"></div>
  <div id="somePathwayBox2"></div>
  <div id="somePathwayBox3"></div>
</body>
```

Styles and Colors
---
The CSS used with the rendered pathway is intended to be easy to override, to customize the look and feel of the pathways for your site. Most major nodes have CSS classes that include `pathwayViewer` in order to help custom selectors target only those nodes that have to do with rendered items in the Pathway Viewer.