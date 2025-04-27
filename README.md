# azure-

Βέβαια! Θα σου γράψω μια **καθαρή, επαγγελματική περιγραφή** του project σου στα αγγλικά, έτοιμη για documentation ή παρουσίαση:

---

# Project Description: Azure IoT Edge End-to-End Telemetry Filtering and Storage

## Objective
The goal of the project was to deploy an Azure IoT Edge device, simulate telemetry data, filter it based on specific temperature thresholds, and store the filtered data securely in Azure Blob Storage for future analysis.

---

## Architecture Overview

1. **IoT Edge Device (Linux - Zorin OS 17.3)**
   - Installed and configured **Azure IoT Edge runtime (aziot-edged)** on a Linux machine.
   - Deployed the **Simulated Temperature Sensor** module (`tempSensor`) to generate telemetry data (temperature, pressure, humidity).
   
2. **Azure IoT Hub**
   - Device (`NIKOLAIDIS_4`) registered and connected to IoT Hub.
   - Device telemetry was streamed live to IoT Hub.
   
3. **Azure Stream Analytics Job**
   - Created a Stream Analytics job to read incoming telemetry from IoT Hub.
   - Applied a **SQL-like query** to filter events where:
     ```sql
     machine.temperature > 27
     ```
   - Only events with `machine.temperature` above 27°C were selected for output.

4. **Azure Blob Storage (Storage Account: kubas)**
   - Configured Azure Storage as the output sink for Stream Analytics.
   - Filtered events were written into a container named **high-temperature-events**.
   - Files were organized by:
     ```
     date=YYYY-MM-DD/hour=HH/minute=MM/part-xxxxx.json
     ```

---

## Troubleshooting and Solutions

- **Issue:** Filesystem remounted as read-only (`ro`) after a device crash (caused by inserting a Linux installation USB stick).
- **Solution:** 
  - Re-mounted filesystem as `rw`.
  - Re-created missing directory `/var/lib/aziot/edged/mnt` with correct permissions (`iotedge:iotedge`).
  - Restarted IoT Edge runtime and recovered module functionality.

- **Issue:** Stream Analytics job initially did not find telemetry field `temperature`.
- **Solution:** 
  - Updated query to correctly point to nested telemetry field `machine.temperature`.
  - Restarted Stream Analytics job to reflect changes.

- **Verification:**  
  - Verified live telemetry from the device using `az iot hub monitor-events`.
  - Confirmed filtered events arriving into Azure Blob Storage.

---

## Final Results

✅ IoT Edge device running and sending telemetry  
✅ Stream Analytics filtering events based on machine temperature > 27°C  
✅ Filtered events stored in Azure Blob Storage structured by timestamp  
✅ Full cloud-side telemetry filtering with no need for device reconfiguration

---

# Technologies Used
- Azure IoT Edge (aziot-edged)
- Azure IoT Hub
- Azure Stream Analytics
- Azure Storage Account (Blob Containers)
- Azure CLI
- Azure Storage Explorer
- Linux (Zorin OS 17.3, based on Ubuntu 22.04)

---

# Summary
This project successfully demonstrated a complete Azure IoT Edge solution — from edge telemetry generation to cloud filtering and structured data storage — offering a scalable, secure way to manage IoT data workflows in real-time.

---

🔥 **This setup can now be expanded for:**
- Real-time alerts (temperature anomalies)
- Dashboarding (Power BI integration)
- Long-term data archiving
- Advanced predictive analytics (ML models)

---

✅ **Would you also like me to prepare a clean diagram (architecture flow) for this project?**  
(It would look great if you want to present it somewhere!) 🚀  
Just say "yes"!
