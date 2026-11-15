# System component list


The following document is an overview of every major component of the Parking lot availability System. 
It includes each component's purpose and responsibilities as well.


The system is divided into three main environments:

* **Sensor (Hardware):** Detects vehicles, records vehicle events, and stores data locally when offline.

* **Server (Backend):** Receives sensor data, processes vehicle events, manages pools, handles accounts, and stores historical information.

* **Client (Frontend):** Provides the web interface for Hosts and Guests to view and manage parking information.

  

| Component |	Runs on |	Responsibility | Talks to|
| :---    | --- | --- | ---   |
|Sensor Firmware | Sensor microcontroller (ESP32)  |Detects passing vehicles and records timestamp, direction, speed, and vehicle type. Stores events locally when offline and sends stored events after reconnecting. Reports sensor status and battery level.   |Vehicle Detection Hardware, Local Storage, Wi-Fi Network, Ingestion Service   |
|Local Sensor Storage |Sensor microcontroller | Temporarily stores vehicle events when the sensor cannot communicate with the server. Preserves pending events until successful synchronization. |Sensor Firmware, Ingestion Service |
|Ingestion service| Backend server |Receives vehicle events from sensors, validates incoming data, handles synchronization after reconnection, and forwards events for processing and storage. |Sensor Firmware, Backend API, Event Processing, Database |
|Backend API | Backend server (Node.js or Phyton 3) | Receives vehicle events from sensors, validates incoming data, handles synchronization after reconnection, and forwards events for processing and storage. | Sensor Firmware, Backend API, Event Processing, Database|
|Database | Database server (PostgreSQL)| Stores account information, sensor configurations, vehicle events, pool settings, historical records, sensor status, alerts, and Guest sharing settings. Preserves historical information when sensors or pools are removed or changed.|Backend API, Ingestion Service, Authentication, Sensor Management, Pool Management, Historical Data, Alert Service |
|Data Buffer | Sensor microcontroller (ESP32) | Temporarily stores vehicle events when Wi-Fi is unavailable. Maintains pending events until the backend confirms successful receipt. Supports retransmission after reconnection. | Sensor Firmware and Data Synchronization|
|Web app (Host + Guest) | User's Web browsers (React or Js) | Provides the user interface. Hosts can manage sensors, pools, alerts, and historical data. Guests can view establishment information and shared parking availability. Displays maps and live updates. | Backend API, External Map Provider |
|Authentication & Access Control |Backend server | Verifies Host login credentials and manages permissions. Ensures Hosts can access only their own information, and Guests can access only data explicitly shared by the Host. |Backend API, Database, Web App |
|Alert service Backend server (background module or worker) | Monitors sensor problems, including offline and low-battery conditions, and Host-defined pool thresholds. Creates alerts and makes them available to Hosts. Database, Sensor Management, Pool Management, Backend API, Host Dashboard |
|Map provider (external) | External mapping service| Provides map functionality to display parking lot locations and pool availability on the Host and Guest dashboards. Does not calculate parking availability. Web App |
|Sensor Management | Backend server | Manages sensor registration, labels, configurations, removal, connection status, and battery information. Preserves historical records when sensors are removed. Backend API, Database, Ingestion Service |
|Event Processing & Pool Management | Backend server |Processes vehicle events and calculates pool counts, remaining capacity, availability percentages, and Full status. Handles IN/OUT sensor roles, manual resets, and offline sensor warnings. |Ingestion Service, Backend API, Database, Alert Service |
|Historical Data & Export | Backend server Retrieves historical vehicle events and statistics. Supports time-based queries, breakdowns by vehicle type, direction, and speed, individual vehicle history, and data exports. |Backend API, Database, Web App |
|Data Synchronization |Sensor firmware / communication logic | Sends pending events to the backend, retries unsuccessful transmissions, and handles acknowledgments| Data Buffer, Wi-Fi Connection, Ingestion Service|
|Wi-Fi Connection| Network |Provide wireless communication between the sensor and backend server |Data Synchronization, Ingestion service |
|Magnetometer Sensors| Sensor hardware Detect magnetic changes caused by passing vehicles. |Sensor Firmware |

<br>

 ## Sensor vs. Server Responsibilities


It is important to separate the responsibilities of the physical sensors from the backend server.
<br>

**Sensor Responsibilities**

* Detect passing vehicles.

* Record individual vehicle events.

* Store events locally when Wi-Fi is unavailable.

* Synchronize stored events after reconnecting.

* Report sensor status and battery information.

<br>

**Server Responsibilities**

* Receive and process vehicle events.

* Calculate pool counts and parking availability.

* Manage Host accounts, sensors, and pools.

* Control Host and Guest permissions.

* Store historical data and generate exports.

* Manage alerts and threshold notifications.

* Provide information to the Web App.



<br>
 **Communication Between Components**

 | Communication | Possible Technology | Purpose |
 | --- | --- | ---|
| Magnetometer Sensors → Sensor Firmware |Sensor hardware interface | Vehicles detection | 
| Sensor Firmware → Data Buffer |Local storage and MicroPython |Saves detected vehicle events locally, including timestamps, direction, speed, and vehicle type when available. | 
| Data Buffer → Data Synchronization | MicroPython and Local storage access |Retrieves pending vehicle events that need to be transmitted to the backend. | 
| Data Synchronization → Backend Ingestion Service |Wi-Fi with HTTPS and JSON | Retrieves pending vehicle events that need to be transmitted to the backend. | 
| Backend Ingestion Service → Sensor | HTTPS acknolegment response |Confirms successful event receipt so the sensor can remove acknowledged events from its buffer. | 
| Web Application → Backend API|REST API / HTTPS, JSON and WebSocket |Sends requests and retrieves sensor information, pool availability, and historical data. |
| Backend → Database|PostgreSQL | Stores and retrieves accounts, sensors, vehicle events, pools, alerts, and historical records.| 
| Backend → Web Application| JavaScript and React  |Provides updated information for the Host dashboard and Guest view. | 
| Web Application → Map Provider| Map API|Displays sensor locations and parking-lot locations on the map. | 


 

