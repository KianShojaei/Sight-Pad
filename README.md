# Sight-Pad
𝑹𝒆𝒗𝒐𝒍𝒖𝒕𝒊𝒐𝒏𝒊𝒛𝒆 𝒚𝒐𝒖𝒓 𝒅𝒆𝒗𝒊𝒄𝒆 𝒄𝒐𝒏𝒕𝒓𝒐𝒍 𝒘𝒊𝒕𝒉 𝒊𝒏𝒕𝒖𝒊𝒕𝒊𝒗𝒆 𝒉𝒂𝒏𝒅 𝒈𝒆𝒔𝒕𝒖𝒓𝒆𝒔 – 𝒏𝒐 𝒕𝒐𝒖𝒄𝒉𝒑𝒂𝒅 𝒏𝒆𝒆𝒅𝒆𝒅!





---

# Hand Gesture Control for Mouse and Zoom

This project allows users to control mouse movements, perform clicks, right-clicks, scroll, and zoom operations using hand gestures via a webcam. The implementation is powered by **MediaPipe** for hand tracking and **PyAutoGUI** for system interaction. The program processes hand gestures in real-time to perform corresponding actions such as moving the mouse, clicking, scrolling, and zooming.

## Table of Contents

- [Features](#features)
- [Requirements](#requirements)
- [Setup](#setup)
- [How It Works](#how-it-works)
  - [Webcam and Screen Setup](#webcam-and-screen-setup)
  - [Hand Tracking with MediaPipe](#hand-tracking-with-mediapipe)
  - [Mouse Control](#mouse-control)
  - [Click and Double-Click Detection](#click-and-double-click-detection)
  - [Right-Click Detection](#right-click-detection)
  - [Scrolling and Zooming](#scrolling-and-zooming)
- [Key Variables](#key-variables)
- [Limitations](#limitations)
- [Future Improvements](#future-improvements)
- [License](#license)
- [Contributing](#contributing)

## Features
- **Mouse Control**: Move the mouse with your right hand's index finger.
- **Left/Right Click Detection**: Perform left or right-clicks using specific hand gestures.
- **Double Click**: Detect double-clicks using left hand's index finger positions.
- **Scroll Control**: Scroll up or down using pinching gestures.
- **Zoom Control**: Zoom in or out by bringing both hands together and changing their distance.

## Requirements

You will need the following libraries installed:

- OpenCV (`cv2`)
- MediaPipe
- PyAutoGUI
- NumPy
- Threading (comes with Python)

You can install these dependencies with:
```bash
pip install opencv-python mediapipe pyautogui numpy
```

## Setup

1. **Clone the repository** and navigate into the project folder:
    ```bash
    git clone https://github.com/KianShojaei/Sight-Pad.git
    cd hand-gesture-control
    ```

2. **Install the dependencies** as mentioned above.

3. **Run the script**:
    ```bash
    python hand_control.py
    ```

    The program will activate your webcam and start tracking your hand gestures. Press **`q`** to quit the program.

## How It Works

### Webcam and Screen Setup

The program initializes the webcam and configures it to capture frames in 640x480 resolution for performance efficiency. It also fetches the screen size to map the coordinates of your hand movements to mouse movements.

```python
cap = cv2.VideoCapture(0)
cap.set(3, 640)
cap.set(4, 480)

screen_width, screen_height = pyautogui.size()
```

### Hand Tracking with MediaPipe

**MediaPipe** is used to detect hand landmarks (finger positions). Based on the position of the index and thumb fingers, different actions like mouse movement, clicks, or scroll are triggered.

```python
mp_hands = mp.solutions.hands
hands = mp_hands.Hands(max_num_hands=2)
```

### Mouse Control

Your right hand controls the mouse pointer. The position of the index finger tip on the right hand is tracked, and the mouse is moved accordingly using **PyAutoGUI**.

```python
x = int(index_finger_tip.x * screen_width)
y = int(index_finger_tip.y * screen_height)
pyautogui.moveTo(x, y)
```

### Click and Double-Click Detection

- **Left Click**: Detected by pinching the thumb and index finger of your left hand.
- **Double Click**: Detected when the left-hand fingers are held in a specific position and can only happen once every two seconds to avoid repetitive double-clicks.

```python
if distance < click_threshold:
    pyautogui.mouseDown()  # Left-click down
elif distance > release_threshold:
    pyautogui.mouseUp()  # Release the click
```

### Right-Click Detection

Right-click is detected when your right hand's **thumb** and **pinky** fingers touch.

```python
if thumb_pinky_distance < 0.05:
    pyautogui.rightClick()  # Perform right-click
```

### Scrolling and Zooming

- **Scrolling**: Activate scroll by pinching together the thumb, middle, and ring fingers of your left hand.
- **Zooming**: Use both hands to zoom. Bringing both index fingers together activates zoom mode, and changing the distance between your hands zooms in or out.

```python
if scrolling_active:
    pyautogui.scroll(-100)  # Scroll down
```

For zoom:
```python
if zoom_delta > 0:
    pyautogui.hotkey('ctrl', '+')  # Zoom in
else:
    pyautogui.hotkey('ctrl', '-')  # Zoom out
```

## Key Variables

- **`frame_skip`**: The program skips frames to speed up processing. Every third frame is processed.
- **`click_threshold`**: The threshold distance for detecting a click gesture.
- **`scrolling_active`**: A toggle to enable or disable scrolling functionality.
- **`zoom_scale_factor`**: Sensitivity of the zoom gesture.

## Limitations

- **Lighting Conditions**: The accuracy of hand tracking may vary based on lighting.
- **Performance**: High frame sizes or slow machines may result in lag.

## Future Improvements

- Improve gesture sensitivity and allow users to customize gestures.
- Add additional gestures for other functionalities such as volume control, window management, or keyboard shortcuts.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2024 Kian

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## Contributing

Contributions are welcome! Please open an issue to discuss the change you wish to make, and feel free to open a pull request.

---
