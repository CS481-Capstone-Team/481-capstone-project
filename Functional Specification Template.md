# Functional Specifications

**Project:** Vehicle Lane Counter System  
**Status:** Draft  
**Team:** The international Capstoners   
**Authors:** Guilherme, Zachary, Monica  

## Version History

Version history ommitted as it will be tracked entirely by GitHub.

## 1. Introduction

### 1.1 Purpose
Describe why this document exists, which decisions it supports, and how it will be used during design, development, testing, and acceptance.

This document describes what the Vehicle Lane Counter must do from the user's point of view. It covers user-facing expectations only.

### 1.2 Scope
Define the system or feature boundary.

**In scope**
- Sensors that count each vehicle passing in a lane and log direction, speed, and vehicle type.
- A web app to view live and historical data.
- Host accounts with full control of sensors, pools, alerts, data, and Guest visibility.
- Read-only Guest view of Host-chosen items.
- Pools that combine sensors to track a total (e.g., parking lot availability).
- Host dashboard with live sensor and pool data.
- Historical data queries, breakdowns, and export.
- Offline behavior: sensors keep counting while offline and their counts appear after reconnecting.
- Alerts for system events and Host-defined thresholds.
- MVP: 1 account, 2 sensors, working as expected through a simulated downtime with counts reconciling on reconnect.
- A parking lot map will be displayed in the web app.
- Parking recommendation for Guest entering establishments having more than one Parking lot. 

**Out of scope**
- How people inside a company share an account (out of MVP scope).
- Determine each lot occupancy
- Enforcement of capacity, and any gate control, payment, reservation, or ticketing function.
- turn on and off the sensors remotely. 
   

### 1.3 Intended Audience
Identify who should read this document and why.

| Audience | Why they need this document |
| --- | --- |
| Product / Sponsor | <Reason> |
| Developers | <Reason> |
| QA / Testers | <Reason> |
| Instructors / Evaluators | <Reason> |
| End Users / Client Representatives | <Reason> |

### 1.4 Definitions, Acronyms, and Terms
Define terms used throughout this document.

| Term | Definition |
| --- | --- |
| Host | Account user with full control of the account, sensors, pools, alerts, data, and what Guests see. |
| Guest | Read-only user who sees only the items a Host chooses to share (e.g., parking availability). |
| Sensor | Device installed in a lane that counts each vehicle passing over it. |
| Pool | A group of sensors used to track a total, such as the number of vehicles in a parking lot. |
| In sensor | A sensor in a pool whose vehicles add to the pool count. |
| Out sensor | A sensor in a pool whose vehicles subtract from the pool count. |
| Sensor role | Whether a sensor counts as "in" or "out" within a pool. |
| Capacity | The maximum number of vehicles a pool can hold, set by the Host. |
| Availability | Remaining space in a pool, shown as a number out of capacity and as a percentage. |
| Full | Status shown when a pool has reached its capacity. |
| Vehicle event | A single detected vehicle, with its timestamp, direction, speed, and vehicle type. |
| Sensor offline | Period when a sensor is not connected. Shown as a visible gap in history. |
| Establishment | The place a Guest looks up to view shared data (e.g., a shopping mall). |

### 1.5 Reference Documents
List supporting documents, such as project requirements, designs, mockups, policies, or standards.

| Document | Version / Date | Notes |
| --- | --- | --- |
| <Document name or link> | <Version or date> | <Notes> |

### 1.6 Document Conventions
Use consistent language and requirement IDs. Suggested conventions:

- **Must:** Mandatory behavior
- **Should:** Recommended behavior
- **May:** Optional behavior
- **IDs:** `FR-01` (functional), `NFR-01` (nonfunctional), `UI-01` (interface)

<Additional conventions, if any>

## 2. Product Overview

### 2.1 Product Context
Describe the product's background, related systems, and operating context.

A sensor embedded in a lane counts each vehicle passing over it and logs direction, speed, and vehicle type. Users view live and historical data in a web app.

Use cases:
- **Generic lane counter** (e.g., weigh stations): count and analyze traffic in a lane.
- **Parking lot tracking**: count vehicles in and out to show live availability.

### 2.2 User Classes and Characteristics
Describe the user groups, their needs, and relevant characteristics.

**Host**
- Signs in with an account login.
- Manages their own sensors, pools, alerts, and data.
- Chooses exactly which pools or sensors Guests can see.
- Needs live and historical traffic data.

**Guest**
- Does not need an account. Opens the web app, finds the establishment, and views its shared data.
- Read-only access to Host-chosen items only.
- Non-technical. Needs simple, friendly information such as parking availability.
- Example: a shopping mall visitor checking parking availability.

### 2.3 Overview of Functional Requirements
Summarize the product's major features and expected capabilities.

- Vehicle counting with direction, speed, and vehicle type per vehicle.
- Sensor management: label, rename, reconfigure, remove.
- Pools: combine in and out sensors, set capacity, show live availability and "Full" status, manual reset.
- Host dashboard with live sensor stats, pool availability, and sensor status.
- Historical data: time window queries, breakdowns, per-vehicle history, export.
- Offline handling: sensors keep counting while offline, gaps are clearly marked, counts appear after reconnect.
- Guest view of Host-chosen pools or sensors.
- Alerts for system events and Host-defined thresholds.

### 2.4 Overview of Data Requirements
Summarize the data collected, stored, processed, or transmitted.

- Each detected vehicle: timestamp, direction, speed, vehicle type.
- Vehicle type is best effort. Target categories: car, motorcycle, semi.
- Sensor label, chosen freely by the Host (e.g., `lane7`, `parking-lot-92-exit`).
- Sensor status: offline, low battery, and similar.
- Pool settings: member sensors, each sensor's role (in or out), capacity.
- History records the role a sensor had at the time of each vehicle.
- Offline periods are recorded and shown as gaps.
- Data is kept indefinitely and never lost, including when sensors or pools are removed or changed.

### 2.5 Operating Environment
Describe supported hardware, software, networks, and dependencies.

- Web app, usable on any device with a browser.
- Sensors are installed in a lane by the user.
- Sensors connect to the user's WiFi.
- Sensors run on battery.

### 2.6 General Constraints, Assumptions, Dependencies, and Guidelines
Include relevant budgets, schedules, external dependencies, regulations, or assumptions.

- MVP supports 1 account and 2 sensors.
- MVP must work as expected through a simulated downtime, with counts reconciling on reconnect.
- The host installs the sensor and provides WiFi.
- Vehicle type accuracy depends on sensor precision.

### 2.7 Design and Implementation Constraints
List technical limitations, architecture rules, or compatibility requirements.

<Design and implementation constraints>

### 2.8 User Documentation
Describe planned manuals, tutorials, help guides, or online documentation.

<User documentation>

## 3. Requirements

### 3.1 External Interface Requirements

#### 3.1.1 User Interfaces
Describe screens, navigation, CLI interactions, or other user-facing interfaces.

- **Host sign-in:** account login.
- **Host dashboard:** live per-sensor stats, per-pool availability (number, capacity, percentage, full status), and sensor status (offline, low battery, and similar). More widgets may be added later.
- **Sensor management:** label, rename, reconfigure, or remove sensors.
- **Pool management:** add in and out sensors, set capacity, rename, reconfigure, reset count, or remove pools.
- **Historical data:** time window queries, breakdowns by vehicle type, direction, and speed, per-vehicle event history, export.
- **Guest dashboard:**  and view its shared data. Simple and non-technical.
- **Alerts:** set up threshold alerts and see system alerts.
- Times are shown in a user-defined time zone.

#### 3.1.2 Hardware Interfaces
Describe devices, sensors, peripherals, and required hardware compatibility.

- Sensor is embedded in a lane and detects each vehicle passing over it.
- Sensor runs on battery and reports low battery.
- Sensor keeps counting while offline. It stops only when out of battery or damaged.

#### 3.1.3 Software Interfaces
Describe integrations with other software, including APIs and data formats.

<Software interface requirements>

#### 3.1.4 Communications Interfaces
Describe networking, communication protocols, and message exchange requirements.

- Sensors connect to the user's WiFi.
- Counts made while offline are sent once the sensor reconnects.

### 3.2 Functional Requirements
Repeat the following block for each functional requirement. Give every requirement a unique ID.

#### FR-01: Host Sign-In

- **Purpose / Description:** Host must sign in with an account login to access their account.
- **Inputs:** Account login.
- **Processing:** Verify the login and open the Host's account.
- **Outputs:** Host sees only their own account's sensors, pools, alerts, and data.

#### FR-02: Sensor Setup and Management

- **Purpose / Description:** Host must be able to label and manage each sensor.
- **Inputs:** Sensor label (free text, e.g., `lane7`, `parking-lot-92-exit`), configuration changes, remove request.
- **Processing:** Save the label and changes. On removal, keep all of the sensor's past data.
- **Outputs:** Sensor shows under its label. Changes take effect. Past data stays available.

#### FR-03: Vehicle Detection and Logging

- **Purpose / Description:** Each sensor must count every vehicle that passes over it.
- **Inputs:** A vehicle passing over the sensor.
- **Processing:** Record the vehicle's timestamp, direction, speed, and vehicle type. Vehicle type is best effort (target: car, motorcycle, semi).
- **Outputs:** A new vehicle event appears in live stats and in history.

#### FR-04: Offline Counting and Reconnection

- **Purpose / Description:** Sensors must keep counting while offline, and no counts may be lost when they reconnect.
- **Inputs:** Sensor loses connection, then reconnects.
- **Processing:** Sensor keeps counting while offline. On reconnect, the offline counts are added to history. Counts are never estimated or filled in.
- **Outputs:** True counts eventually show up. The "sensor offline" gap in history closes. If the sensor lost power or was damaged, the gap stays permanent.

#### FR-05: Offline Gaps in History

- **Purpose / Description:** History must clearly show when a sensor was offline.
- **Inputs:** Sensor offline period.
- **Processing:** Mark the period as a gap. Do not estimate counts for it.
- **Outputs:** A visible gap marked "sensor offline" in history.

#### FR-06: Pool Setup and Management

- **Purpose / Description:** Host must be able to create pools that combine sensors to track a total.
- **Inputs:** Pool name, sensors, each sensor's role (in or out), capacity.
- **Processing:** "In" sensors add to the pool count. "Out" sensors subtract. Host can add as many in and out sensors as desired. Host can rename, reconfigure, change capacity, or remove a pool. Removing or changing a pool keeps its past data.
- **Outputs:** Pool is created or updated. A warning is shown if a pool has only "in" or only "out" sensors.

#### FR-07: Sensor Role Change

- **Purpose / Description:** Host must be able to change a sensor's role.
- **Inputs:** New role for the sensor.
- **Processing:** Apply the new role to future vehicles only. History keeps the role the sensor had at the time.
- **Outputs:** Current pool count is not changed (TBD, to confirm).

#### FR-08: Live Pool Availability

- **Purpose / Description:** Each pool must show its current availability live.
- **Inputs:** Vehicle events from the pool's sensors, pool capacity.
- **Processing:** Update the pool count as vehicles go in and out.
- **Outputs:** Availability as a number out of capacity and as a percentage. "Full" status when the pool is full.

#### FR-09: Pool Manual Reset

- **Purpose / Description:** Host must be able to manually reset a pool's count.
- **Inputs:** Reset request from the Host.
- **Processing:** Reset the pool's count.
- **Outputs:** Pool shows the reset count.

#### FR-10: Pools With Offline Sensors

- **Purpose / Description:** Pools must warn users when a sensor in them is offline.
- **Inputs:** A sensor in the pool goes offline.
- **Processing:** Keep the pool's last known count.
- **Outputs:** Pool is flagged as potentially inaccurate until the offline counts arrive or the Host resets the pool.

#### FR-11: Host Dashboard

- **Purpose / Description:** Host must see sensor and pool data live in one place.
- **Inputs:** Live sensor and pool data.
- **Processing:** Keep the dashboard up to date.
- **Outputs:** Per-sensor live stats, per-pool availability (number, capacity, percentage, full status), and sensor status (offline, low battery, and similar).

#### FR-12: Historical Data Queries

- **Purpose / Description:** Host must be able to look at a sensor's past data.
- **Inputs:** Sensor and time window (e.g., the last X units of time).
- **Processing:** Gather the sensor's data for that window.
- **Outputs:** Totals for the window, with breakdowns by vehicle type, direction, and speed.

#### FR-13: Per-Vehicle Event History

- **Purpose / Description:** Host must be able to view each individual vehicle recorded.
- **Inputs:** Sensor and time window.
- **Processing:** List the vehicle events.
- **Outputs:** Each vehicle's timestamp, direction, speed, and type.

#### FR-14: Data Export

- **Purpose / Description:** Host must be able to export data.
- **Inputs:** Export request for aggregated results or per-vehicle events.
- **Processing:** Prepare the requested data for download.
- **Outputs:** Exported aggregated results or per-vehicle events.

#### FR-15: Data Retention

- **Purpose / Description:** Data must be kept indefinitely and never lost.
- **Inputs:** All recorded data.
- **Processing:** Keep data even when sensors or pools are removed or changed.
- **Outputs:** Full history is always available.

#### FR-16: Guest Sharing

- **Purpose / Description:** Host must choose exactly which pools or sensors Guests can see.
- **Inputs:** Host's selection of pools or sensors to share.
- **Processing:** Show only the selected items to Guests.
- **Outputs:** Guests see the shared items and nothing else.

#### FR-17: Guest View

- **Purpose / Description:** Guests must be able to view an establishment's shared data without an account.
- **Inputs:** Guest opens the web app and finds the establishment.
- **Processing:** Show the establishment's shared items, read-only, updating live within a reasonable delay.
- **Outputs:** Simple, friendly view with no technical detail (e.g., parking availability).

#### FR-18: System Alerts

- **Purpose / Description:** Host must be alerted about sensor problems.
- **Inputs:** System events such as sensor offline, sensor low battery, and similar.
- **Processing:** Detect the event and create an alert.
- **Outputs:** Alert to the Host. Delivery channels are TBD.

#### FR-19: Threshold Alerts

- **Purpose / Description:** Host must be able to set alerts based on their own thresholds.
- **Inputs:** Threshold set by the Host (e.g., a pool reaching a set percentage full).
- **Processing:** Watch for the threshold being reached.
- **Outputs:** Alert to the Host when the threshold is reached. Delivery channels are TBD.

#### FR-20: Time Zone Display

- **Purpose / Description:** Times must be shown in a user-defined time zone.
- **Inputs:** User's chosen time zone.
- **Processing:** Show all times in that time zone.
- **Outputs:** Times displayed in the chosen time zone.

### 3.3 Performance Requirements
Define measurable response-time, throughput, scalability, or resource-use targets.

- Live views, including the Guest view, update within a reasonable delay.
- MVP supports 1 account and 2 sensors.

### 3.4 Security
Describe access control, data protection, and other security requirements.

- Host must sign in with an account login.
- An account can only access its own sensors.
- Guests have read-only access.
- Guests see only the pools or sensors the Host chooses to share.

### 3.5 Usability
Describe accessibility, navigation, and ease-of-use requirements.

- Web app must be usable on any device with a browser.
- Guest view must be friendly to all users, with no technical detail.
- Guests do not need an account to view shared data.

### 3.6 Other Requirements
Include relevant legal, regulatory, compliance, or other requirements not covered above.

<Other requirements>

## Appendix A. Analysis Models

List supporting diagrams, such as data flows, class diagrams, state diagrams, or entity-relationship diagrams.

| Document name | Description | Location |
| --- | --- | --- |
| <Model name> | <Description> | <File path or link> |

## Appendix B. Issues List

Track unresolved issues and their status.

| Issue ID | Issue description | Status |
| --- | --- | --- |
| ISS-01 | Can a sensor belong to more than one pool? | Open |
| ISS-02 | Pool count when a sensor's role changes (current intent: unchanged). | Open |
| ISS-03 | Alert delivery channels (email, SMS, push, etc.). | Open |
| ISS-04 | Vehicle type categories (depend on sensor precision). | Open |
| ISS-05 | Guest access method. | Open |
| ISS-06 | How people inside a company share an account. | Open |
| ISS-07 | Default time zone. | Open |
