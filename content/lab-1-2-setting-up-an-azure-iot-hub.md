# Lab 1.2 - Setting Up an Azure IoT Hub

In this lab you will provision a new Azure IoT Hub. Once you have the IoT Hub created, you will be able to create a new Azure IoT device that you will use to send telemetry to Azure. Azure IoT devices are software references to physical devices.

# Setup an Azure IoT Hub
In a browser, navigate to the Azure Portal at [https://portal.azure.com](https://portal.azure.com){:target="_blank"}. Login to your Azure account (if you don't have one, register for a [free trial](https://azure.microsoft.com/en-us/pricing/free-trial/){:target="_blank"}). Once logged in:

1. Click on the __New__ menu option in the upper-left.
2. Select __Internet of Things__.
3. Select __Azure IoT Hub__.
4. Give it a name. (e.g. your name followed by _iot-labs_). 
5. Select or create a new __Resource Group__.
6. Select a location. Choose the one closest to your physical location.

![Create an IoT Hub](/images/RPi3/RPi3_New-IoT-Hub.png)
  
Once the IoT Hub is created, 

1. Navigate into the IoT Hub.
2. Click on the _key_ icon at the top of the blade.
3. In the next blade, click on the __iothubowner__ entry.
4. Copy the __Connection string-primary key__ to your clipboard.

![Get the IoT Hub Owner Connection String](/images/RPi3/RPi3_AzureIoTConnectionString.png)

Azure IoT Hub only allows connections from known devices that present proper credentials. In this lab you will use either the _DeviceExplorer_ utility or the _iothub-explorer_ command line interface to provision a device for use in Azure IoT Hub. Although Azure IoT Hub supports multiple authentication schemes, you will use pre-shared keys in this lab.

## Option 1: Use Azure IoT Hub DeviceExplorer (Windows Only)
The simplest way to provision a new device is with the _DeviceExplorer_ utility. _DeviceExplorer_ is only available in Windows. 

1. Download and run [Device Explorer][deviceexplorer]{:target="_blank"}. 
2. After running the installer, the _DeviceExplorer.exe_ can be found at __C:\Program Files (x86)\Microsoft\DeviceExplorer__. 
3. Run _DevicExplorer.exe_.
4. Open the _Configuration_ tab.
5. Paste the _iothubowner_ connection string from the previous section of this lab in the _IoT Hub Connection String_ field.
6. Click __Update__.

![DeviceExplorer](/images/RPi3/RPi3_deviceexplorer01.png)

You'll now create a new Azure IoT Device.

1. Switch to the _Management_ tab.
3. Click the _Create_ button.
4. In the dialog that opens, enter a name for your IoT device. This can be the same name as your Raspberry Pi 3, or it can be different.
5. Click the _Create_ button, and click _Done_ on the confirmation dialog that opens.

![Create a new virtual device](/images/RPi3/RPi3_deviceexplorer02.png) 

You will see your device in the _Devices_ list. Once a device is created, you can get the device-specific connection string.

1. Select your device in the _Devices_ list
2. Right-click and select __Copy connection string for selected device__:

![Get the device-specific connection string](/images/RPi3/RPi3_deviceexplorer03.png) 

## Option 2: Use the IoT Hub Explorer Command Line Interface (Cross Platform)
If you're not using Windows, or if you prefer to use a command line interface, you can install the _iothub-explorer_ command line interface. The iothub-explorer tool enables you to provision devices in your IoT hub. It runs on any computer where Node.js is available. You can install Node.js from [NodeJS.org](https://nodejs.org){:target="_blank"}.

1. Open a command prompt and type the following:

<pre>
  npm install -g iothub-explorer
</pre>

2. After _iothub-explorer_ is installed, execute the following, replacing <code>[connection-string]</code> with your <i>iothubowner</i> connection string (from the previous lab) and <code>[device-id]</code> with the Azure IoT Hub id for this device.

<pre>
  iothub-explorer [YOUR IOT HUB CONNECTION STRING] create [YOUR DEVICE NAME] --connection-string
</pre>

3. Copy the device-specific connection string.

## Conclusion &amp; Next Steps
Congratulations! You have created an Azure IoT Hub that you will connect devices to. You also created a representation of a physical device in your IoT Hub. In the next lab you will build a Universal Windows Application that will collect data from the RPi3 and send it to Azure IoT Hub.

In the [next lab][nextlab] you will send telemetry to the cloud through Azure IoT Hub.

---

Back to [Lab 1 - Windows IoT](/content/lab-1-windows-iot.md)

Back to [IoT Labs homepage](/readme.md)

---

*[9gag:](http://9gag.com/) Sorry for  the long post, here's a [potato](https://www.quora.com/What-does-Sorry-for-the-long-post-heres-a-potato-mean-in-9GAG):*

![9gag Potato](/images/potato04.jpg)

[nextlab]: /content/lab-1-3-sending-telemetry-to-the-cloud.md
[deviceexplorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md
