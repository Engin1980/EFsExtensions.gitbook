# Checklist XML File

## Overview

Checklist XML file defines all the checklists, their items, triggers and relations. The file is written in the XML format.

There is no simple editor to create your own checklist file. You will have to learn the XML syntax and create the XML file with checklists on your own. The following text describes the XML file content.

{% hint style="info" %}
Note that the validity of the checklist file can be verified against \`[ChecklistSchema.xsd](https://github.com/Engin1980/ChlaotSolution/blob/master/Xmls/Xsds/ChecklistSchema.xsd)\` file.
{% endhint %}

For triggers, you must be also familiar with FS SimVars & L:Vars.

## Basic XML content

The checklist XML file consists of three sections:

* `meta` - see [Meta tag](../shared/xml-meta-information.md) explanation,
* `properties` - to define custom airplane [FS SimVar](../shared/fs-simvar-explanation.md) properties,
* `checklist` - (repeated) to define checklist(s).

Every `checklist` consists of:

* some xml attributes describing its settings and behavior,
* `items` - element containing
  * `item` - representing one checklist item to check, consisting of:
    * `call` - the callout (or label) of the item, like "Landing Gear", and
    * `confirmation` - the callout of the confirmation, like "Down, three green".
* `variables` - element containing custom variables to be set for checklist evaluation (TODO link)
* `trigger` - containing the trigger on which the reading of the checklist should be automatically invoked.

### Meta

The description of the meta tag is explained [here](../shared/xml-meta-information.md).

### Properties

In this section, you can define your own custom L:Vars used by your plane, which should be then linked to the checklist trigger condition. The section contains a list of g-properties (TODO link).&#x20;

Note that the FS default L:vars (properties) are already defined by default. Use this section only if your plane is using some custom property, like `L:A32NX_PARK_BRAKE_LEVER_POS`. If you create duplicit property declaration, the checklist module will not start.

### Checklist

The checklist xml-element has following xml attributes:



<table><thead><tr><th width="174">Name</th><th width="165">Domain</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>Text</td><td>Mandatory. Defines the "id" of the checklist used for referencing later. Also, if "callSpeech" is not defined, us used as the checklist call speech (see below).</td></tr><tr><td>callSpeech</td><td>Text</td><td>Optional. Used as a callout of checklist name. Can be used, if the "id" is shortened. E.g., id=lnup, callSpeech=line up. If not used, "id" is taken.</td></tr><tr><td>nextChecklistIds</td><td>Text;Text;...</td><td>Optional. Used to define, which checklist(s) follow(s) after this one. Multiple can be selected, the the first one is default (e.g., after approach, there can be landing or go-around). If not defined, the following checklist in the XML file is taken. If not defined and is the last, the first checklist is taken as the next one.</td></tr></tbody></table>

The checklist xml-element has following xml elements:

|                         |                                                                                                                                                             |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| items                   | Mandatory. Contain checklist items as a sequence of "item" xml-element(s).                                                                                  |
| variables               | Optional. Contains checklist variables a sequence of "variable" xml element(s).                                                                             |
| trigger                 | Optional. Defining automated checklist trigger. Once the checklist is the current one and the trigger is true, the checklist will be automatically started. |
| customEntrySpeech       | Optional. Custom speech used at the beginning of the checklist.                                                                                             |
| customExitSpeech        | Optional. Custom speech used at the end of the checklist.                                                                                                   |
| customPausedAlertSpeech | Optional. Custom speech used as a remined after predefined interval if the checklist was paused.                                                            |



## Minimal Checklist File

An example of an empty checklist file follows:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<checklistSet
 xmlns="http://chlaot.org/checklists.xsd"
  xmlns:g="http://chlaot.org/global.xsd"
  xsi:noNamespaceSchemaLocation="file://./Xsds/ChecklistSchema.xsd"
  firstChecklistId="batteryOn">
  
</checklistSet>
```

## Checklist File Content

Checklist file contains of an expected content:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<checklistSet
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns="http://chlaot.org/checklists.xsd"
  xmlns:g="http://chlaot.org/global.xsd"
  xsi:noNamespaceSchemaLocation="file://./Xsds/ChecklistSchema.xsd"
  firstChecklistId="batteryOn">
  <meta>
    ...
  </meta>
  <properties title="FBW Properties">
    <g:property name="fbwParkingBrake" simVar="L:A32NX_PARK_BRAKE_LEVER_POS"/>
    ...
  </properties>
  <checklist id="beforeStart" callSpeech="Before Start-up">
    <items>
      <item>
        <call type="speech" value="Parking Brake"/>
        <confirmation type="speech" value="Set"/>
      </item>
      <item>
        ...
      </item>
      ...
    </items>
    <trigger>
      <g:and>
        <g:property name="fbwParkingBrake" direction="above" expression="0" />
        ...
      </g:and>
    </trigger>
  </checklist>
  <checklist id="inTaxi" callSpeech="In Taxi">
    ...
  </checklist>
  ...
</checklistSet>

```
