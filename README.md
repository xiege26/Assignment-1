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
I used the system Python 3.10 and used software-based OpenGL rendering to handle 3D rendering in the virtual machine.

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

Run these commands and paste the actual terminal output (not just screenshots):

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
_[Include one screenshot showing both tests passing]_

![Python Tests Passing](path/to/your/screenshot.png)

---

## 3. ROS 2 Workspace Check

### 3.1 Build the workspace

Paste the build output summary (final lines only):

```bash
source /opt/ros/humble/setup.bash
colcon build
```

**Expected output:**
```
Summary: 1 package finished [x.xx s]
```

**Your actual output:**
```
Starting >>> env_check_pkg
Finished <<< env_check_pkg [0.29s]

Summary: 1 package finished [0.79s]
```

### 3.2 Run talker and listener

Show both source commands:

```bash
source /opt/ros/humble/setup.bash
source install/setup.bash
```

**Then run talker:**
```bash
ros2 run env_check_pkg talker
```

**Output (3–4 lines):**
```
[INFO] [1769078204.658629930] [env_check_pkg_talker]: AAE5303 talker ready (publishing at 2 Hz).
[INFO] [1769078205.158809409] [env_check_pkg_talker]: Publishing: 'AAE5303 hello #0'
[INFO] [1769078205.658808282] [env_check_pkg_talker]: Publishing: 'AAE5303 hello #1'
[INFO] [1769078206.158846655] [env_check_pkg_talker]: Publishing: 'AAE5303 hello #2'
```

**Run listener:**
```bash
ros2 run env_check_pkg listener
```

**Output (3–4 lines):**
```
[INFO] [1769078211.771682191] [env_check_pkg_listener]: AAE5303 listener awaiting messages.
[INFO] [1769078214.296471027] [rclcpp]: signal_handler(SIGINT/SIGTERM)
```

**Alternative (using launch file):**
```bash
ros2 launch env_check_pkg env_check.launch.py
```

**Screenshot:**  
_[Include one screenshot showing talker + listener running]_

![Talker and Listener Running](path/to/your/screenshot.png)

---

## 4. Problems Encountered and How I Solved Them

> **Note:** Write 2–3 issues, even if small. This section is crucial — it demonstrates understanding and problem-solving.

### Issue 1: OpenCV and Open3D failed with "libGL.so.1: cannot open shared object file"

**Cause / diagnosis:**  
The error occurred because OpenCV and Open3D require OpenGL libraries (libGL.so.1) for graphics operations, but these libraries were not installed in the WSL2 Ubuntu environment. This is a common issue in minimal Linux installations where graphics libraries are not included by default.

**Fix:**  
Installed the required OpenGL libraries using apt:

```bash
apt install -y libgl1-mesa-glx libglib2.0-0
```

**Reference:**  
StackOverflow and OpenCV documentation for WSL2 setup

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

### Issue 3: ROS 2 repository GPG key verification failed

**Cause / diagnosis:**  
When trying to add the ROS 2 repository, the GPG key was not properly imported, causing apt to fail with "The following signatures couldn't be verified because the public key is not available: NO_PUBKEY F42ED6FBAB17C654".

**Fix:**  
Removed the corrupted key file and re-downloaded it properly:

```bash
rm -f /usr/share/keyrings/ros-archive-keyring.gpg
curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key | gpg --dearmor -o /usr/share/keyrings/ros-archive-keyring.gpg
apt update
```

**Reference:**  
Official ROS 2 Humble installation guide

---

## 5. Use of Generative AI (Required)

Choose one of the issues above and document how you used AI to solve it.

> **Goal:** Show critical use of AI, not blind copying.

### 5.1 Exact prompt you asked

**Your prompt:**
```
为什么docker显示没有wsl？
```

### 5.2 Key helpful part of the AI's answer

**AI's response (relevant part only):**
```
1检查并安装/启用 WSL：
这是最常见的原因。你需要确保 Windows 已经安装了 WSL 2。
使用管理员权限打开 PowerShell 或 CMD。
输入以下命令并回车：wsl –install

2手动检查 Windows 功能：
有时候命令行安装不完整，需要手动勾选功能：
1.	按 Win + R，输入 optionalfeatures，回车。
2.	在弹出的窗口中，确保以下两个选项都已勾选：
o	✅ Windows Subsystem for Linux (适用于 Linux 的 Windows 子系统)
o	✅ Virtual Machine Platform (虚拟机平台)
3.	点击确定，安装完成后重启电脑。

3在 Docker Desktop 设置中启用
如果上述都在系统层面搞定了，但 Docker 还是报错，可能是 Docker 没配置好：
1.	打开 Docker Desktop。
2.	进入 Settings (设置齿轮) -> General。
3.	勾选 Use the WSL 2 based engine。
4.	点击 Apply & restart。
```

### 5.3 What you changed or ignored and why

Explain briefly:
- Did the AI recommend something unsafe?
- Did you modify its solution?
- Did you double-check with official docs?

**Your explanation:**  
The AI's answer was helpful for understanding WSL2 setup, but in my case, I was already using WSL2 successfully. The actual problem I encountered was different - it was about missing system libraries (libGL.so.1) in the Linux environment, not about Docker or WSL2 installation. I used the AI's general troubleshooting approach (checking system components step by step) but applied it to the specific library installation issue. I verified the solution by checking the official Ubuntu package documentation before installing the libraries.

### 5.4 Final solution you applied

Show the exact command or file edit that fixed the problem:

```bash
apt install -y libgl1-mesa-glx libglib2.0-0
```

**Why this worked:**  
This command installed the OpenGL libraries that OpenCV and Open3D require for graphics operations. The `libgl1-mesa-glx` package provides the libGL.so.1 library, and `libglib2.0-0` provides additional GLib utilities needed by the graphics stack. After installation, both OpenCV and Open3D were able to load successfully.

---

## 6. Reflection (3–5 sentences)

Short but thoughtful:

- What did you learn about configuring robotics environments?
- What surprised you?
- What would you do differently next time (backup, partitioning, reading error logs, asking better AI questions)?
- How confident do you feel about debugging ROS/Python issues now?

**Your reflection:**

As a beginner, I learned that configuring robotics environments requires patience and careful attention to details, especially with ROS and Python. I was surprised by how many small issues can prevent the system from running correctly. It took me many attempts to get everything working, and next time I would focus more on backing up configurations, organizing my workspace, reading error logs carefully, and asking clearer questions to AI or online forums. After this experience, I feel more confident in debugging ROS and Python problems, even though I know I still have a lot to learn.

---

## 7. Declaration

✅ **I confirm that I performed this setup myself and all screenshots/logs reflect my own environment.**

**Name:**  
Xiaohan WANG

**Student ID:**  
25047428G

**Date:**  
22/01/2026
