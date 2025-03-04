# Checklist Overview

## Overview

An **airplane checklist** is a crucial tool that ensures safety, consistency, and efficiency in aviation operations. It helps pilots follow a structured set of procedures before, during, and after a flight, reducing the risk of human error and ensuring that no critical step is overlooked. By systematically verifying the aircraft’s condition, systems, and controls, checklists help confirm that the plane is ready for flight and operating within safe parameters.

Beyond routine procedures, checklists are also vital for managing emergency situations. In high-stress moments, pilots can rely on clear, step-by-step instructions to handle issues such as engine failures, electrical malfunctions, or unexpected weather conditions. This structured approach prevents panic and improves decision-making under pressure.

Another important function of checklists is standardization. They ensure that flight crews follow consistent procedures, whether they are flying different aircraft or operating in various conditions. This not only enhances safety but also improves coordination between pilots and co-pilots, as they can systematically confirm critical tasks together.

Checklists are typically invoked with respect to specific task, like (but not limited to):

* **Preflight Checklist** – Ensures the aircraft is airworthy before departure.
* **Before Takeoff Checklist** – Confirms all systems are ready for flight.
* **Climb, Cruise, and Descent Checklists** – Covers in-flight operational checks.
* **Landing Checklist** – Prepares the aircraft for safe landing.
* **Emergency Checklists** – Guides pilots through specific malfunctions or failures.
* **Shutdown and Securing Checklist** – Ensures safe power-down after landing.

## Checklists in the App

In the app, the checklists are defined in the XML file. Every file can contain several checklists. Every checklist consists of several check items, where every item has its label/description (like "Landing gear") and the confirmation ("down/checked/off/...").

In FS2020 and this App, checklists can be invoked manually (from the App or by pressing a keyboard shortcut) or automatically (based on plane conditions - like climbing, landing light on, or others).

## What now?

* I need to know, how to initialize and load the checklist - go to [Checklist Init](checklist-init.md).
* I need to know how to manage checklist during the flights -> go to [Checklist Run](checklist-run.md).
* I need to know, how to set generic settings for the whole Checklist module -> go to [Checklist Settings](checklist-settings.md).
* I'd like to create my own, or change existing checklist -> go to [Checklist XML File](checklist-xml-file.md).
