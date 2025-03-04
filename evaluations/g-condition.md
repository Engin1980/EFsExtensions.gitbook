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

This example waits (see `g:for`) for 10 seconds.

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

