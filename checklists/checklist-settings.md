# Checklist Settings

Checklist Settings contains following sections:

* Synthetizer - how the speeches are generated,
* Key Shortcuts - what keys are used to manage the checklist reading,
* Other Settings.

## Synthetizer

Synthetizer is used to generate Checklist speeches if required.

See [Shared/Synthetizer Settings](../shared/synthetizer-settings.md).

## Key Shortcuts

This section defines key shortcuts, which are used to start/pause, skip forward and skip backward actions.

{% hint style="warning" %}
Note that if the keybinding overrides some Windows default binding, the Windows binding will not work until the application is stopped.
{% endhint %}

**Play / Pause**

This action starts reading of the current checklist, or pauses the reading, if it is in the progress. Once paused, on resume the last incomplete item is read again.

The default binding is `Ctrl + X`.

**Skip to next checklist**

This action skips to the following checklist in the sequence and invokes its reading.

The default binding is `Ctrl + Alt + X`.

**Skip to previous checklist**

This action skips to the previous checklist in the sequence and invokes its reading.

The default binding is `Ctrl + Alt + Shift + X`.

## Other Settings

**Read Confirmations**

If checked, the copilot will read checklist label and also its confirmation, like:

```
Landing gear - three green.
Flaps - set.
```

If unchecked, the copilot will read only checklist label, like:

```
Landing gear.
Flaps.
```

**Alert on paused checklist**

If checked, the app detects the paused checklist and reminds it in the predefined interval using the predefined speech, like:

```
Taxi checklist is pending.
```

**Use Autoplay**

If checked, the checklists are automatically started once their trigger condition is true. For example, the "runway line-up checklist" can be invoked automatically when "landing lights" or "strobe lights" are set "on".

If unchecked, you must invoke checklist reading manually.
