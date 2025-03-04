# g:Speech

TODO

## Overview

g:Speech represents a single speech in the App.

## XML

The speech contains two xml attributes:

* **type** - representing the type of value - `speech` or `file`, and
* **value** - representing the value. If the type is `speech`, then value is a text string to be spoken; if type is `file`, then value is a relative link to the `mp3` or `wav` file to be played.

Examples (note the element name depends on the context, here from checklists):

```xml
<call type="speech" value="A P U Bleed"/>
<confirmation type="file" value=".\snd\on.wav"/>
```
