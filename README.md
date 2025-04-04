# Driver Risk Detection API

This model analyzes real-time dashcam footage to assess the risk level based on distance from other vehicles(proximity) and speed.

---

## Overview
- Uses **YOLOv8** to detect nearby vehicles.
- Estimates **proximity and speed** of other vehicles near the dashcam vehicle.
- Determines **risk level** as **Safe, Caution, or Danger**.
- Provides **two endpoints**:
  - `/live` → For real-time dashcam processing.
  - `/analyze` → For analyzing an uploaded video file(for anyone to test out the model).

---

## Installation & Setup
### **1. Install Dependencies**
Run the following command in cmd to install the required packages:

pip install -r requirements.txt

### **2. Ensure YOLO Model File Exists**
The `yolov8n.pt` file should be placed in the same directory as `driver_risk.py`(i've done it, but please double-check).

### **3. Run the API Server**
To start the API, navigate to the directory where the files are stored, and run:

uvicorn driver_risk:app --host 0.0.0.0 --port 8080 --reload

- **For local testing**, access the API docs at: `http://127.0.0.1:8080/docs`
- **For cloud hosting**, use services like **Railway, Render, or Google Cloud Run**.

---

## API Endpoints
### **1. `/live`  Real-Time Dashcam Analysis**
Processes a live video feed from the dashcam/webcam and streams risk level updates(this happens by running the driver_risk.py file, note that this will auto turn on the webcam of your device if used to normal device).
#### ** Request:**

- Method: `GET`
- URL: `http://localhost:8080/live` (this site would contain all JSON responses in real-time for the webcam live-feed)

#### **🔹 Example Response:**
```json
{
    "frame": 1710953456.732,
    "risk_level": "Safe",
    "proximity": 120.5,
    "speed": 8.9
}
```

---

### **2. Analyze an Uploaded Video File( i've named this test_video.mp4)**

Accepts a **video file** and processes it **frame-by-frame**.

#### ** Request:**
- Method: `POST`
- URL: `http://localhost:8080/live` (this site would contain all JSON responses in real-time for the test_video frames being processed)

#### **Example Request **
Run this code on cmd to run taking the input as a pre-recorded video(test_video.mp4) for your reference:

uvicorn driver_risk_example:app --host 0.0.0.0 --port 8080 --reload


#### **🔹 Example Response:**
```json
{
    "status": "success",
    "results": [
        {"frame": 1, "risk_level": "Safe", "proximity": 120.5, "speed": 8.9},
        {"frame": 2, "risk_level": "Caution", "proximity": 90.2, "speed": 15.1},
        {"frame": 3, "risk_level": "Danger", "proximity": 40.7, "speed": 28.3}
    ]
}
```

---

## Troubleshooting:
### ** If port already in use**
If `ERROR: [Errno 10048] error while attempting to bind on address` appears:

uvicorn driver_risk:app --port 8081  #Use a different port


## Finally:
 Ensure **all required files** are in the same directory:
- `driver_risk.py` (Main API file)
- `driver_risk_example.py` (Test API file for video processing)
- `yolov8n.pt` (YOLOv8 Model File)
- `requirements.txt` (Dependencies List)
- `README.md` (This file)
- `test_video.mp4` (For testing `/analyze` endpoint)


---

### Thank You.

