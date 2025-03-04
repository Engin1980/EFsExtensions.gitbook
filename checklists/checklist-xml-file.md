# Checklist XML File

## Overview

Checklist XML file defines all the checklists, their items, triggers and relations. The file is written in the XML format.

There is no simple editor to create your own checklist file. You will have to learn the [XML syntax](../shared/xml-and-xsd-brief-explanation.md) and create the XML file with checklists on your own. The following text describes the checklist XML file content.

{% hint style="info" %}
Note that the validity of the checklist file can be verified against \`[ChecklistSchema.xsd](https://github.com/Engin1980/ChlaotSolution/blob/master/Xmls/Xsds/ChecklistSchema.xsd)\` file. Here you can also find the precise XML file definition.
{% endhint %}

For triggers, you must be also familiar with [g:Conditions](../evaluations/g-condition.md).

## Basic XML structure

The checklist XML file contains the root element `checklistSet` that contains three sections:

* `meta` - see [Meta tag](../shared/xml-meta-information.md) explanation,
* `properties` - to define custom airplane [FS SimVar](../shared/fs-simvar-explanation.md) properties,
* `checklist` - (repeated) to define checklist(s).

Example of brief content of checklist XML file:

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
    ...
  </properties>
  <checklist id="beforeStart" callSpeech="Before Start-up">
    ...
  </checklist>
  <checklist id="beforeTaxi" callSpeech="Before Taxi">
    ...
  </checklist>
  ...
</checklistSet>
```

Futhrermore, every `checklist` consists of:

* some xml attributes describing its settings and behavior,
* `items` - element containing
  * `item` - representing one checklist item to check, consisting of:
    * `call` - the callout (or label) of the item, like "Landing Gear", and
    * `confirmation` - the callout of the confirmation, like "Down, three green".
* `variables` - element containing custom variables to be set for checklist evaluation (TODO link)
* `trigger` - containing the trigger on which the reading of the checklist should be automatically invoked.

The example of checklist XML file with checklist:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<checklistSet ...>
  ...    
  <checklist id="inTaxi" callSpeech="In Taxi">
    <items>
      <item>
        <call type="speech" value="Packs"/>
        <confirmation type="speech" value="On"/>
      </item>
      <item>
        <call type="speech" value="A P U Bleed"/>
        <confirmation type="speech" value="Off"/>
      </item>
      ...
    </items>
    <variables>
      <g:randomVariable name="delay" minimum="3" maximum="8" isInteger="true"/>
    </variables>
    <trigger>
      <g:and>
        <g:property name="engine1Running" direction="above" expression="0" />
        <g:property name="engine2Running" direction="above" expression="0" />
        <g:for seconds="{delay}">
          <g:property name="gs" direction="above" expression="15"/>
        </g:for>
      </g:and>
    </trigger>
  </checklist>
  ...
</checklistSet>

```

## XML Element explanation

### ChecklistSet

This is the root element in XML. It has one xml attribute:

* **firstChecklistId** - refers to the Id of the first checklist ready after the App run. Optional, if not defined, the first checklist defined in the file will be used.

### Meta

The description of the meta tag is explained [here](../shared/xml-meta-information.md).

### Properties

In this section, you can define your own custom [properties](../evaluations/g-property.md) used by your plane, which should be then linked to the checklist trigger condition. The format is defined [here](../evaluations/g-property.md).&#x20;

Note that some properties are already defined by default (see link above). Use this section only if your plane is using some custom property, like `L:A32NX_PARK_BRAKE_LEVER_POS`.&#x20;

Example:

```xml
...
<properties title="FBW Properties">
  <g:property name="fbwParkingBrake" simVar="L:A32NX_PARK_BRAKE_LEVER_POS"/>
</properties>
...
```

{% hint style="warning" %}
If you create duplicit property declaration, the checklist module will not start.
{% endhint %}

### Checklist

The checklist xml-element has following xml attributes:

<table><thead><tr><th width="174">Name</th><th width="132">Domain</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>Text</td><td>Mandatory. Defines the "id" of the checklist used for referencing later. Also, if "callSpeech" is not defined, us used as the checklist call speech (see below).</td></tr><tr><td>callSpeech</td><td>Text</td><td>Optional. Used as a callout of checklist name. Can be used, if the "id" is shortened. E.g., id=lnup, callSpeech=line up. If not used, "id" is taken.</td></tr><tr><td>nextChecklistIds</td><td>Text;Text;...</td><td>Optional. Used to define, which checklist(s) follow(s) after this one. Multiple can be selected, the the first one is default (e.g., after approach, there can be landing or go-around). If not defined, the following checklist in the XML file is taken. If not defined and is the last, the first checklist is taken as the next one.</td></tr></tbody></table>

The checklist xml-element has following xml elements:

<table><thead><tr><th width="265">Attribute Name</th><th>Description</th></tr></thead><tbody><tr><td>items</td><td>Mandatory. Contain checklist items as a sequence of "item" xml-element(s).</td></tr><tr><td>variables</td><td>Optional. Contains checklist variables a sequence of "variable" xml element(s).</td></tr><tr><td>trigger</td><td>Optional. Defining automated checklist trigger. Once the checklist is the current one and the trigger is true, the checklist will be automatically started.</td></tr><tr><td>customEntrySpeech</td><td>Optional. Custom speech used at the beginning of the checklist.</td></tr><tr><td>customExitSpeech</td><td>Optional. Custom speech used at the end of the checklist.</td></tr><tr><td>customPausedAlertSpeech</td><td>Optional. Custom speech used as a remined after predefined interval if the checklist was paused.</td></tr></tbody></table>

The elements in this element are:

* `<items>` - containing several:
  * `<item>` - containing a pair of:
    * `<call>` - used to define checklist item label
    * `<confirmation>` - used to define checklist item confirmation
* `<variables>`  - containing several:
  * `<variable>` - defining custom checklist variables
* `<trigger>` - defining automatic trigger causing checklist being read, containing a single g:condition.

### Call & Confirmation

## Examples

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
