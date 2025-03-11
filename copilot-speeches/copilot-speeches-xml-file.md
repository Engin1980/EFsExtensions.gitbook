# Copilot Speeches XML File

## Overview

Copilot Speeches XML file contains definitions of copilot announcements together with triggers used to their automatic invocation.

There is no simple editor to create your own file. You will have to learn the [XML](../shared/xml-and-xsd-brief-explanation.md) syntax and create the XML file on your own. The following text describes the Copilot Speeches XML file content.

{% hint style="info" %}
Note that the validity of the checklist file can be verified against `CopilotSchema.xsd` file. Here you can also find the precise XML file definition.
{% endhint %}

## Basic XML Structure

The file defines root element `copilotSet` consisting of:

* `meta` - see [Meta tag](https://app.gitbook.com/o/eFbPL0xlHGQcXfYjB0ap/s/PdAIOMnAtMPtfMfndwpj/~/changes/25/shared/xml-meta-information) explanation,
* `properties` - to define custom airplane [FS SimVar](https://app.gitbook.com/o/eFbPL0xlHGQcXfYjB0ap/s/PdAIOMnAtMPtfMfndwpj/~/changes/25/shared/fs-simvar-explanation) properties,
* `speechDefinition` - (repeated) to define speech definition(s).

The example of brief XML file is:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<copilotSet
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns="http://github.com/EFS-Extensions/xmls/copilots.xsd"
  xmlns:g="http://github.com/EFS-Extensions/xmls/global.xsd"
  xsi:noNamespaceSchemaLocation="file://./Xsds/CopilotSchema.xsd">
  <metaInfo>
    ...
  </metaInfo>
  <properties title="custom">
  </properties>
  <speechDefinition title="Positive rate">
    ...
  </speechDefinition>
  <speechDefinition title="Acceleration announcement (100)kts">
    ...
  </speechDefinition>  
</copilotSet>
```

Futhermore, every `speechDefinition` consists of:

* `speech` defining the announcement,
* `trigger` defining condition, when speech is invoked,
* `reactivationTrigger` defines condition, when speech trigger can be again evaluated,
* `variables` to define custom variables.

A simple example:

```xml
 ...
   <speechDefinition title="Acceleration announcement (100)kts">
    <speech type="speech" value="{speed} knots" />
    <trigger>
      <g:and>
        <g:property name="ias" direction="passingUp" expression="{speed}"/>
        <g:property name="ias" isTrendBased="true" direction="above" expression="0"/>
      </g:and>
    </trigger>
    <reactivationTrigger>
      <g:property name="ias" direction="below" expression="10" />
    </reactivationTrigger>
    <variables>
      <g:userVariable name="speed" defaultValue="100"/>
    </variables>
  </speechDefinition> 
...
```

## XML Element Explanation

### copilotSet

The element servers as a root element and has no XML attributes.

### meta

The description of the meta tag is explained [here](../shared/xml-meta-information.md).

### properties <a href="#properties" id="properties"></a>

In this section, you can define your own custom [properties](https://app.gitbook.com/o/eFbPL0xlHGQcXfYjB0ap/s/PdAIOMnAtMPtfMfndwpj/~/changes/25/evaluations/g-property) used by your plane, which should be then linked to the trigger condition(s). The format is defined [here](https://app.gitbook.com/o/eFbPL0xlHGQcXfYjB0ap/s/PdAIOMnAtMPtfMfndwpj/~/changes/25/evaluations/g-property).

Note that some properties are already defined by default (see link above). Use this section only if your plane is using some custom property, like `L:A32NX_PARK_BRAKE_LEVER_POS`.

Example:

```xml
...
<properties title="FBW Properties">
  <g:property name="fbwParkingBrake" simVar="L:A32NX_PARK_BRAKE_LEVER_POS"/>
</properties>
...
```

{% hint style="warning" %}
If you create duplicit property declaration, the Copilot Speeches module will not start.
{% endhint %}

### speechDefinition

The speech definition has one XML attribute:

* `title` - used to entitle the speech.

The elements in this element are:

* `speech` - mandatory, representing the announcement. For more detail info see [here](../shared/g-speech.md).
* `trigger`- defines condition; when the condition is true, the speech is announced and the further trigger checks are disabled. The format of the condition is explained [here](../evaluations/g-condition.md).
* `reactivationTrigger` - defines the condition; when the condition is true, the `trigger` evaluation is enabled (again). The format of the condition is explained [here](../evaluations/g-condition.md).
* `variables` (optional) consisting of:
  * `variable` - (repetitive) defining the variable(s) usable in the triggers or speech. The format is explained [here](../evaluations/g-variables.md).

## Examples

```xml
<?xml version="1.0" encoding="utf-8" ?>
<copilotSet
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns="http://github.com/EFS-Extensions/xmls/copilots.xsd"
  xmlns:g="http://github.com/EFS-Extensions/xmls/global.xsd"
  xsi:noNamespaceSchemaLocation="file://./Xsds/CopilotSchema.xsd">
  <metaInfo>
    <g:label>Common IFS copilot callouts</g:label>
    <g:author>Marek Vajgl</g:author>
    <g:description></g:description>
    <g:version>0.1</g:version>
  </metaInfo>
  <properties title="custom">
  </properties>
  <speechDefinition title="Positive rate">
    <speech type="speech" value="Positive rate" />
    <trigger>
      <g:and>
        <g:property name="height" direction="above" expression="30" />
        <g:property name="vs" direction="above" expression="100" />
        <g:property name="ias" direction="above" expression="40" />
      </g:and>
    </trigger>
    <reactivationTrigger>
      <g:for seconds="10">
        <g:property name="height" direction="below" expression="1" />
      </g:for>
    </reactivationTrigger>
  </speechDefinition>
  <speechDefinition title="Acceleration announcement (100)kts">
    <speech type="speech" value="{speed} knots" />
    <trigger>
      <g:and>
        <g:property name="ias" direction="passingUp" expression="{speed}"/>
        <g:property name="ias" isTrendBased="true" direction="above" expression="0"/>
      </g:and>
    </trigger>
    <reactivationTrigger>
      <g:property name="ias" direction="below" expression="10" />
    </reactivationTrigger>
    <variables>
      <g:userVariable name="speed" defaultValue="100"/>
    </variables>
  </speechDefinition>  
</copilotSet>
```

