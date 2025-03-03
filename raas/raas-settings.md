---
description: This page describes RaaS settings and their meaning.
---

# RaaS Settings

RaaS Settings contains following sections:

* Synthetizer - how the speeches are generated,
* Files - what should be pre-loaded on startup,
* Thresholds - how the module should be evaluated in runtime.

## Synthetizer

Synthetizer is used to generate RaaS announcements if required.

See [Shared/Synthetizer Settings](../shared/synthetizer-settings.md).

## Files

To get rid of repeated file loading, this settings can set-up default files loaded automatically on startup.

**Default Airports File**

This file is supposed to contain information about airports and their runways.  For more information, see TODO.

If the file is specified, it will be automatically loaded on module init.

**Default RaaS File**

This file defines the basic RaaS speeches and their properties. For more information, see TODO.

If the file is specified, it will be automatically loaded on module init.

## Thresholds

{% hint style="warning" %}
A term **ortho-distance** is mentioned in the following text. The term _ortho-distance_ refers to the distance of the aircraft from the runway axis - i.e. the minimum distance of the aircraft from a parallel line passing through the runway axis.
{% endhint %}

{% hint style="warning" %}
A term _height_ is mentioned in the following test. This term refres to the airplane height above the ground level. Note that for some models, this value represents the "height" of the plane including gear, so even landed plane has small positive height.

**Do not** expect height to be 0 (zero) feet for landed planes.
{% endhint %}

{% hint style="warning" %}
Plane heading/bearing calculations uses simplified model of inclination evaluation. Note that there may be significant rounding error in those calculations (about +- 3 degrees) when comparing plane heading with runway bearing.
{% endhint %}

### Holding Point Announcements

This announcements are made when plane is approaching to the runway, e.g.: `Approachíng runway 26`. Note that once this callout si done for a specific runway, it will not be invoked again until the runway is _reset_ (see which properties do the reset below).

**Max height**

* Unit: feet
* Default value: 50 ft
* If the plane height from ground is above this value, no announcements will be evaluated. Moreover, _resets_ previous announcements. Used to suppress announcements for flying planes.

**Min Ortho-Distance (m)**

* Unit: meters
* Default value: 40 m
* If the plane is under this ortho-distance from runway, no announcements will be evaluated.

**Short-Long rwy thresholds**

* Unit: meters
* Default value: 1200 m
* Decides, wether the runwy is "short" or "long". Important for the further processing (see below).

**Announce long rwy ortho-distance**

* Unit: meters
* Default value: 120 m
* At this (or below) this ortho-distance, the announcement will be invoked. Applies only for **long** runways.

**Announce short rwy ortho-distance**

* Unit: meters
* Default value: 90 m
* At this (or below) this ortho-distance, the announcement will be invoked. Applies only for **short** runways.

**Max ortho-distance**

* Unit: meters
* Default value: 250
* At or above this ortho-distance, no announcements are evaluated. Moreover, this distance _resets_ previous announcements.

**Custom ICAO-based holding-point ortho-distance**

Here, you can specify the orth-distance at which (or below) the announcement is invoked for a specific airport defined by its ICAO code. In general, this overrides _Announce long rwy ortho-distance_ and _Announce long rwy ortho-distance_ for specific airports.

### Line Up Announcements

This covers announcements done when lining up the runway, e. g. `On runway 26`. Those announcements for a specific runway are not repeated until the same announcements for a different runways are done.

**Max height**

* Unit: feet
* Default value: 50 ft
* If the plane height from ground is above this value, no announcements will be evaluated. Used to suppress announcements for flying planes.

**Max rwy ortho-distance**

* Unit: meters
* Default value: 40 m
* If the plane orth-distnace from the runway is over this value, no announcements will be made.

**Max speed (IAS/kts)**

* Unit: kts
* Default value: 20 kts
* If plane indicated speed (IAS) is above this value, no announcements will be made. Used to suppress announcements for landing planes.

**Max bearing-heading diff (deg)**

* Unit: degrees
* Default value: 15°
* If difference between runway bearing and plane heading in absolute is above this value, no announcements will be made. Used to suppress announcements if the plane is on the runway, but heading a different (or not aligned) direction.&#x20;

### Landing Announcements

**Min height**

* Unit: feet
* Default value: 50 ft
* If the plane height from ground is below this value, no announcements will be evaluated.

**Max height**

* Unit: feet
* Default value: 800 ft
* If the plane height from ground is above this value, no announcements will be evaluated.

**Max distance**

* Unit: meters
* Default value: 7048 m (approx 4 nautical miles)
* If the plane distance from the runway threshold is above this value, no announcements will be evaluated.

{% hint style="info" %}
_Max height_  and _max distance_ controls when the approaching announcement should be invoked. Note that both those conditions must be fulfiled to invoke the announcement.
{% endhint %}

**Max rwy ortho-distance**

* Unit: meters
* Default value: 300 ft
* If the ortho-distance of the plane from the runway is above this value, no announcements will be evaluated. Used suppress the anouncements if the plane is not aligned with the runway.

**Max vertical speed**

* Unit: feet/min
* Default value: 0 ft/min
* If the plane vertical speed is above this value, no announcements will be evaluated. Note that positive value means climb and negative value means descend. Used to suppress warnings when planes are climbing on go-arounds during interrupted approach.

### Remaining Distance Announcements

fasef

**Max height**

* Unit: feet
* Default value: 50 ft
* If the plane height from ground is above this value, no announcements will be evaluated.

**Max rwy ortho-distance**

* Unit: meters
* Default value: 40 m
* If the plane orth-distnace from the runway is over this value, no announcements will be made. This is used to detect if the plane has vacated the runway.

**Max bearing-heading diff (deg)**

* Unit: degrees
* Default value: 15°
* If difference between runway bearing and plane heading in absolute is above this value, no announcements will be made. Used to suppress announcements if the plane is on the runway, but heading a different (or not aligned) direction.&#x20;



