# Lab 1.2 - Setting Up an Azure IoT Hub

In this lab you will provision a new Azure IoT Hub. Once you have the IoT Hub created, you will be able to create a new Azure IoT device that you will use to send telemetry to Azure. Azure IoT devices are software references to physical devices.

# Setup an Azure IoT Hub
In a browser, navigate to the Azure Portal at [https://portal.azure.com](https://portal.azure.com). Use your Azure Pass or your Azure account. Once logged in:

1. Click on the **New** menu option in the upper-left.
2. Select **Internet of Things**.
3. Select **IoT Hub**.
4. Click on **Create** option on the right.
5. Give it a name. (e.g. your name followed by *iot-labs*). 
6. Select or create a new **Resource Group**.
7. Select a location. Choose the one closest to your physical location (ex: North Europe).

![Create an IoT Hub](/images/lab2_rpi2-new-iot-hub.png)
  
Once the IoT Hub is created, 
 
1. Navigate into the IoT Hub.
2. Click on the *key* icon (Shared Access Policies) at the top of the blade.
3. If no Policies appear, wait a little while (they are the last thing to be created)
4. In the next blade, click on the **iothubowner** entry.
5. Copy the **Connection string-primary key** to your clipboard.

![Get the IoT Hub Owner Connection String](/images/lab1_rpi2-azure-iot-connection-string.png)

Azure IoT Hub only allows connections from known devices that present proper credentials. In this lab you will use either the *DeviceExplorer* utility or the *iothub-explorer* command line interface to provision a device for use in Azure IoT Hub. Although Azure IoT Hub supports multiple authentication schemes, you will use pre-shared keys in this lab.

## Option 1: Use Azure IoT Hub DeviceExplorer (Windows Only)
The simplest way to provision a new device is with the *DeviceExplorer* utility. *DeviceExplorer* is only available in Windows. 

1. Download and run [Device Explorer][deviceexplorer]. 
2. After running the installer, the *DeviceExplorer.exe* can be found at **C:\Program Files (x86)\Microsoft\DeviceExplorer**. 
3. Run *DevicExplorer.exe*.
4. Open the *Configuration* tab.
5. Paste the *iothubowner* connection string from the previous section of this lab in the *IoT Hub Connection String* field.
6. Click **Update**.

![DeviceExplorer](/images/lab1_rpi2-deviceexplorer01.png)

You'll now create a new Azure IoT Device.

1. Switch to the *Management* tab.
3. Click the *Create* button.
4. In the dialog that opens, enter a name for your IoT device. This can be the same name as your Raspberry Pi 3, or it can be different.
5. Click the *Create* button, and click *Done* on the confirmation dialog that opens.

![Create a new virtual device](/images/lab2_rpi2-deviceexplorer02.png) 

You will see your device in the *Devices* list. Once a device is created, you can get the device-specific connection string.

1. Select your device in the *Devices* list
2. Right-click and select **Copy connection string for selected device**:

![Get the device-specific connection string](/images/lab2_rpi2-deviceexplorer03.png) 

## Option 2: Use the IoT Hub Explorer Command Line Interface (Cross Platform)
If you're not using Windows, or if you prefer to use a command line interface, you can install the *iothub-explorer* command line interface. The iothub-explorer tool enables you to provision devices in your IoT hub. It runs on any computer where Node.js is available. You can install Node.js from [NodeJS.org](https://nodejs.org).

1. Open a command prompt and type the following:

<pre>
  npm install -g iothub-explorer
</pre>

2. After *iothub-explorer* is installed, execute the following, replacing <code>[connection-string]</code> with your <i>iothubowner</i> connection string (from the previous lab) and <code>[device-id]</code> with the Azure IoT Hub id for this device.

<pre>
  iothub-explorer [YOUR IOT HUB CONNECTION STRING] create [YOUR DEVICE NAME] --connection-string
</pre>

3. Copy the device-specific connection string.

## Conclusion &amp; Next Steps
Congratulations! You have created an Azure IoT Hub that you will connect devices to. You also created a representation of a physical device in your IoT Hub. In the next lab you will build a Universal Windows Application that will collect data from the RPi3 and send it to Azure IoT Hub.

In the **[next lab][nextlab]** you will send telemetry to the cloud through Azure IoT Hub.

---

Back to [Lab 1 - Windows IoT](/content/lab-1-windows-iot.md)

Back to [IoT Labs homepage](/readme.md#labs)

---

*[9gag:](http://9gag.com/) Sorry for  the long post, here's a [potato](https://www.quora.com/What-does-Sorry-for-the-long-post-heres-a-potato-mean-in-9GAG):*

![9gag Potato](/images/potato04.jpg)

[nextlab]: /content/lab-1-3-sending-telemetry-to-the-cloud.md
[deviceexplorer]: https://github.com/Azure/azure-iot-sdks/releases
