# g:Property

{% hint style="warning" %}
Note that [g:condition](g-condition.md) also uses `g:property`. But in [g:condition](g-condition.md), `g:property` refers to the **property condition**. Here, by `g:property` we refer to the FS **property declaration**.
{% endhint %}

## Background

When Flight Simulator (FS) is running, it is possible to register to it and it starts send you information about the plane and simulation state. It has a set of predefined variables, calles _SimVars_. Once you register to a specific _SimVar_, FS will provide you information every second (or different time interval) or once the value of the SimVar change.

Look [here to learn more about SimVars](../shared/fs-simvar-explanation.md).

As SimVar readout is quite complex and have many initial parameters, **g:properties** is a concept allowing easy check of the SimVar value in the App.

## Overview

g:Property is used in XML to define a mapping between a property **name** and the SimVar value of the airplane. For example, the following line defines a property named `alt` which will contain current airplane altitude in feet.

```xml
<g:property name="alt" description="Altitude" simVar="PLANE ALTITUDE" unit="foot" />
```

Here:

* **name** - defines the name of the property,
* **description** - describes the property
* **simVar** - defines the entry point in the Flight Simulator via so called SimVars, from where the value is read,
* **unit** - defines the unit of the variable (for example, the altitude can be in meters or feet).

Every property in the App must have unique name. However (although not suggested), properties with different name can be bound to the same `simVar` name.

Properties are declared:

* by default - the App have some predefined set of properties (see below)
* custom in XML file - checklists, copilot and other modules can define their own custom properties if necessary.

## Predefined properties

In the App, there are several predefined properties. You can use any of them in [g:property conditions](g-condition.md) directly without any additional work.&#x20;

**Plane Props**

<table><thead><tr><th width="132">Name</th><th width="141">Description</th><th width="347">SimVar</th><th>Unit</th></tr></thead><tbody><tr><td>alt </td><td>Altitude</td><td>PLANE ALTITUDE</td><td>foot</td></tr><tr><td>height</td><td></td><td>PLANE ALT ABOVE GROUND</td><td>foot</td></tr><tr><td>ias</td><td></td><td>AIRSPEED INDICATED</td><td>knot</td></tr><tr><td>gs</td><td></td><td>GROUND VELOCITY</td><td>knot</td></tr><tr><td>vs</td><td>Vertical speed</td><td>VERTICAL SPEED</td><td>feet/minute</td></tr><tr><td>bankAngle</td><td></td><td>PLANE BANK DEGREES</td><td>degree</td></tr></tbody></table>

**Plane Lights**

<table><thead><tr><th width="206">Name</th><th width="347">SimVar</th></tr></thead><tbody><tr><td>beaconLight</td><td>LIGHT BEACON</td></tr><tr><td>strobeLight</td><td>LIGHT STROBE</td></tr><tr><td>landingLights</td><td>LIGHT LANDING</td></tr></tbody></table>

**Plane Systems**

<table><thead><tr><th width="207">Name</th><th width="347">SimVar</th></tr></thead><tbody><tr><td>engine1Running</td><td>ENG COMBUSTION:1</td></tr><tr><td>engine2Running</td><td>ENG COMBUSTION:2</td></tr><tr><td>engine3Running</td><td>ENG COMBUSTION:3</td></tr><tr><td>engine4Running</td><td>ENG COMBUSTION:4</td></tr></tbody></table>

**Plane Attitude**

<table><thead><tr><th width="253">Name</th><th width="257">SimVar</th><th>Unit</th></tr></thead><tbody><tr><td>parkingBrakeSet</td><td>BRAKE PARKING POSITION</td><td>0 = not set; 1 = set</td></tr><tr><td>pushbackTugConnected</td><td>PUSHBACK ATTACHED</td><td>0 = no; 1 = yes</td></tr></tbody></table>

**Plane Orientation**

| Name         | SimVar              | Unit |
| ------------ | ------------------- | ---- |
| acceleration | ACCELERATION BODY Z | knot |

{% hint style="info" %}
Note that this is not final list and other properties may be added in the future.
{% endhint %}

{% hint style="info" %}
Note that you can adjust the global predefined properties in `SimProperties.xml` file.
{% endhint %}

### Custom Properties

In several files (like checklists, failures or copilot speeches) you can define your own property declaration in the appropriate section. The XML syntax is:

```xml
<g:property 
  name="vs" 
  description="Vertical speed" 
  simVar="VERTICAL SPEED" 
  unit="feet/minute"/>
```

The xml attributes are:

* **name** - global unique name of the property. If the property name is duplicit (together among predefined or all custom properties), the App will crash. Mandatory.
* **description** - simple property explanation or description. Used when property name is too short or confusing. Optional.
* **simVar** - a name of [FS SimVar](../shared/fs-simvar-explanation.md) to which the property is bound to. The SimVar property must be known and exist in FS. Mandatory.
* **unit** - an unit in which the property value is presented. The predefined values must match those defined as valid units in [FS SimVars](../shared/fs-simvar-explanation.md). Optional, if not used, FS SimVar default unit is used.

Examples:

```xml
<g:property name="gs" simVar="GROUND VELOCITY" unit="knot" />
<g:property name="vs" description="Vertical speed" simVar="VERTICAL SPEED" unit="feet/minute"/>
<g:property name="bankAngle" simVar="PLANE BANK DEGREES" unit="degree" />
```
