# Checklist File Content

## Minimal Checklist File

An example of an empty checklist file follows:

```
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

```
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

### Element description

**checklistSet**

Id | Type | Explanation
--|--|--
firstChecklistId | Text | Id of the first checklist started in the sequence. Optional. If not used, the first defined checklist is used.


