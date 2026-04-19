# rtsp-gmm-motion-detection

Real-time motion detection over RTSP using Gaussian Mixture Model (GMM) background subtraction with OpenCV. Built as a robust alternative to default camera software that struggles to distinguish meaningful motion from environmental noise such as wind-blown foliage.

---

## The Problem

Most IP cameras ship with built-in motion detection based on simple frame differencing or fixed-threshold pixel comparison. This approach is highly sensitive to irrelevant changes in the scene — a tree branch swaying in the wind triggers the same alert as a person walking through the frame.

This project replaces that naive approach with a statistically-driven background model that **learns what "normal" looks like** and only flags deviations that are truly significant.

---

## Solution: GMM Background Subtraction

Instead of comparing frames naively, the system models each pixel as a mixture of Gaussian distributions over time. The background model continuously adapts to slow, gradual changes (swaying trees, lighting shifts) while remaining sensitive to abrupt, significant motion (people, vehicles).

**Key advantages over the default:**
- Naturally ignores repetitive, low-amplitude motion (wind, foliage)
- Adapts to gradual scene changes without false positives
- Detects real moving objects (people, bicycles) reliably in real time
- No deep learning required — runs efficiently on modest hardware

---

## Demo

### Video 1 — Wind Robustness
A tree sways continuously in the wind. The GMM background model correctly identifies this as a background pattern and suppresses it, producing **zero false detections**.

### Video 2 — Real-Time Object Detection
<video src="./assets/camera_video_03.mp4"></video>
A man walking and a person on a bicycle cross the scene. The system detects and localizes both as significant motion events in **real time**, with no classification or neural network involved.

---

## Stack

| Component | Technology |
|---|---|
| Camera stream ingestion | RTSP |
| Stream handling | OpenCV VideoCapture / FFmpeg |
| Background subtraction | GMM (`cv2.createBackgroundSubtractorMOG2`) |
| Language | Python (OpenCV) |

---

## How It Works

```
RTSP Stream
    │
    ▼
Frame Capture (OpenCV / FFmpeg)
    │
    ▼
Guassian Blur Filtering
    │
    ▼
GMM Background Subtraction
    │  → Updates background model per frame
    │  → Outputs foreground mask
    ▼
Contour Detection
    │  → Filter by area threshold
    ▼
Bounding Box Overlay → Display / Record
```

---


## Why Not Deep Learning?

This project intentionally avoids DL-based object detection (YOLO, SSD, etc.) to demonstrate that:

- Classical computer vision is still highly effective for motion-based surveillance
- GMM is computationally lightweight and deployable on edge hardware
- No GPU, no model weights, no internet connection required
- Inference latency is sub-millisecond compared to tens of milliseconds for DL models

---

## References

- Stauffer, C., & Grimson, W. E. L. (1999). *Adaptive background mixture models for real-time tracking.* CVPR.
- OpenCV Documentation — [Background Subtraction](https://docs.opencv.org/4.x/d1/dc5/tutorial_background_subtraction.html)

---

## License

MIT
