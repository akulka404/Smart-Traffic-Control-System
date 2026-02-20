# Smart Traffic Control System

A computer-vision-based traffic management application that uses OpenCV to automatically detect and count vehicles at traffic signals, monitor live camera feeds, and track objects in real time. The system features a PyQt5 dark-themed GUI that integrates all three modes into a single control panel.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Circuit Diagram](#circuit-diagram)
- [How It Works](#how-it-works)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

Traffic congestion is a growing problem in urban areas. This project tackles it by using image processing and computer vision techniques to:

1. Count the number of vehicles present at each of four traffic signal lanes using static images.
2. Detect red-light signals from a live webcam feed using HSV color segmentation.
3. Track a user-selected object across video frames using OpenCV's built-in object-tracking algorithms.

All three capabilities are accessible through a compact PyQt5 GUI with a dark theme.

---

## Features

| Feature | Description |
|---|---|
| **Vehicle Count per Road** | Processes four traffic-signal images using a Haar Cascade classifier (`cars.xml`) to count vehicles in each lane and display the counts on LCD widgets. |
| **Live Camera Feed** | Captures frames from the default webcam, applies Gaussian blur and HSV masking to detect red-colored objects (traffic lights), and draws bounding boxes with centroid markers. |
| **Live Object Tracking** | Uses OpenCV object-tracking algorithms (CSRT by default) to track a region of interest selected by the user in a video file or live stream. |
| **Dark-themed GUI** | Built with PyQt5 and styled with `qdarkstyle` for a modern, readable interface. |

---

## Project Structure

```
Smart-Traffic-Control-System/
├── gui.py                     # Main PyQt5 application window and button logic
├── gui.ui                     # Qt Designer UI definition file
├── live.py                    # Live webcam feed with red-object detection
├── opencv_object_tracking.py  # Object tracking using OpenCV trackers
├── cars.xml                   # Haar Cascade classifier for vehicle detection
├── Circuit.png                # Hardware circuit diagram
├── Images/
│   ├── 1.jpg                  # Traffic signal 1 sample image
│   ├── 2.jpg                  # Traffic signal 2 sample image
│   ├── 3.jpg                  # Traffic signal 3 sample image
│   ├── 4.jpg                  # Traffic signal 4 sample image
│   └── video.mp4              # Sample video for object tracking
└── README.md
```

---

## Prerequisites

- Python 3.6+
- A webcam (for live-feed and live-tracking modes)
- The following Python packages:

| Package | Purpose |
|---|---|
| `opencv-python` | Image processing and computer vision |
| `PyQt5` | GUI framework |
| `qdarkstyle` | Dark stylesheet for PyQt5 |
| `imutils` | Convenience utilities for OpenCV (video stream, FPS counter, resizing) |
| `numpy` | Numerical operations on image arrays |

---

## Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/akulka404/Smart-Traffic-Control-System.git
   cd Smart-Traffic-Control-System
   ```

2. **Install dependencies**

   ```bash
   pip install opencv-python PyQt5 qdarkstyle imutils numpy
   ```

3. *(Optional)* Place your own traffic images as `Images/1.jpg` through `Images/4.jpg` and a video as `Images/video.mp4`.

---

## Usage

Run the main GUI application:

```bash
python gui.py
```

The control panel exposes three buttons:

| Button | Action |
|---|---|
| **Traffic per Road** | Reads `Images/1.jpg` – `Images/4.jpg`, detects vehicles with the Haar Cascade, shows each image with bounding boxes, and displays per-lane counts on the LCD displays. Press any key to advance between images. |
| **Live Feed Camera 1** | Opens the default camera (`/dev/video0` or index `0`), overlays a crosshair grid, and highlights red-colored regions in real time. Press **Esc** to exit. |
| **Live Tracking** | Opens `Images/video.mp4` (or a live stream if no file is found), displays frames at 500 px width. Press **s** to select a region of interest and start tracking; press **q** to quit. |

---

## Circuit Diagram

The hardware integration circuit for this project is shown below:

![Circuit Diagram](Circuit.png)

---

## How It Works

### 1. Vehicle Detection (`gui.py` — Traffic per Road)

- Images for four traffic lanes are loaded with `cv2.imread`.
- Each frame is converted to grayscale and passed to `cv2.CascadeClassifier.detectMultiScale` using a pre-trained `cars.xml` Haar Cascade.
- Detected vehicles are outlined with red rectangles and counted.
- The highest-traffic lane is printed to the console and the counts are shown on the GUI's LCD widgets.

### 2. Live Red-Object Detection (`live.py`)

- Each webcam frame is blurred with a Gaussian filter to reduce noise.
- The frame is converted to the HSV color space and a mask is applied for red hues (`H: 0–10`, `S: 100–255`, `V: 100–255`).
- The mask is dilated to fill gaps, and contours above an area threshold of 3,500 px² are drawn as bounding boxes with centroid circles.

### 3. Object Tracking (`opencv_object_tracking.py`)

- Supports multiple OpenCV trackers: **CSRT** (default), KCF, Boosting, MIL, TLD, MedianFlow, and MOSSE.
- The user presses **s** to draw a bounding box around the target object; the chosen tracker then follows it frame by frame.
- Tracker type can be changed via the `--tracker` command-line argument when the script is run directly.

---

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository.
2. Create a new branch: `git checkout -b feature/your-feature-name`.
3. Commit your changes: `git commit -m "Add your feature"`.
4. Push to the branch: `git push origin feature/your-feature-name`.
5. Open a Pull Request.

---

## License

This project is open-source. See the repository for license details.
