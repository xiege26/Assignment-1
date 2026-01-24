# Assignment-1
# AAE5303 Environment Setup Report



## 1. System Information

**Laptop model:**  
Legion R7000P2021H

**CPU / RAM:**  
AMD Ryzen 7 5800H with Radeon Graphics / 16GB RAM

**Host OS:**  
Windows 11

**Linux/ROS environment type:**  
WSL2 Ubuntu


---

## 2. Python Environment Check

### 2.1 Steps Taken
I used Python 3.10.12 in a virtual environment (.venv) on WSL2. This satisfies the course requirement of Python 3.10+.

**Tool used:**  
venv

**Key commands:**
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

**Any deviations from the default instructions:**  
None

### 2.2 Test Results

```bash
python scripts/test_python_env.py
```

**Output:**
```
========================================
AAE5303 Environment Check (Python + ROS)
Goal: help you verify your environment and understand what each check means.
========================================

Step 1: Environment snapshot
  Why: We capture platform/Python/ROS variables to diagnose common setup mistakes (especially mixed ROS env).
Step 2: Python version
  Why: The course assumes Python 3.10+; older versions often break package wheels.
Step 3: Python imports (required/optional)
  Why: Imports verify packages are installed and compatible with your Python version.
Step 4: NumPy sanity checks
  Why: We run a small linear algebra operation so success means more than just `import numpy`.
Step 5: SciPy sanity checks
  Why: We run a small FFT to confirm SciPy is functional (not just installed).
Step 6: Matplotlib backend check
  Why: We generate a tiny plot image (headless) to confirm plotting works on your system.
Step 7: OpenCV PNG decoding (subprocess)
  Why: PNG decoding uses native code; we isolate it so corruption/codec issues cannot crash the whole report.
Step 8: Open3D basic geometry + I/O (subprocess)
  Why: Open3D is a native extension; ABI mismatches can segfault. Subprocess isolation turns crashes into readable failures.
Step 9: ROS toolchain checks
  Why: The course requires ROS tooling. This check passes if ROS 2 OR ROS 1 is available (either one is acceptable).
  Action: building ROS 2 workspace package `env_check_pkg` (this may take 1-3 minutes on first run)...
  Action: running ROS 2 talker/listener for a few seconds to verify messages flow...
Step 10: Basic CLI availability
  Why: We confirm core commands exist on PATH so students can run the same commands as in the labs.

=== Summary ===
✅ Environment: {
  "platform": "Linux-6.6.87.2-microsoft-standard-WSL2-x86_64-with-glibc2.35",
  "python": "3.10.12",
  "executable": "/root/PolyU-AAE5303-env-smork-test/.venv/bin/python",
  "cwd": "/root/PolyU-AAE5303-env-smork-test",
  "ros": {
    "ROS_VERSION": "2",
    "ROS_DISTRO": "humble",
    "ROS_ROOT": null,
    "ROS_PACKAGE_PATH": null,
    "AMENT_PREFIX_PATH": "/root/ros2_ws/install/env_check_pkg:/opt/ros/humble",
    "CMAKE_PREFIX_PATH": "/root/ros2_ws/install/env_check_pkg"
  }
}
✅ Python version OK: 3.10.12
✅ Module 'numpy' found (v2.2.6).
✅ Module 'scipy' found (v1.15.3).
✅ Module 'matplotlib' found (v3.10.8).
✅ Module 'cv2' found (v4.13.0).
✅ Module 'rclpy' found (vunknown).
✅ numpy matrix multiply OK.
✅ numpy version 2.2.6 detected.
✅ scipy FFT OK.
✅ scipy version 1.15.3 detected.
✅ matplotlib backend OK (Agg), version 3.10.8.
✅ OpenCV OK (v4.13.0), decoded sample image 128x128.
✅ Open3D OK (v0.19.0), NumPy 2.2.6.
✅ Open3D loaded sample PCD with 8 pts and completed round-trip I/O.
✅ ROS 2 CLI OK: /opt/ros/humble/bin/ros2
✅ ROS 1 tools not found (acceptable if ROS 2 is installed).
✅ colcon found: /usr/bin/colcon
✅ ROS 2 workspace build OK (env_check_pkg).
✅ ROS 2 runtime OK: talker and listener exchanged messages.
✅ Binary 'python3' found at /root/PolyU-AAE5303-env-smork-test/.venv/bin/python3

All checks passed. You are ready for AAE5303 🚀
```

```bash
python scripts/test_open3d_pointcloud.py
```

**Output:**
```
ℹ️ Loading /root/PolyU-AAE5303-env-smork-test/data/sample_pointcloud.pcd ...
✅ Loaded 8 points.
   • Centroid: [0.025 0.025 0.025]
   • Axis-aligned bounds: min=[0. 0. 0.], max=[0.05 0.05 0.05]
✅ Filtered point cloud kept 7 points.
✅ Wrote filtered copy with 7 points to /root/PolyU-AAE5303-env-smork-test/data/sample_pointcloud_copy.pcd
   • AABB extents: [0.05 0.05 0.05]
   • OBB  extents: [0.08164966 0.07071068 0.05773503], max dim 0.0816 m
🎉 Open3D point cloud pipeline looks good.
```

**Screenshot:**  
<img width="1920" height="970" alt="image" src="https://github.com/user-attachments/assets/b1f07e8b-4776-4b61-9c50-a083996c8f0c" />
<img width="1171" height="208" alt="image" src="https://github.com/user-attachments/assets/0ba9c66f-2508-4937-88f3-2eaf2655323a" />


---

## 3. ROS 2 Workspace Check

### 3.1 Build the workspace

```
source /opt/ros/humble/setup.bash
colcon build
```

**Expected output:**
```
Summary: 1 package finished [x.xx s]
```

**Actual output:**
```
Starting >>> env_check_pkg
Finished <<< env_check_pkg [11.0s]

Summary: 1 package finished [11.3s]
```

### 3.2 Run talker and listener


```bash
source /opt/ros/humble/setup.bash
source install/setup.bash
```

**Then run talker:**
```bash
ros2 run env_check_pkg talker.py
```

**Output (3–4 lines):**
```
[INFO] [1769074829.378148244] [talker]: Publishing: 'Hello World! 1'
[INFO] [1769074829.378148244] [talker]: Publishing: 'Hello World! 2'
[INFO] [1769074829.378148244] [talker]: Publishing: 'Hello World! 3'
```

**Run listener:**
```bash
ros2 run env_check_pkg listener.py

```

**Output (3–4 lines):**
```
[INFO] [1769074829.378148244] [listener]: I heard: 'Hello World! 83'
[INFO] [1769074829.378148244] [listener]: I heard: 'Hello World! 84'
[INFO] [1769074829.378148244] [listener]: I heard: 'Hello World! 85'
```

**Screenshot:**  
<img width="694" height="274" alt="image" src="https://github.com/user-attachments/assets/21298a79-3c4d-4c4b-b8e0-29d485c8f93f" />
<img width="1199" height="600" alt="image" src="https://github.com/user-attachments/assets/88d6f547-4b9d-407c-9597-1a30105375e1" />
<img width="1199" height="600" alt="image" src="https://github.com/user-attachments/assets/68d9d65c-d31a-442a-bc59-c873e661304e" />

---

## 4. Problems Encountered and How I Solved Them

### Issue 1: colcon not found on PATH

**Cause / diagnosis:**  
```colcon``` was not installed initially in WSL2, causing ROS 2 workspace build to fail.

**Fix:**  
Installed ```colcon``` using apt:

```bash
sudo apt update
sudo apt install python3-colcon-common-extensions -y
```

**Reference:**  
Official ROS 2 Humble documentation, course instructions

---

### Issue 2: ROS 2 workspace build failed with "ModuleNotFoundError: No module named 'catkin_pkg'"

**Cause / diagnosis:**  
When running `colcon build`, the build system tried to use Python from the virtual environment, but `catkin_pkg` was not installed in that environment. The `catkin_pkg` module is required by ROS 2's build system to parse package.xml files.

**Fix:**  
Installed `catkin_pkg` in the virtual environment:

```bash
source .venv/bin/activate
pip install catkin_pkg
```

**Reference:**  
ROS 2 documentation on colcon build requirements

---

### Issue 3: ROS 2 workspace build fails on first try

**Cause / diagnosis:**  
Previous failed build artifacts caused ```env_check_pkg``` build to fail.

**Fix:**  
Cleaned build artifacts and rebuilt:

```bash
rm -rf build install log
colcon build
```

**Reference:**  
Course template instructions, ROS 2 colcon documentation

---

## 5. Use of Generative AI (Required)

### 5.1 Exact prompt you asked

**Prompt:**

I am building a ROS 2 workspace in WSL2 Ubuntu and get "ModuleNotFoundError: No module named 'catkin_pkg'". How can I fix this?


### 5.2 Key helpful part of the AI's answer

**AI's response (relevant part only):**

You need to install the 'catkin_pkg' Python module inside your current virtual environment using pip:
```
pip install catkin_pkg
```

### 5.3 What you changed or ignored and why

The AI’s suggestion was safe. I applied it exactly as recommended. I double-checked the fix by running ```colcon build``` successfully.

### 5.4 Final solution you applied

```bash
pip install catkin_pkg
```

**Why this worked:**  
```catkin_pkg``` provides the necessary Python interface for parsing package.xml files during the ROS 2 workspace build.

---

## 6. Reflection (3–5 sentences)

As a beginner, I learned that configuring robotics environments requires patience and careful attention to details, especially with ROS and Python. I was surprised by how many small issues can prevent the system from running correctly. It took me many attempts to get everything working, and next time I would focus more on backing up configurations, organizing my workspace, reading error logs carefully, and asking clearer questions to AI or online forums. After this experience, I feel more confident in debugging ROS and Python problems, even though I know I still have a lot to learn.

---

## 7. Declaration

✅ **I confirm that I performed this setup myself and all screenshots/logs reflect my own environment.**

**Name:**  
_[WANG Xiaohan]_

**Student ID:**  
_[25047428G]_

**Date:**  
_[2026-01-22]_
