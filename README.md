### **Detailed Technical Description: IoT-Based Dehydrator Controller**

The project is an IoT-enabled dehydrator controller designed to automate and optimize the food dehydration process. It integrates hardware and software components to provide precise temperature control based on user-defined profiles while offering real-time monitoring through a mobile app. The system is modular and can be adapted for various sensors and actuators, making it highly versatile.

---

#### **1. System Overview**

The system consists of three major components:

1. **Controller Box**: The hardware unit powered by the ESP32 microcontroller. It interfaces with the heater, fan, and sensors to control the dehydration process.
2. **Mobile App**: Built using Flutter, the app enables users to upload drying profiles, start and stop operations, and monitor real-time performance.
3. **Cloud Server**: Developed using Golang, the server facilitates file uploads, profile storage, and communication between the app and the controller box.

---

#### **2. Workflow**

1. **Uploading Profiles**:
   - Users create custom drying profiles in an Excel file, specifying time and temperature coordinates.
   - Through the Flutter app, the file is uploaded to the cloud server using **HTTP**.

2. **Retrieving Profiles**:
   - The ESP32 controller connects to the cloud server via Wi-Fi.
   - It retrieves the profile file and parses the time-temperature data.

3. **Executing Profiles**:
   - Based on the retrieved data, the controller adjusts the heater's power via relays to maintain the target temperature.
   - The fan operates simultaneously to distribute heat uniformly.
   - The temperature sensor provides real-time feedback to the ESP32 for precise control.

4. **Real-Time Monitoring**:
   - The ESP32 publishes real-time data (temperature readings, status updates) to the server using the **MQTT** protocol.
   - The Flutter app subscribes to the MQTT topics and displays a live graph comparing the expected and actual temperatures.

5. **Fault Detection and Indication**:
   - The controller monitors operational conditions.
   - Any faults (e.g., sensor failure or temperature deviations) are flagged, and the status is updated on the app. The Fault LED on the controller also indicates errors.

---

#### **3. Hardware Components**

- **ESP32 Microcontroller**:
  - Handles Wi-Fi connectivity, MQTT communication, and actuator control.
  - Downloads drying profiles from the server.
  - Executes the control logic for temperature and fan operations.

- **Temperature Sensor**:
  - Monitors the dehydrator's internal temperature.
  - Provides real-time feedback to the controller for accurate adjustments.

- **Heater**:
  - Controlled through a relay module based on the desired temperature setpoints.

- **Fan**:
  - Ensures uniform heat distribution across the drying chamber.

- **Relays**:
  - Switches the heater and fan on or off under ESP32 control.

- **LED Indicators**:
  - **Network LED**: Indicates Wi-Fi connection status.
  - **Power LED**: Signals that the controller is powered on.
  - **Fault LED**: Lights up in case of operational errors.

- **Memory Card (16GB)**:
  - Stores profiles and logs locally as a backup in case of network failures.

---

#### **4. Software Components**

##### **4.1 Mobile App (Flutter Framework)**
- **Purpose**:
  - User-friendly interface for interacting with the dehydrator.
  - Facilitates profile upload, operation monitoring, and control.

- **Features**:
  - Upload profiles via HTTP to the cloud server.
  - Start/stop batch operations.
  - Display real-time temperature graphs with expected vs. actual values.
  - Notifications for faults or completed operations.

- **Technology**:
  - Cross-platform development using Flutter (Dart programming language).

---

##### **4.2 Cloud Server (Golang)**
- **Purpose**:
  - Acts as the central hub for communication between the app and the controller.
  - Handles file uploads, stores drying profiles, and manages real-time data exchange.

- **Features**:
  - Implements RESTful APIs for file uploads and status updates.
  - Manages MQTT communication for real-time monitoring.
  - Securely stores user-uploaded profiles and operation logs.

- **Technology**:
  - Built using Golang for high performance and scalability.
  - Integrates with cloud platforms to handle file storage and message brokering.

---

##### **4.3 Communication Protocols**
- **HTTP**:
  - Used for uploading Excel files (profiles) to the server.
  - Facilitates batch start/stop commands sent from the app to the server.

- **MQTT**:
  - Lightweight publish-subscribe protocol for real-time communication.
  - ESP32 publishes temperature readings and status updates to the server.
  - The app subscribes to these updates to display real-time data to the user.

---

##### **4.4 ESP32 Firmware**
- **Purpose**:
  - Executes the drying profile logic and communicates with the server and app.

- **Features**:
  - Wi-Fi initialization and cloud server connection.
  - Parsing of profile data and implementing control logic.
  - Publishing real-time data via MQTT.
  - Error detection and handling.

- **Technology**:
  - Developed using Arduino IDE or MicroPython.

---

#### **5. Key Features and Advantages**

1. **Custom Profiles**:
   - Allows precise control over drying time and temperature for various food items.

2. **Real-Time Monitoring**:
   - Users can track the drying process remotely through the app.

3. **Fault Detection**:
   - Ensures reliability with error alerts and notifications.

4. **Modularity**:
   - Supports additional sensors and actuators, enabling future upgrades.

5. **Scalability**:
   - Cloud-based architecture allows integration of multiple dehydrators or additional IoT devices.
