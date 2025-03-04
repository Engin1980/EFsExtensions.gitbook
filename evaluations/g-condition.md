# g:Condition

## Overview

Conditions are used to evaluate with respect to airplane current state and variables. They return true/false result. With respect to this result, some action is typcially invoked (like checklist is started, copilot speech is invoked or failure is invoked).

{% hint style="info" %}
Only for programmers: In Appl implementation, the conditions refers to _StateModel_ and _StateCheckCondition._
{% endhint %}

There are several predefined conditions (all of them will be explained in more detail later):

* true - to represent always `true`,
* false - to represent always `false`,
* and - to do logical AND over all nested conditions,
* or - to do logical OR over all nested conditions,
* for - returns `true` if nested condition is `true` for predefined number of seconds,
* wait - holds evaluation for a while,
* property - returns true/false with respect to the current property value.

{% hint style="info" %}
Logical AND means that **all** nested properties must be true.&#x20;

Logical OR means that **at least one** nested property must be true.
{% endhint %}

Examples:

```
property(alt, >5000)
```

... returns true if plane altitude is over 5000.

```
and
  property(alt, >5000)
  property(ias, >250)
```

.. returns true if plane altitude is over 5000 **and also** plane indicated speed is greater than 250.

```
for(5)
  or
    property(alt, >5000)
    property(ias, >250)
```

... returns true if if plane altitude is over 5000 **or** plane indicated speed is greater than 250 **for 5 seconds in a sequence**.

By combining all those condition types you can create your custom condition.

## XML

Here, the XML syntax of conditions and their precise meaning will be explained.

{% hint style="info" %}
Note that many conditions introduced below can be chained with [g:variables](g-variables.md).

In many cases, in xml attribute value the exact numerical value (like `5000`) can be replaced with variable name in braces (like `{speed}`) - this means that there must exist a variable named `speed` and its current value will be used.
{% endhint %}

{% hint style="info" %}
Note that the same goal can be achieved using different combination of conditions - like g:wait can be replaced with the combination of g:for+g:true.
{% endhint %}

### g:true

This condition returns always `true`. It is typically used together encapsulated by other properties, or for debugging purposes.

This XML element has no xml attributes and has a simple syntax:

```xml
<g:true />
```

The example of usage:

```xml
<g:for seconds="10">
  <g:true />
</g:for>
```

This example waits for 10 seconds (see `g:for`).

### g:false

This condition returns always `false`. It is typically used for debugging purposes only.

This XML element has no xml attributes and has a simple syntax:

```xml
<g:false />
```

### g:and

This condition returns `true` if all nested conditions returns `true`. The XML element **must always** contain nested conditions.

The syntax is:

```xml
<g:and>
  <g:...at elast one nested condition is mandatory ... />
  <g:...other nested condition />
  <g:...another nested condition />
  ...
</g:and>
```

An xample:

```xml
<g:and>
 <g:property name="engine1Running" direction="above" expression="0" />
 <g:property name="engine2Running" direction="above" expression="0" />
</g:and>
```

This condition returns true if both engines are running (their property value is greater than 0).

### g:or

This condition returns `true` if at least one nested conditions returns `true`. The XML element **must always** contain nested conditions.

The syntax is:

```xml
<g:or>
  <g:...at elast one nested condition is mandatory ... />
  <g:...other nested condition />
  <g:...another nested condition />
  ...
</g:or>
```

An xample:

```xml
<g:or>
 <g:property name="engine1Running" direction="above" expression="0" />
 <g:property name="engine2Running" direction="above" expression="0" />
</g:or>
```

This condition returns true if at least one engine is running (its property value is greater than 0).

### g:for

This condition returns `true` if the nested condtion returns `true` for predefined number of seconds in sequence. The sequence must be interrupted - that means if the nested property returns `false`, the counting starts again from 0.

The syntax is:

```xml
<g:for seconds="5">
  <g:... exactly one nested condition is mandatory ... />
</g:for>
```

... where `seconds` attribute defines integer positive value representing the number of attempts (=seconds) how many times the nested condition must be true to return `true`.

An example:

```xml
<g:for seconds="10">
  <g:property name="gs" direction="above" expression="15"/>
</g:for>
```

This condition returns `true` if airlane's ground speed is above 15 kts for 10 seconds in the sequence.

### g:wait

This condition pauses the evaluation of the nested condition for the predefined amount of seconds.  Before the interval is expired, returns `false`. When the interval is expired, returns the value of the nested condition.

The condition is used to suppress the evaluation for preset amount of seconds.

The syntax is:

```xml
<g:wait seconds="5">
  <g:... exactly one nested mandatory condition />
</g:wait>
```

... where `seconds` attribute represents the number of seconds to ignore the nested condition and return `false`. After that, the nested condition result is returned.

An example:

```xml
<g:wait seconds="5">
  <g:property name="ias" direction="above" expression="40" />
</g:wait>
```

This example returns `false` for 5 seconds. Then, returns `true` if airplane indicated speed is above 40 knots, `false` otherwise.

### g:property

{% hint style="warning" %}
Note that [property declaration](g-property.md) also uses `g:property`. But in [property declaration](g-property.md), `g:property` refers to the **FS property declaration.** Here, by `g:property` we refer to the **property-based condition**.
{% endhint %}

This condition is evaluated with respect to the airplane's current status. It is linked to the airplane property and its value.

The syntax is:

```xml
<g:property 
  name="propName" 
  isTrendBased="false"
  direction="above" 
  expression="eng1Run" 
  randomness="+-100"
  sensitivity="-200" />
```

The xml properties are:

* **name** - represents the property name. The property name must match name of some predefined properties. TODO link to properties list. Mandatory.
* **direction** - represents, how is the expression evaluated. For list of available values see the table below. Mandatory.
* **expression -** represents the value to which the property is evaluated. Is a numerical value. Mandatory.
* **isTrendBased** - setting this flat `true`means that not the direct property value is taken, but difference between previous and current value is taken. For example, if previous IAS was 45 and the current IAS is 50, the property value will be 5. If this flag is `false` (what is by default), the direct property value is taken. Optional, default value is `false`.
* **randomness** - used to add a random number to the expression. For example, if `expression=500` and `randomness=+-50`, the real expression value will be random value between `450..550`. This attribute is used to add some randomness into the behavior. For exact format see below.  Optional, default value is `0`.
* **sensitivity** - is used to evaluate value when **direction** is to `exactly`. For example, if `expression=500`,  `direction=exactly`, `sensitivity=+50` all property values between `500..550` will return `true`. With `sensitivity=0` (by default), only property value exactly equal to `500` will return `true`.

The **direction** xml attribute has available values:

<table><thead><tr><th width="163">Value</th><th>Explanation</th></tr></thead><tbody><tr><td>above</td><td>True if expression value is greater than property value.</td></tr><tr><td>below</td><td>True if expression value is smaller than property value.</td></tr><tr><td>exactly</td><td>True if expression value is (moreless) same as property value. Note to use meaningful sensitivity here as it is almost impossible the property value be the same as expression. E.g., expression can be 50 kts, but property value is something like 50.07501238.</td></tr><tr><td>passing</td><td><p>True if:</p><ul><li>property value was below the expression value and is now greater than the expression value, or</li><li>property value was above the expression value and is now lower than expression value.</li></ul></td></tr><tr><td>passingUp</td><td>True if property value was below the expression value and is now greater than the expression value.</td></tr><tr><td>passingDown</td><td>True if property value was above the expression value and is now lower than expression value.</td></tr></tbody></table>

The **randomness**  and **sensitivity** format is:

* leading sign: `+`, `-`, `+-`, or `-+`
* integer number value.

Examples: `+-50`, `+50`, `-50`, `-+50`.

Note that many xml attributes are optional, so commonly the property is written in much shorter way than above, see examples below.

#### Examples

```xml
<g:property name="fbwParkingBrake" direction="above" expression="0" />
<!-- true if the parking brake is set (value is 1, what is above 0) -->
```

```xml
<g:property name="beaconLight" direction="passingUp" expression="0.5"/>
<!-- true if beacon light was 0 and is now 1 (was switched on and passed up 0.5 -->
```

<pre class="language-xml"><code class="lang-xml"><strong>&#x3C;g:property name="alt" direction="passing" expression="10000" 
</strong>  randomness="+-300" />
&#x3C;!-- true if plane altitude is passing FL100 (up or down). Note that the 
expression will randomize the value in range 9700 .. 10300, so the property 
will not be true exactly at 10000 ft, but somewhere in this range. -->
</code></pre>

```xml
<g:property name="alt" direction="passing" expression="10000" 
  sensitivity="+-300" />
<!-- true if plane altitude is passing FL100 (up or down). Note that the 
expression sensitivity is 9700 to 10300 ft, so the property 
will be true when passing UP at 9700 ft and truen when passing DOWN at 10300. -->
```

```xml
<g:and>
  <g:property name="ias" direction="passingUp" expression="{speed}"/>
  <g:property name="ias" isTrendBased="true" direction="above" expression="0"/>
</g:and>
<!-- true if both followings are true:
 * ias is passing up the value of {speed} variable. E.g., if {speed}=20, 
   will be true if ias is rising over 20 kts
 * ias trend is positive (above 0). That is, if difference between the current and 
   previous ias is greater than 0.
   
Simply say, this condition si true if plane is accelerating over preset {speed}.
-->
```

```xml
<g:and>
  <g:property name="alt" direction="below" expression="{alt}"/>
  <g:property name="vs" direction="below" expression="900"/>
</g:and>
<!-- true if both followings are true:
  * Altitude is below value of variable {alt}
  * Vertical speed is below 900 ft/min
  
Simply say, this condition is true if plane is descending over preset {alt}.
-->
```
