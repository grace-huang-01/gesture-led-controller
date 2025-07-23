# Gesture LED Controller

This project trains a gesture classifier to control the color of an LED on the Arduino Nano 33 BLE Sense Lite board.

## Overview

This is a simple TinyML project that enables gesture-based LED control on the Arduino Nano 33 BLE Sense Lite board. You will be training a small neural network classifier through Edge Impulse to distinguish between up-down, left-right, and idle gestures. All the necessary data can be collected from the board's built-in IMU sensor using the Edge Impulse CLI. The deployed model performs real-time inference on the board, the results of which are used to change the LED's color.

This hands-free gesture control can be expanded for wearable tech, interactive installations, or accessibility devices where traditional input methods are limited.

## Attribution & License

This project is based on and modifies code from the [Edge Impulse Ingestion SDK](https://github.com/edgeimpulse/example),  
licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).

© 2022 EdgeImpulse Inc.  
Modifications and additions by Grace H., 2025.

## Hardware

- Arduino Nano 33 BLE Sense Lite

> Note: I am using the "Lite" version from Prof. Vijay Janapa Reddi (Harvard University) and Pete Warden (Google). This project is compatible with the regular Arduino Nano 33 BLE Sense as well as the Arduino Nano 33 BLE Sense Rev2. Just make sure to follow your board-specific instructions and you should be okay!

## Getting Started

Here is a step-by-step guide for getting started on this project. I'll be walking you through setting up Edge Impulse, collecting your own gesture data, training and deploying your model, and coding the LED controller. I'll also be giving you tips and tricks for resolving some bugs that you might run into along the way (they certainly got me lol). With no further ado, let's get started!

### 1. Create an Edge Impulse account

Edge Impulse is a powerful online tool that allows you to interface your board for data collection, preprocess data, and execute the full ML lifecycle — all without requiring a single line of code.

If you are new to Edge Impulse, you will need to create a free account at https://edgeimpulse.com/.

<img width="1890" height="927" alt="image" src="https://github.com/user-attachments/assets/c6c7d12c-2eee-40f3-99be-0fbf8aec1c37" />
&nbsp;

Click 'Get Started' and sign up for a free Developer Plan. 

> Note: if you plan on collecting data using your board, you will need to set a password for your account at some point. You can always add a password later through your account profile, if needed.

<img width="942" height="430" alt="image" src="https://github.com/user-attachments/assets/9aae167b-a64d-4846-b660-3416295c0f18" />  

### 2. Install necessary software

Edge Impulse allows you to collect your own data from a supported board (like ours). To use this feature, you will need to connect your board using the Edge Impulse CLI. If you already have the CLI installed or you plan on directly uploading this repo's data, go ahead and skip to the next section. For first-time users, installing the CLI (and its dependencies) can be a little tricky, so read the documentation carefully (instructions linked below). 

**ATTENTION WINDOWS USERS:** Please read these tips before starting your installs. Don't make the same mistakes I did!
> - When installing Node.js, make sure you select the check box for installing Tools for Native Modules. This will open up another script to install Chocolatey once Node.js finishes installing.
> <img width="382" height="298" alt="image" src="https://github.com/user-attachments/assets/87ae2a96-732a-4e20-8d35-615a9f008786" />

> - When installing Chocolatey, it's good to close all other tabs and applications and just let it run. It's memory-hungry and will take a bit.
> - If for some reason you accidentally close the Chocolatey script, you can find it again by searching 'Install Additional Tools for Node.js' in the Start menu.
> <img width="1156" height="622" alt="image" src="https://github.com/user-attachments/assets/07bb81d0-ee11-486e-b5f6-8f39ef26140e" />

> - Visual Studio Build Tools will automatically be installed during the "Tools for Native Modules" installation process. If, when you run `npm install -g edge-impulse-cli --force`, you get a massive error where a lot of elements pertaining to VS can't be found, there's a chance your VS Build Tools installation was interrupted before completion. Open Visual Studio Installer from the Start menu and check the status of your VS Build Tools installation. If it's paused, resume the installation. Once it finishes, run `npm uninstall -g edge-impulse-cli` and then `npm install -g edge-impulse-cli` to try again.

**Go to the installation docs:** https://docs.edgeimpulse.com/docs/edge-ai-hardware/mcu/arduino-nano-33-ble-sense

After you finish, you should have the following softwares installed:
- Python3
- Node.js
- Tools for Native Modules
- Edge Impulse CLI
- Arduino CLI

### 3. Collecting Data



### 4. Training Your Model

### 5. Deploying Your Model

### 6. LED Controller

## License

This repository is licensed under the Apache License 2.0. See the [LICENSE](LICENSE) file for details.
