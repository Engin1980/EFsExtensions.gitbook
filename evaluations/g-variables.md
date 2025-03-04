# G:Properties

## Overview

Variables are named values, which can be used in conditions. Every variable has its unique _name_ and _value_. Value is always numerical.

There are two types of variables:

* **user variable** - where the value is expected to be provided by user, typically using some user interface during initialization of a module, and
* **random variable** - where the value is generated randomly from the predefined range.

User variables are typically used, when the user must specify the correct value during the module initialization. This value is a subject to change and differs on every run. For example, user variables may be used to declare transition altitudes, Vx speeds (V1/Vr/V2), required take off distance, etc.

Random variables are used to bring some randomization and fuzzy behavior into the process. For example, there should be an announcement at FL100, but you want to have always a small deviation so the announcement is not stritly at FL100, but somewhere between FL95 to FL105. Then, random variable is the key.

## XML

To use a variable, they must be declared within the corresponding XML file. To declare the variable in the XML file, you need to have linked the `xsd` namespace in your XML's file header:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<aaa
  ...
  xmlns:g="http://chlaot.org/global.xsd"
>
```

Then, you can use:

* `<g:userVariable ... />`
* `<g:randomVariable ... />`

### g:userVariable

XML element `<g:userVariable>` has the following xml attributes:

* **name** - unique name of the variable. Typically, you can refer to the name later in other declarations using braces, e.g. `{variableName}`. Name must start with a character and then contain characters or digits only. Mandatory.
* **defaultValue** - a default value used if user did not provide any custom value. If not provided and user did not enter her/his custom value, the behavior is unexpected. Optional.
* **description** - a custom description of the variable. Optional.

Example:

```xml
<g:userVariable name="speed" defaultValue="80" description="Speed in kts" />
```

### g:randomVariable

Random variable will have randomly generated value with respect to its properites, from range \<minimum, maximum>.

XML element `<g:randomVariable>` has the following xml attributes:

* **name** - unique name of the variable. Typically, you can refer to the name later in other declarations using braces, e.g. `{variableName}`. Name must start with a character and then contain characters or digits only. Mandatory.
* **minimum** - minimal randomly generated value, inclusive. Mandatory.
* **maximum -** maximal randomly generated value, inclusive. mandatory.
* **isInteger -** true/false flag showing if the random generated value should be integer (true) or decimal (false). Optional, if not set, `false` value is default.
* **description** - a custom description of the variable. Optional.

Example:

```xml
<g:randomVariable name="delay" minimum="3" maximum="8" isInteger="true"/>
```

