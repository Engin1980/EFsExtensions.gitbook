# g:Property

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

{% hint style="info" %}
Note that [g:condition](g-condition.md) also uses `g:property`. But in [g:condition](g-condition.md), `g:property` refers to the **property condition**. Here, by `g:property` we refer to the FS **property declaration**.
{% endhint %}

Properties are declared:

* by default - the App have some predefined set of properties (see below)
* custom in XML file - checklists, copilot and other modules can define their own custom properties if necessary.

## Predefined properties

In the App, there are several predefined properties:

**Plane props**

<table><thead><tr><th width="132">Name</th><th width="141">Description</th><th width="347">SimVar</th><th>Unit</th></tr></thead><tbody><tr><td>alt </td><td>Altitude</td><td>PLANE ALTITUDE</td><td>foot</td></tr><tr><td>height</td><td></td><td>PLANE ALT ABOVE GROUND</td><td>foot</td></tr><tr><td>ias</td><td></td><td>AIRSPEED INDICATED</td><td>knot</td></tr><tr><td>gs</td><td></td><td>GROUND VELOCITY</td><td>knot</td></tr><tr><td>vs</td><td>Vertical speed</td><td>VERTICAL SPEED</td><td>feet/minute</td></tr><tr><td>bankAngle</td><td></td><td>PLANE BANK DEGREES</td><td>degree</td></tr></tbody></table>

**Plane props**

<table><thead><tr><th width="132">Name</th><th width="141">Description</th><th width="347">SimVar</th><th>Unit</th></tr></thead><tbody><tr><td>alt </td><td>Altitude</td><td>PLANE ALTITUDE</td><td>foot</td></tr><tr><td>height</td><td></td><td>PLANE ALT ABOVE GROUND</td><td>foot</td></tr><tr><td>ias</td><td></td><td>AIRSPEED INDICATED</td><td>knot</td></tr><tr><td>gs</td><td></td><td>GROUND VELOCITY</td><td>knot</td></tr><tr><td>vs</td><td>Vertical speed</td><td>VERTICAL SPEED</td><td>feet/minute</td></tr><tr><td>bankAngle</td><td></td><td>PLANE BANK DEGREES</td><td>degree</td></tr></tbody></table>

**Plane props**

<table><thead><tr><th width="132">Name</th><th width="141">Description</th><th width="347">SimVar</th><th>Unit</th></tr></thead><tbody><tr><td>alt </td><td>Altitude</td><td>PLANE ALTITUDE</td><td>foot</td></tr><tr><td>height</td><td></td><td>PLANE ALT ABOVE GROUND</td><td>foot</td></tr><tr><td>ias</td><td></td><td>AIRSPEED INDICATED</td><td>knot</td></tr><tr><td>gs</td><td></td><td>GROUND VELOCITY</td><td>knot</td></tr><tr><td>vs</td><td>Vertical speed</td><td>VERTICAL SPEED</td><td>feet/minute</td></tr><tr><td>bankAngle</td><td></td><td>PLANE BANK DEGREES</td><td>degree</td></tr></tbody></table>



alt - Altitude" simVar="PLANE ALTITUDE" unit="foot" /> \<g:property name="height" simVar="PLANE ALT ABOVE GROUND" unit="foot" /> \<g:property name="ias" simVar="AIRSPEED INDICATED" unit="knot" /> \<g:property name="gs" simVar="GROUND VELOCITY" unit="knot" /> \<g:property name="vs" description="Vertical speed" simVar="VERTICAL SPEED" unit="feet/minute"/> \<g:property name="bankAngle" simVar="PLANE BANK DEGREES" unit="degree" /> \</g:properties> \<g:properties title="Plane lights"> \<g:property name="beaconLight" simVar="LIGHT BEACON" /> \<g:property name="strobeLight" simVar="LIGHT STROBE" /> \<g:property name="landingLights" simVar="LIGHT LANDING" /> \</g:properties> \<g:properties title="Plane Systems"> \<g:property name="engine1Running" simVar="ENG COMBUSTION:1" /> \<g:property name="engine2Running" simVar="ENG COMBUSTION:2" /> \<g:property name="engine3Running" simVar="ENG COMBUSTION:3" /> \<g:property name="engine4Running" simVar="ENG COMBUSTION:4" /> \</g:properties> \<g:properties title="Plane attitude"> \<g:property name="parkingBrakeSet" simVar="BRAKE PARKING POSITION"/> \<g:property name="pushbackTugConnected" simVar="PUSHBACK ATTACHED"/> \</g:properties> \<g:properties title="Plane orientation"> \<g:property name="acceleration" simVar="ACCELERATION BODY Z" unit="knot" /> \</g:properties>
