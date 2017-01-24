#Lab - Configure your device and get the tools

### What you will do
Configure Pi for first-time use and install the Raspbian operating system. Raspbian is a free operating system that is optimized for the Raspberry Pi hardware. 
Download the development tools and the software for the first sample application for Raspberry Pi 3. 

### What you will learn
In this article, you will learn:

* How to install Raspbian on Pi.
* How to power up Pi by using a USB cable.
* How to connect Pi to the network by using an Ethernet cable or wireless network.
* How to add an LED to the breadboard and connect it to Pi.

* How to install Git and Node.js.
  * [Git](https://git-scm.com/) is an open source distributed version control system. The sample application for this article is stored on Git.
  * [Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.
* How to use NPM to install additional Node.js development tools.
  * The minimum version requirement of Node.js is 4.5 LTS.
  * [NPM](https://www.npmjs.com/) is one of the package managers for Node.js.

### What you will need
To complete this operation, you need the following parts from your Raspberry Pi 3 Starter Kit:

* The Raspberry Pi 3 board
* The 16-GB microSD card
* The 5-volt 2-amp power supply with the 6-foot micro USB cable
* The breadboard
* Connector wires
* A 560-ohm resistor
* A diffused 10-mm LED
* The Ethernet cable

![Lab2 Material][lab2-material]

You also need:

* A wired or wireless connection for Pi to connect to.
* A USB-SD adapter or miniSD card to burn the operating system image onto the microSD card.
* A computer running Windows, Mac, or Linux. The computer is used to install Raspbian on the microSD card.
* An Internet connection to download the necessary tools and software.

### Install Raspbian on the microSD card
Prepare the microSD card for installation of the Raspbian image.

1. [Download](https://www.raspberrypi.org/downloads/raspbian/) Raspbian.
  1. Download the .zip file for Raspbian Jessie with Pixel.
  2. Extract the Raspbian image to a folder on your computer.
2. Install Raspbian to the microSD card.
  1. [Download](https://www.etcher.io/) and install the Etcher SD card burner utility.
  2. Run Etcher and select the Raspbian image that you extracted in step 1.
  3. Select the microSD card drive. Note that Etcher may have already selected the correct drive.
  4. Click Flash to install Raspbian to the microSD card.
  5. Remove the microSD card from your computer when installation is complete. It's safe to remove the microSD card directly because Etcher automatically ejects or unmounts the microSD card upon completion.
  6. Insert the microSD card into Pi.

![Lab2 Material][lab2-sdcard]

### Turn on Pi
Turn on Pi by using the micro USB cable and the power supply.

![Lab2 Material][lab2-power]

> It is important to use the power supply in the kit that is at least 2A to make sure that your Raspberry has enough power to work correctly.

### Connect Raspberry Pi 3 to the network
You can connect Pi to a wired network or to a wireless network. Make sure that Pi is connected to the same network as your computer. For example, you can connect Pi to the same switch that your computer is connected to.

#### Connect to a wired network
Use the Ethernet cable to connect Pi to your wired network. The two LEDs on Pi turn on if the connection is established.

![Lab2 Material][lab2-connect-ethernet]

#### Connect to a wireless network
Follow the [instructions](https://www.raspberrypi.org/learning/software-guide/wifi/) from the Raspberry Pi Foundation to connect Pi to your wireless network. These instructions require you to first connect a monitor and a keyboard to Pi.

### Connect the LED to Pi
To complete this task, use the [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), the connector wires, the LED, and the resistor. Connect them to the [general-purpose input/output](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO) ports of Pi.

![Lab2 Material][lab2-breadboard]

* Connect the shorter leg of the LED to GPIO GND (Pin 6).
* Connect the longer leg of the LED to one leg of the resistor.
* Connect the other leg of the resistor to GPIO 4 (Pin 7).

> that the LED polarity is important. This polarity setting is commonly known as Active Low.

![Lab2 Material][lab2-pinout-breadboard]

**Congratulations!** You've successfully configured Pi.
Now, lets get the tools!

### [Install Git and Node.js](#install-git-note)
Click the following links to download and install Git and Node.js LTS for Windows.

* [Get Git for Windows](https://git-scm.com/download/win/)
* [Get Node.js LTS for Windows](https://nodejs.org/en/)

### Install additional Node.js development tools
Use [gulp.js](http://gulpjs.com/) to automate the deployment of the sample application to Pi. You also use the [device-discovery-cli](https://github.com/Azure/device-discovery-cli) to retrieve network information about your IoT devices.

Start a command prompt as an administrator. Install **gulp** and **device-discovery-cli** by running the following command:

´´´
npm install -g device-discovery-cli gulp
´´´

### Install Visual Studio Code
[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code. Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS. You use this editor later in the tutorial to edit the sample code.

### Summary
In this article, you’ve learned how to configure Pi by installing Raspbian, connecting Pi to a network, and connecting an LED to Pi. Note that the LED doesn't yet light up. The next task is to install the necessary tools and software in preparation for running a sample application on Pi.

You've installed the required development tools and software for the first sample application. The next task is to create, deploy, and run the sample application on Pi.

![Lab2 Hardware Ready][lab2-hardware-ready]

---

Back to [Lab 2 - IoT with Linux and Node.js](/content/lab-2-linux-node-iot.md)

Back to [IoT Labs homepage](/readme.md#labs)

---

*[9gag:](http://9gag.com/) Sorry for  the long post, here's a [potato](https://www.quora.com/What-does-Sorry-for-the-long-post-heres-a-potato-mean-in-9GAG):*

![9gag Potato](/images/potato06.jpg)

[lab2-material]: ./images/lab2_starter-kit.jpg "Lab 2 Material"
[lab2-sdcard]: ./images/lab2_sdcard.jpg "Lab 2 Insert SD Card"
[lab2-power]: ./images/lab2-power.jpg "Lab 2 Turn on you Rpi3"
[lab2-connect-ethernet]: ./images/lab2_connect-ethernet.jpg "Lab 2 Connect ethernet"
[lab2-breadboard]: ./images/lab2_breadboard-led-resistor.jpg "Lab 2 Breadboard"
[lab2-pinout-breadboard]: ./images/lab2_pinout-breadboard.png "Lab 2 Pinout"
[lab2-hardware-ready]: ./images/lab2_hardware-ready.jpg "Lab 2 Hardware Ready"


