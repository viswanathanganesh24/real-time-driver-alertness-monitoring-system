# Real-Time Driver Alertness Monitoring System

Driving tired is dangerous, plain and simple. Even a few seconds of micro-sleep at highway speeds can lead to a catastrophe. This project is a practical attempt to tackle that problem head-on. It uses a standard webcam and some clever computer vision tricks to keep an eye on the driver's face. If it spots signs of sleepiness, like drooping eyelids or a head that's turned away—it sounds a loud buzzer to snap the driver back to attention before things go wrong.

---

## Why This Exists

You've probably heard the stats before. The National Highway Traffic Safety Administration estimates that drowsy driving is a factor in roughly 100,000 police-reported crashes every year in the U.S. alone. Those incidents result in over 1,500 deaths and tens of thousands of injuries. 

The tricky part? Many drowsy-driving cases fly under the radar because it's almost impossible to prove fatigue after a crash happens. That gap between what gets reported and what actually happens—is exactly what this project aims to address. A simple, always-on monitor could make a real dent in those numbers.

---

## Tools We Used

Here's the tech stack that makes this work:

- **OpenCV** – Does all the heavy lifting for image capture and real-time processing.
- **imutils** – A handy set of helpers that makes OpenCV a little less painful to work with.
- **Dlib** – Supplies the facial landmark detection that helps us locate eyes and other features.
- **scikit-learn** & **NumPy** – Handle the number crunching and any lightweight ML tasks behind the scenes.

---

## Quick Start Guide

Get the system up and running on your machine in just a few steps.

**1. Clone the repository**

```bash
git clone https://github.com/viswanathanganesh24/real-time-driver-alertness-monitoring-system.git
```

**2. (Optional) Set up a virtual environment**

Keeping dependencies isolated is always a good idea. Here's how:

```bash
pip install virtualenv
virtualenv -p python3 env
```

Activate it:

- **Windows**:  
  ```bash
  env\Scripts\activate
  ```
- **Linux / Mac**:  
  ```bash
  source env/bin/activate
  ```

**3. Install the required packages**

```bash
pip install -r requirements.txt
```

**4. Fire up the monitor**

```bash
python app.py --shape-predictor shape_predictor.dat --alarm Alert.wav
```

That's it. Point your webcam at yourself and test it out.

---

## How the Detection Loop Works

Here's what happens inside the main processing loop, frame by frame:

1. **Grab a frame** – The system pulls a live feed from the default camera.

2. **Look for a face** – A Haar cascade classifier scans the frame. If no face is found, the driver is either turned away or distracted—so the buzzer goes off immediately.

3. **Zoom in on the eyes** – Once a face is locked, the system runs a second Haar cascade to locate the eye regions within that face crop.

4. **Check for sideways glances** – If both eyes aren't visible, the system assumes the driver is looking elsewhere (not at the road) and triggers the alarm.

5. **Examine pupil position** – For each detected eye, Hough Circle Transform is used to find the pupil. This tells us whether the eyelid is covering the pupil or if the eye is fully open.

6. **Track state over time** – The open/closed status of each eye is logged for every frame. If the system registers closed eyes for **five consecutive frames**, that's a clear drowsiness signal.

7. **Trigger the alarm** – The moment that threshold is crossed, the audio buzzer (`Alert.wav`) kicks in to warn the driver.

---

## Final Note

This project isn't meant to replace proper rest or safe driving habits—but it can serve as an extra layer of safety for long hauls or late-night drives. Feel free to fork it, tweak it, and make it your own.
