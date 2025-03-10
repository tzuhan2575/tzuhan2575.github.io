---
title: "Automate Mouse Movement with Python"
date: 2025-03-10T14:00:19+08:00
draft: false
tags: ["Python", "Automation", "Mouse Control", "PyAutoGUI"]
categories: ["Tech Guide", "Python"]
summary: "Learn how to use Python and PyAutoGUI to automate mouse movements and random delays."
weight: 1
slug: "python-mouse-automation"
---

## 🚀 Introduction

Sometimes, we need to keep our computer active without manual interaction. This can be useful for:

- Preventing the screen from sleeping
- Keeping a remote session active
- Automating simple tasks

In this guide, we will learn how to use Python to move the mouse automatically on Mac and solve common permission issues.

---

### Step 1: Install Required Packages

We will use pyautogui to control the mouse. First, install it using pip:

```shell
pip install pyautogui
```

This package allows Python to simulate mouse movements and clicks.

### Step 2: Write a Simple Mouse Movement Script

Now, let’s write a basic Python script to move the mouse randomly every few minutes.

```shell
import pyautogui
import time
import random

while True:
    x, y = random.randint(100, 1000), random.randint(100, 700)
    pyautogui.moveTo(x, y, duration=1)
    print(f"Mouse moved to: ({x}, {y})")
    time.sleep(random.randint(300, 600))  # Wait 5-10 minutes
```

This script will:

1. Move the mouse to a random position (between x: 100-1000, y: 100-700)
2. Move smoothly in 1 second
3. Wait for 5-10 minutes before moving again

Run the script:

```shell
python mouse.py
```

---

## 🔧 Fix Common Issues

### ❌ Issue 1: Fix Mac Security Issues

If you see this error:

```shell
This process is not trusted! Input event monitoring will not be possible until it is added to accessibility clients.
```

It means MacOS does not allow Python to control the mouse.

✅ Solution: Grant Accessibility Permissions

1. Open System Settings → Privacy & Security
2. Click Accessibility
3. Enable Terminal (or your Python IDE) to allow control

After enabling, restart the script and it should work!

---

## Extended

### Stop the Script with ESC Key

Right now, the script runs forever. Let’s modify it to stop when we press the ESC key.

```shell
import pyautogui
import time
import random
from pynput import keyboard

stop_flag = False  # Stop condition

def on_press(key):
    global stop_flag
    if key == keyboard.Key.esc:
        stop_flag = True
        print("Exiting program...")

listener = keyboard.Listener(on_press=on_press)
listener.start()

while not stop_flag:
    x, y = random.randint(100, 1000), random.randint(100, 700)
    pyautogui.moveTo(x, y, duration=random.uniform(0.5, 2.0))
    print(f"Mouse moved to: ({x}, {y})")
    time.sleep(random.randint(300, 600))
```

Now, you can press ESC to stop the script safely.

---

## 🎯 Conclusion

Now you have a simple Python mouse automation script! You learned how to:

- ✅ Move the mouse automatically
- ✅ Solve Mac security permission issues
- ✅ Stop the script with ESC

This script can be used for:

- Keeping your computer awake
- Remote desktop automation
- Simple user simulation

Try it out and modify it for your needs! 🚀
