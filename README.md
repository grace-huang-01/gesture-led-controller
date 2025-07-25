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

Click 'Get Started' and sign up for a free Developer Plan. 

> Note: if you plan on collecting data using your board, you will need to set a password for your account at some point. You can always add a password later through your account profile, if needed.

<img width="942" height="430" alt="image" src="https://github.com/user-attachments/assets/9aae167b-a64d-4846-b660-3416295c0f18" />  

### 2. Connecting to Edge Impulse

Edge Impulse allows you to collect your own data from a supported board (like ours). To use this feature, you will need to connect your board through the Edge Impulse CLI. If you already have the CLI installed or you plan on directly uploading this repo's data, open a new project and skip ahead to the next section. For first-time users, this setup procedure can be a little tricky, so read the instructions carefully. 

**Summary of What to Expect**

First, you will need to install some dependencies linked in the installation docs (linked below). After you finish, you should have the following softwares installed:
- Python3
- Node.js + Tools for Native Modules
- Edge Impulse CLI
- Arduino CLI

Then, follow steps 1-4 in the installation docs to connect your board, flash it with the correct firmware, run the Edge Impulse daemon, and link your board to a new project. 

Installation Docs: https://docs.edgeimpulse.com/docs/edge-ai-hardware/mcu/arduino-nano-33-ble-sense

**ATTENTION WINDOWS USERS:** Please read these tips before starting your installs. Don't make the same mistakes I did!
> - When installing Node.js, make sure you select the check box for installing Tools for Native Modules. This will open up another script to install Chocolatey once Node.js finishes installing.
> <img width="382" height="298" alt="image" src="https://github.com/user-attachments/assets/87ae2a96-732a-4e20-8d35-615a9f008786" />

> - When installing Chocolatey, it's good to close all other tabs and applications and just let it run. It's memory-hungry and will take a bit.
> - If for some reason you accidentally close the Chocolatey script, you can find it again by searching 'Install Additional Tools for Node.js' in the Start menu.
> <img width="1156" height="622" alt="image" src="https://github.com/user-attachments/assets/07bb81d0-ee11-486e-b5f6-8f39ef26140e" />

> - Visual Studio Build Tools will automatically be installed during the "Tools for Native Modules" installation process. If you get a massive error during Edge Impulse CLI installation saying that a lot of VS elements can't be found, there's a chance your VS Build Tools installation was interrupted before completion. Open Visual Studio Installer from the Start menu and check the status of your VS Build Tools installation. If it's paused, resume the installation. Once it finishes, run `npm uninstall -g edge-impulse-cli` and then `npm install -g edge-impulse-cli` to try again.
> - Note: The Edge Impulse firmware is necessary to run the Edge Impulse CLI daemon. So if you've flashed a different script to your Arduino board since the last time you connected to Edge Impulse, don't forget to flash this firmware again before running the daemon.

When you have your Arduino board connected to a new project, the 'Devices' tab will look something like this: 

<img width="722" height="197" alt="image" src="https://github.com/user-attachments/assets/ca4a2bb8-67c1-4db0-b571-4862af3c4fba" />

### 3. Uploading Data

This section is for those of you who want to upload the data provided in this repo. If you have the Edge Impulse CLI installed and you're ready to collect your own data, skip down to the next section to begin data collection! 

- In your new project, go to the 'Data Acquisition' tab from the left menu. Click on '+ Add data' and select 'Upload data'.

<img width="403" height="369" alt="image" src="https://github.com/user-attachments/assets/67b80acb-b97f-4a06-a8ef-ad05b148792c" />

<img width="596" height="200" alt="image" src="https://github.com/user-attachments/assets/b366fde0-9e94-4052-9c07-c741b69831e5" />

- Set the upload mode to 'Select a folder' and upload the data folder from this repo. Leave the other settings below as is.

<img width="313" height="452" alt="image" src="https://github.com/user-attachments/assets/4615ebf1-31ec-428a-8429-9585447e3255" />

- After the upload is finished, you should have something like this:

<img width="751" height="461" alt="image" src="https://github.com/user-attachments/assets/97c17200-1ca3-4b57-bfc8-fb5426bb6dcc" />

- Now that the data is loaded, you are ready to begin building a model. Go to **5. Creating Your Impulse** to get started!

### 4. Collecting Data



<img width="347" height="365" alt="image" src="https://github.com/user-attachments/assets/b6abb39f-7739-4eb3-a200-d316d9c534fc" />

### 5. Creating Your Impulse

- Go under the 'Impulse Design' tab and open 'Create impulse'. 

<img width="1464" height="913" alt="image" src="https://github.com/user-attachments/assets/91de6e2e-6038-4970-b3e1-4b52c9469db8" />

- Click on 'Add a processing block' and select the Spectral Analysis block from the pop-up menu.

<img width="601" height="127" alt="image" src="https://github.com/user-attachments/assets/29c5f37a-6445-40ed-80e4-b0aa0d0be0a3" />

- Click on 'Add a learning block' and select the Classification block.

<img width="599" height="109" alt="image" src="https://github.com/user-attachments/assets/b08f32a4-ce1d-44ec-9b22-da404aa31d2a" />

- Your impulse should now look like this:

<img width="1456" height="900" alt="image" src="https://github.com/user-attachments/assets/2ddb391d-aa02-40f8-af16-2cb13b86cc98" />

- Save your impulse.

### 6. Generating Features

When you saved your impulse, two new tabs were added under 'Impulse Design' called 'Spectral Features' and 'Classifier'. 

<img width="158" height="131" alt="image" src="https://github.com/user-attachments/assets/e1b0e7aa-43c4-41c5-8c3b-da558b3878f6" />

- Open 'Spectral Features'. Here you will be able to visualize your raw and post-DSP data.

<img width="746" height="412" alt="image" src="https://github.com/user-attachments/assets/d66a5a96-7f10-4766-aca9-59cfe8ebfbdf" />

- Switch to the 'Generate Features' tab and click 'Generate Features'.

<img width="356" height="383" alt="image" src="https://github.com/user-attachments/assets/12b5229b-5473-44dd-826f-3ae887b7455a" />

- The Feature Explorer allows you to visualize the spread of your labels. If your labels are in visually distinct clusters, that's a good sign.

<img width="349" height="394" alt="image" src="https://github.com/user-attachments/assets/604aae7a-f743-472b-9736-6b6fa158e6a5" />

### 7. Training Your Model

- Under 'Impulse Design' again, open 'Classifer'. We are going to leave the default Neural Network settings as is.
- Scroll down to Neural Network architecture. Click on 'Add an extra layer'.

  <img width="341" height="344" alt="image" src="https://github.com/user-attachments/assets/52247404-fc94-4b94-ad89-f3f4e939b50c" />

- Add a Dense layer and assign 3 neurons to it.
<img width="596" height="220" alt="image" src="https://github.com/user-attachments/assets/66a087c1-7dd2-4e8a-bb6c-e18c5bd85510" />
<img width="383" height="289" alt="image" src="https://github.com/user-attachments/assets/72848a14-fa45-4783-ab21-2b41a638484b" />

- Save and train your neural network.

  <img width="344" height="380" alt="image" src="https://github.com/user-attachments/assets/1f65bb50-4b3d-4345-959e-e5e13a360e23" />

- Once the training completes, you will be able to see your model's performance in both statistical and graphical form.

<img width="777" height="915" alt="image" src="https://github.com/user-attachments/assets/0623891d-c2ca-46df-805b-f37184d986ae" />
<img width="388" height="400" alt="image" src="https://github.com/user-attachments/assets/52671245-f82e-4b5e-b9fc-c4274fa1a93f" />

### 8. Testing Your Model

- Under 'Impulse Design', open 'Model testing'.
- Click 'Classify all' to run the model on your test data.

<img width="350" height="320" alt="image" src="https://github.com/user-attachments/assets/3c40552c-fc5a-490b-b9fe-c712a1c3ca8f" />

- As with the training stage, you will be able to see your model's testing performance in the tables and graph.

<img width="766" height="891" alt="image" src="https://github.com/user-attachments/assets/6e3289a2-2ca1-4a33-805c-75d2d5de0e3b" />
<img width="386" height="290" alt="image" src="https://github.com/user-attachments/assets/fe66e500-e66f-4569-af27-70041ab60738" />

### 9. Deploying Your Model

- Under 'Impulse Design', open 'Deployment'.
- In the configuration box, search for 'Arduino Library' and set it as your deployment option.

<img width="426" height="307" alt="image" src="https://github.com/user-attachments/assets/a1ca258b-30f5-46fa-b3a1-7abe6990afb1" />

- Scroll down to 'Model Optimizations' and click on 'Run model testing' to compare the performance of the optimized and unoptimized versions of your model.

<img width="421" height="412" alt="image" src="https://github.com/user-attachments/assets/0caad26d-7c45-41b2-8b95-e60d075e45ab" />

- Based on the results, select which model you'd like to deploy and press 'Build'.

<img width="421" height="317" alt="image" src="https://github.com/user-attachments/assets/7bcad926-f97e-4dc0-9802-6eafcb2914f6" />

- Once your build has finished, you'll get this pop up and a zip file download.

<img width="542" height="269" alt="image" src="https://github.com/user-attachments/assets/fd6cd0bf-8997-4a35-8534-d5ee199056a6" />

- Open your Arduino IDE

### 10. LED Controller

## License

This repository is licensed under the Apache License 2.0. See the [LICENSE](LICENSE) file for details.
