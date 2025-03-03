---
description: >-
  This page describes a creation of a new checklist or adjustment of the
  existing one.
---

# Checklists

[#meta](shared-items.md#meta "mention")[shared-items.md](shared-items.md "mention")Checklists are provided as XML file with a specific structure.&#x20;

## Minimal Checklist File

An example of an empty checklist file follows:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<checklistSet
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns="http://chlaot.org/checklists.xsd"
  xmlns:g="http://chlaot.org/global.xsd"
  xsi:noNamespaceSchemaLocation="file://./Xsds/ChecklistSchema.xsd"
  firstChecklistId="batteryOn">
  
</checklistSet>
```

##

## Checklist File Content

Checklist file contains of an expected content:

<pre class="language-xml"><code class="lang-xml"><strong>&#x3C;?xml version="1.0" encoding="utf-8" ?>
</strong>&#x3C;checklistSet
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns="http://chlaot.org/checklists.xsd"
  xmlns:g="http://chlaot.org/global.xsd"
  xsi:noNamespaceSchemaLocation="file://./Xsds/ChecklistSchema.xsd"
  firstChecklistId="batteryOn">
  &#x3C;meta>
    ...
  &#x3C;/meta>
  &#x3C;properties title="FBW Properties">
    &#x3C;g:property name="fbwParkingBrake" simVar="L:A32NX_PARK_BRAKE_LEVER_POS"/>
    ...
  &#x3C;/properties>
  &#x3C;checklist id="beforeStart" callSpeech="Before Start-up">
    &#x3C;items>
      &#x3C;item>
        &#x3C;call type="speech" value="Parking Brake"/>
        &#x3C;confirmation type="speech" value="Set"/>
      &#x3C;/item>
      &#x3C;item>
        ...
      &#x3C;/item>
      ...
    &#x3C;/items>
    &#x3C;trigger>
      &#x3C;g:and>
        &#x3C;g:property name="fbwParkingBrake" direction="above" expression="0" />
        ...
      &#x3C;/g:and>
    &#x3C;/trigger>
  &#x3C;/checklist>
  &#x3C;checklist id="inTaxi" callSpeech="In Taxi">
    ...
  &#x3C;/checklist>
  ...
&#x3C;/checklistSet>

</code></pre>

The top element is `checklistSet`. It may have the following attributes:

<table><thead><tr><th width="173">Id</th><th width="95">Type</th><th>Explanation</th></tr></thead><tbody><tr><td>firstChecklistId</td><td>Text</td><td>Id of the first checklist started in the sequence. Optional. If not used, the first defined checklist is used.</td></tr></tbody></table>

It may have the following elements:

<table><thead><tr><th width="172">Id</th><th>Explanation</th></tr></thead><tbody><tr><td>Meta</td><td>Meta-info about the current file.</td></tr><tr><td>Properties</td><td>A custom defined properties for the current checklist. Contains a set of <em>Property</em> elements.</td></tr><tr><td>Checklists</td><td>A set fo defined checklists. Contains a set of <em>Checklist</em> elements.</td></tr></tbody></table>

### Meta

Meta section contains generic information about the file. For more info how `Meta` section is constructed see _Shared Items - Meta_ page.

### Properties

This section contains defined properties for this file. For more info how properties are used see _Properties Usage_ page. For more info how properties are defined see _Shared items - Properties_ page.

### Checklist

Checklist represents one checklist in the file. Every checklist must have _id_, _call-speech_ defining when the check-list is used, _items_ and optionally _a trigger_, which causes automatic checklist reading invocation.&#x20;

Checklist attributes:

<table><thead><tr><th width="170">Name</th><th width="78">Type</th><th>Explanation</th></tr></thead><tbody><tr><td>id</td><td>Text</td><td>Unique identifier of the checklist. Is used for further checklist referencing.</td></tr><tr><td>callSpeech</td><td>Text</td><td>Call speech used to label and title the checklist.</td></tr><tr><td>beginCallFile</td><td>Text</td><td>Speech played before checklist start. Optional. If not set, default speech based on <code>callSpeech</code> is used.</td></tr><tr><td>endCallFile</td><td>Text</td><td>Speech played when checklist ends. Optional. If not set, default speech based on <code>callSpeech</code> is used.</td></tr><tr><td>nextChecklistIds</td><td>Text</td><td>Id's (<code>id</code>) delimited by <code>;</code> of checklists, which became active once this checklist is completed.</td></tr></tbody></table>
