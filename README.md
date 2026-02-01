# 🛡️ SafeDrive Guardian System

**SafeDrive Guardian** is a real-time driver fatigue detection system that bridges Computer Vision with hardware alerts. It monitors eye-blink patterns through a webcam and triggers physical alarms via an Arduino if the driver shows signs of drowsiness.

---

## 🚀 How It Works

The system utilizes a combination of image processing and hardware integration:

1.  **Face Detection:** Uses `dlib`'s pre-trained HOG + Linear SVM face detector.
2.  **Landmark Mapping:** Identifies 68 specific facial coordinates (landmarks).
3.  **EAR Calculation:** It calculates the **Eye Aspect Ratio**. When the vertical distance between eyelids decreases significantly, the ratio increases, signaling a "blink" or "closed eye."
4.  **Hardware Response:** If eyes remain closed for a threshold of 10 consecutive frames, the system sends a signal to an Arduino to activate safety peripherals.



---

## 🛠️ Requirements

### Software
* Python 3.x
* `opencv-python`
* `dlib`
* `numpy`
* `pyfirmata`

### Hardware
* **Webcam** (Integrated or External)
* **Arduino Board** (Uno/Mega/Nano)
* **Safety Peripherals:** 2x LEDs or a Buzzer connected to Digital Pins 2 and 3.

---

## 🔧 Installation & Setup

1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/sayanah1121/SafeDrive-Guardian-System.git](https://github.comsayanah1121/SafeDrive-Guardian-System.git)
    cd SafeDrive-Guardian-System
    ```

2.  **Install Dependencies:**
    ```bash
    pip install opencv-python dlib numpy pyfirmata
    ```

3.  **Download Shape Predictor:**
    Download the `shape_predictor_68_face_landmarks.dat` file from the [dlib repository](http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2), extract it, and place it in the project folder.

4.  **Arduino Configuration:**
    * Connect your Arduino to your computer.
    * Open the Arduino IDE.
    * Navigate to **File > Examples > Firmata > StandardFirmata**.
    * Upload the sketch to your board.
    * Check your COM Port (e.g., `COM17`) and update line 9 in the script if necessary.

---

## 💻 Usage

Run the detection script:
```bash
python main.py
System Logic:Eye StateLogicHardware ActionOpenRatio < 5.7LEDs OFF (Pins 2 & 3 LOW)BlinkingRatio > 5.7"blink" displayed on screenFatigueRatio > 5.7 (for 10+ frames)"CLOSE" displayed + LEDs ON📊 Technical MathematicsThe system relies on the Eye Aspect Ratio (EAR) logic. For each eye, 6 landmarks are identified:Horizontal Line ($L_h$): The distance between the corners of the eye (Landmarks 36 & 39).Vertical Line ($L_v$): The distance between the center of the upper and lower eyelids.The Equation:The distance between two points $P_1(x_1, y_1)$ and $P_2(x_2, y_2)$ is calculated using the Euclidean distance formula:$$d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$$In this implementation, the Blinking Ratio is defined as:$$Ratio = \frac{\text{Horizontal distance}}{\text{Vertical distance}}$$When the eye closes, the vertical distance decreases (approaching zero), causing the total Ratio to increase. If this value stays above the threshold of 5.7 for 10 frames, the system triggers the fatigue alarm.
