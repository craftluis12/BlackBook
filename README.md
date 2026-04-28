# BlackBook
This Project creates a decentralize communication, while being able to communicate with teams and being able to see each other in a map 

# Table of Contents

- [Project Overview](#project-overview)
- [Hardware Needed](#hardware-needed)
- [Optional Hardware](#optional-hardware)
- [Software Needed](#software-needed)
- [System Overview](#system-overview)
- [Setting Up OpenTAK Server](#setting-up-opentak-server)
- [Flashing the Meshtastic Device](#flashing-the-meshtastic-device)
- [Setting Up Meshtastic for the Server](#setting-up-meshtastic-for-the-server)
- [Meshtastic ATAK Setup](#meshtastic-atak-setup)
- [Connecting ATAK to the Server Using SSL](#connecting-atak-to-the-server-using-ssl)
- [Server Diagram](#server-diagram)


# OpenTAK Server and Meshtastic Setup

# Project Overview

- This project documents the setup of an OpenTAK Server system using a Raspberry Pi 5, Meshtastic devices, ATAK, and a local wireless network. The goal of this setup is to create a portable communication and tracking system where ATAK devices can connect to an OpenTAK Server and share information through the network. The Raspberry Pi 5 is used to host OpenTAK Server. A mini wireless router provides the local network, and Meshtastic devices are used to support off-grid communication. ATAK is used on mobile devices to connect to the server, view shared location data, and communicate with other users on the system. This setup is useful for testing portable field communication, local team tracking, and server-based ATAK networking.

# Hardware Needed

| Hardware | Purpose | Link |
|---|---|---|
| Raspberry Pi 5 | Runs OpenTAK Server and manages connected ATAK clients | [Link to Buy](https://www.pishop.us/product/raspberry-pi-5-8gb/?src=raspberrypi) |
| Mini Wireless Router | Creates the local network used by the Raspberry Pi, ATAK devices, and Meshtastic gateway | [Link to Buy](https://www.amazon.com/dp/B07794JRC5?ref=ppx_yo2ov_dt_b_fed_asin_title) |
| Battery Bank | Powers the portable setup when wall power is not available | [Link to Buy](https://www.amazon.com/dp/B0DBG2D5DF?psc=1&pd_rd_i) |
| Meshtastic Device | Used for mesh communication and connection with the OpenTAK Server setup | [Link to Buy](https://www.amazon.com/ESP32-V3-Module-3000mAh-Battery/dp/B0D7HSHTNX?crid=1N6MK9LUGZFUP&dib=eyJ2IjoiMSJ9.Uf) |
| Phone or Tablet | Runs ATAK and connects to the OpenTAK Server | N/A |



# Software Needed

| Software | Purpose |
|---|---|
| OpenTAK Server | Main server used to manage ATAK connections and connected users |
| Ubuntu Desktop or Raspberry Pi OS | Operating system used on the Raspberry Pi 5 |
| Meshtastic Firmware | Firmware used on the Meshtastic device |
| Meshtastic Mobile App | Used to configure the Meshtastic device |
| ATAK | Android Team Awareness Kit app used for mapping, tracking, and communication |
| Meshtastic ATAK Plugin | Allows ATAK to work with Meshtastic devices |
| Web Browser | Used to access the OpenTAK Server Web UI |

# System Overview

- This setup creates a portable OpenTAK Server network that can be used for local communication, tracking, and field testing. The Raspberry Pi 5 runs the OpenTAK Server, while the mini wireless router creates the local network that all devices connect to. ATAK devices, such as Android phones or tablets, connect to the OpenTAK Server through the router. Once connected, users can share location data and communicate through the ATAK system. Meshtastic devices can also be added to support mesh communication when direct network coverage is limited.

## How It Connects
```text
Battery Bank
     ↓
Raspberry Pi 5
     ↓
OpenTAK Server
     ↓
Mini Wireless Router
     ↓
ATAK Phones / Tablets

Meshtastic Device
     ↓
Meshtastic ATAK Plugin
     ↓
ATAK
     ↓
OpenTAK Server
```
| Part                  | Role                                                    |
| --------------------- | ------------------------------------------------------- |
| Raspberry Pi 5        | Hosts the OpenTAK Server                                |
| Mini Wireless Router  | Provides the local network                              |
| Battery Bank          | Powers the portable setup                               |
| ATAK Device           | Connects users to the server and displays map/team data |
| Meshtastic Device     | Supports mesh communication                             |
| OpenTAK Server Web UI | Used to manage users, settings, and server access       |

# Setting Up OpenTAK Server
```text
# Step 1: Flash the Raspberry Pi:
Download an image of **Ubuntu Server** flash it to the Raspberry Pi 5. Using the **Raspberry Pi Imager** to flash the operating system onto the SD card.

# Step 2: Install OpenTAK Server:
After the Raspberry Pi boots, open a terminal and run the correct command for your operating system.

curl https://i.opentakserver.io/ubuntu_installer -Ls | bash -

# Step 3: Answer the Installer Prompts
During the installation, the installer may ask if you want to install **ZeroTier**.
For this setup, select:
"No"
For the other installation prompts, select:
"Yes"

# Step 4: Open the OpenTAK Server Web UI
After OpenTAK Server is installed, open a web browser and enter the Raspberry Pi IP address.
Example:
"http://RASPBERRY_PI_IP" Replace `RASPBERRY_PI_IP` with the actual IP address of your Raspberry Pi. This should open the OpenTAK Server management Web UI.

# Step 5: Log In and Change the Default Password
Use the default login:
Username: administrator
Password: password
After logging in, change the default password.
To change the password:
1. Go to **Admin**.
2. Go to **Users**.
3. Select the administrator account.
4. Click **Reset Password**.
5. Create a new password.

# Step 6: Enable Meshtastic in the Config File
After logging in and changing the password, open a terminal on the Raspberry Pi.
Go to the OpenTAK Server folder:
cd ots/

Open the configuration file:
nano config.yml

Find this setting:
OTS_ENABLE_MESHTASTIC: false
Change it to:
OTS_ENABLE_MESHTASTIC: true

Then find this setting:
OTS_MESHTASTIC_TOPIC: opentakserver
You can keep the default topic or change it to your own topic name. Make sure this topic matches the Meshtastic MQTT topic you use later.
Save and exit:
CTRL + O
ENTER
CTRL + X

Reboot the Raspberry Pi:
sudo reboot now
After the reboot, OpenTAK Server should be ready for Meshtastic configuration.
```

# Flashing the Meshtastic Device

```text
# Step 1: Download the USB Drivers

Go to the Meshtastic ESP32 serial driver page:
https://meshtastic.org/docs/getting-started/serial-drivers/esp32/

Download both drivers: -Note If using Linux or Mac just follow their instruction
- **CP210X USB to UART Bridge Driver**
- **CH9102 Driver for Windows**
> **Important:** Extract both driver downloads into one folder. This makes it easier to select the correct driver folder later.

# Step 2: Connect the Meshtastic Device
Connect the Meshtastic device to your computer using a USB cable.

On Windows, open:
Control Panel > Hardware and Sound > Devices and Printers
Look under **Unspecified** for the Meshtastic device.
Then open **Device Manager** and find the device there.

# Step 3: Update the Device Driver
After finding the device in Device Manager:

1. Right-click the device.
2. Select **Update driver**.
3. Select **Browse my computer for drivers**.
4. Click **Browse**.
5. Select the folder where both drivers were extracted.
6. Click **Next** and let Windows install the driver.

# Step 4: Flash Meshtastic Firmware

Open the Meshtastic web flasher:
https://flasher.meshtastic.org
Follow the instructions on the website to flash the correct firmware onto your Meshtastic device.

After flashing is complete, the device should be ready for setup in the Meshtastic mobile app.

```





## Server Diagram

The diagram below shows how the system connects together. The Raspberry Pi runs OpenTAK Server, the wireless router provides the local network, the battery bank powers the portable setup, and ATAK devices connect through the network.

![Server Diagram](server-diagram.png)
